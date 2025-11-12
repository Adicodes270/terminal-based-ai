# Detailed Documentation for FRIDAY.py

`FRIDAY.py` is a Python script that acts as an artificial intelligence assistant, combining several functionalities including voice recognition, text-to-speech, and interaction with external APIs for news and weather. Below is a comprehensive breakdown of the code, its structure, functionalities, and the libraries used.

## Overview

The script leverages the following libraries:

- **google.genai**: For generating responses using Google DeepMind's Gemini AI model.
- **pyttsx3**: A text-to-speech conversion library.
- **threading**: To manage concurrent operations, particularly for voice recognition.
- **speech_recognition**: A library used for recognizing speech through the microphone.
- **faster_whisper**: For real-time transcription of audio.
- **requests**: A library to make HTTP requests to external APIs.
- **logging**: For logging events, errors, and information.
- **tempfile** and **os**: Used for creating temporary files and directories.
- **queue**: To manage responses and communication between threads.

## Logging Configuration

The script initializes a logging system, storing logs in a file named `data.log`. It records events with timestamps and log levels (INFO, ERROR).

```python
LOG_FILE = "data.log"

logging.basicConfig(
    filename=LOG_FILE,
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)
```

## Text-To-Speech Configuration

The text-to-speech engine is initialized and configured for a specific speech rate and volume:

```python
engine = pyttsx3.init()
engine.setProperty('rate', 150)
engine.setProperty('volume', 0.9)
```

## Global State Variables

Several global state variables are defined to manage the application's behavior:
- `is_listening`: A flag to indicate if the application is currently listening for voice input.
- `listening_thread`: A reference to the thread handling real-time voice recognition.
- `is_voice_mode`: Indicates if the voice output is enabled.
- `is_clear_mode`: Determines whether the terminal should be cleared after each response.
- `exit_ai`: A flag to exit the main loop and terminate the application.
- `stop_speaking`: A flag to control the speaking behavior of the application.

## Core Functions

### Auxiliary Functions

Any ancillary function enhances the functionality:

1. **clear_terminal()**: Clears the console output.
2. **delete_log()**: Deletes the log file and reinitializes the logger.
3. **get_live_news()**: Fetches latest news headlines from the News API.
4. **get_weather(location)**: Retrieves the current weather conditions for a specified location using the wttr.in service.
5. **type_text_slowly(text, delay)**: Simulates typing in the console, outputting text character by character.
6. **speak_text(text)**: Uses the TTS engine to speak a given text.
7. **stop_voice_playback()**: Stops the TTS engine if it is currently speaking.
8. **chatbot_response(prompt, voice_mode, clear_mode)**: Generates responses based on user prompts, using the AI model or fetching weather/news as appropriate.

### Voice Recognition Function

- **transcribe_realtime_whisper()**: Listens for real-time audio input through the microphone, transcribing it using the Whisper model. It handles microphone access and audio processing by saving audio data to a temporary file.

### Main Function

- **main()**: The central control hub of the application which manages user interactions through a menu. It handles different command inputs, updates the AI's state (like enabling voice recognition), and processes user prompts.

### Error Handling

Various try-except blocks are used throughout the application to log and handle exceptions, ensuring that the application remains robust and can report errors gracefully.

## Main Execution

The script execution starts from the `if __name__ == "__main__":` block, where it calls the `main()` function inside a try-except to handle keyboard interrupts gracefully.

```python
if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("\nExiting due to keyboard interrupt...")
        logging.info("AI session exited by keyboard interrupt.")
        if listening_thread and listening_thread.is_alive():
            is_listening = False
            listening_thread.join()
        sys.exit(0)
```

## Conclusion

`FRIDAY.py` integrates multiple functionalities in a cohesive system, allowing for interactive AI-driven conversations through voice and text. The careful management of user inputs, along with error handling and logging, ensures that the assistant is both user-friendly and resilient.

