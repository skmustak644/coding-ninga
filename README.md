# coding-ninga
#AI base Assistent
import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import webbrowser
import os
import smtplib
import pyaudio
import pyautogui

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
# print(voices[1].id)
engine.setProperty('voice', voices[1].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        speak("Hellooo  sir.. Gooood. Morning!")

    elif hour>=12 and hour<18:
        speak("Hellooo sir Gooood Afternoon!")   

    elif hour>=18 and hour<21:
        speak("Helloooo sir Gooood Evening!")

    else: 
        speak("Hellooo Sir Goood Night")

    speak("I am Bronika Sir. Please tell me how may I help you")       

def takeCommand():
    #It takes microphone input from the user and returns string output

    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("Recognizing...")    
        query = r.recognize_google(audio, language='en-in')
        print(f"User said: {query}\n")

    except Exception as e:
        # print(e)    
        print("Say that again please...")  
        return "None"
    return query

def sendEmail(to, content): #cammand
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('skmustak644@gmail.com', 'yuor password')
    server.sendmail('skmustak644@gmail.com', to, content)
    server.close()

if __name__ == "__main__":
    wishMe()
    while True:
    # if 1:
        query = takeCommand().lower()

        # Logic for executing tasks based on query
        if 'wikipedia' in query:
            speak('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=2)
            speak("According to Wikipedia")
            print(results)
            speak(results)

        elif 'hi ' in query or 'hello' in query:
            speak("hello sir, how can i help you?")
        
        elif 'open youtube' in query:
            speak("Ok Sir")
            webbrowser.open("youtube.com")

        elif 'open google' in query:
            webbrowser.open("google.com")
        
        elif 'who are you' in query:
            speak("i am Bronika sir")


        elif 'play music' in query:
            spotify="C:\\Users\\skmus\\OneDrive\\Desktop\\Spotify.lnk"    
            os.startfile(spotify)
            pyautogui.press('playpause')

        elif 'what is the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")    
            speak(f"Sir, the time is {strTime}")

        elif 'open whatsapp' in query:
            codePath = "C:\\Users\\skmus\\OneDrive\\Desktop\\WhatsApp.lnk"
            os.startfile(codePath)
        elif 'quit' in query or 'bye' in query:
            speak("OK, sir. I am now exiting the program. Thank you for using me. Goodbye!")
            break
            



        elif 'send email' in query:
            try:
                speak("What should I say?")
                content = takeCommand()
                speak("To whome you want to sent?")
                to ="sksaklinmustak2017@gmail.com"   
                sendEmail(to, content)
                speak("Email has been sent!")
            except Exception as e:
                print(e)
                speak("Sorry my friend Saklin bhai. I am not able to send this email")



