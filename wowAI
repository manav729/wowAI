import openai
from gtts import gTTS
from playsound import playsound
import speech_recognition as sr
from mtranslate import translate
import os
openai.api_key = "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"
model_engine = "text-davinci-003"


def listen_to_audio():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        audio = recognizer.listen(source)

    try:
        user_input = recognizer.recognize_google(audio)
        print(f"You: {user_input}")
        return user_input
    except sr.UnknownValueError:
        print("Sorry, I couldn't understand what you said.")
        return None

import datetime

def speak_text(text):
    timestamp = datetime.datetime.now().strftime("%Y%m%d-%H%M%S")
    filename = f"response_{timestamp}.mp3"
    tts = gTTS(text=text, lang='en', slow=False, tld='com')
    tts.save(filename)
    if os.path.exists(filename):
        playsound(filename)
        os.remove(filename)
    else:
        print("Error: File not found.")


def generate_response(user_input, language):
    completions = openai.Completion.create(
        engine=model_engine,
        prompt=user_input,
        max_tokens=1024,
        n=1,
        stop=None,
        temperature=0.5,
    )
    message = completions.choices[0].text

    if language != "":
        message = translate(message, language)

    return message

def chatbot():
    print("Chatbot: Hello, I am your chatbot, how would you like to input your message?")
    speak_text(
        "Hello, I am your chatbot, how would you like to input your message? Please type 'text' for text input or 'voice' for voice input.")

    input_method = input().lower()

    while input_method not in ["text", "voice"]:
        print(
            "Chatbot: I'm sorry, I didn't understand your input method. Please type 'text' for text input or 'voice' for voice input.")
        speak_text(
            "I'm sorry, I didn't understand your input method. Please type 'text' for text input or 'voice' for voice input.")
        input_method = input().lower()

    if input_method == "text":
        print("Chatbot: You have chosen text input.")
        speak_text("You have chosen text input.")

    if input_method == "voice":
        print("Chatbot: You have chosen voice input.")
        speak_text("You have chosen voice input.")

    print(
        "Chatbot: Which language would you like the output to be translated to? Please enter a language code (e.g. 'es' for Spanish) or leave blank for no translation.")
    speak_text(
        "Which language would you like the output to be translated to? Please enter a language code or leave blank for no translation.")

    language = input().lower()
    if language != "":
        print("Chatbot: Output will be translated to " + language)
        speak_text("Output will be translated to " + language)

    print("Chatbot: What can I do for you?")
    speak_text("What can I do for you?")

    while True:
        if input_method == "text":
            user_input = input("You: ")
            if user_input == "":
                continue
        else:
            user_input = listen_to_audio()

        if user_input.lower() in ["goodbye", "bye", "see you later", "stop"]:
            print("Chatbot: Goodbye!")
            speak_text("Goodbye!")
            os.remove("response.mp3")
            return

        response = generate_response(user_input, language)
        print("Chatbot: " + response)
        speak_text(response)

        # Check if the user pressed the 'n' key to stop audio and take another input
        key_press = input("Press 'n' to take another input: ")
        if key_press.lower() == 'n':
            if input_method == "text":
                continue

        else:
            os.remove("response.mp3")
            speak_text("Please say your next input.")
            continue


if __name__ == "__main__":
    chatbot()
