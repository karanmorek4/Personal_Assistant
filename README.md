# Personal_Assistant

import pyttsx3
import datetime
import speech_recognition as sr
import wikipedia 
import webbrowser
import os
import random
import pywhatkit
from googletrans import Translator

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
# print(voices[1].id)
engine.setProperty('voice',voices[2].id)

def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def wishMe():
    hour = int (datetime.datetime.now().hour)
    if hour >= 0 and hour<= 12:
        speak("Good Morning ")
    elif hour>=12 and hour <=16:
        speak("Good Afternoon ")
    elif hour>=16 and hour <=24 :
        speak("Good Evening ")
    

def address():
        speak("Whats your good name ?")
        goodName=takeCommand()
        speak(f" Oh Hi {goodName}!! I'm your personal assistant Gidddi")
        return goodName

def takeCommand():
    # it takes microphone input and return string output
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listing ...")
        r.pause_threshold = 1 
        audio = r.listen(source)
    
    try:
        print("Recongnizing ...")
        query = r.recognize_google(audio,language='en-in')
        print(f"User said : {query}\n")
    
    except Exception as e:
        # print(e)
        print("Say That Again ...")
        return "None"
    return query


if __name__=="__main__":
    
    speak("Hello !!")
    speak("How can I help you ?")
    # address()
    # wishMe()

    while True:
        query = takeCommand().lower()
    
        #logic for executing tasks  based on query
        if 'wikipedia' in query:
            speak("Searching wikipedia ...")
            query = query.replace("wikipedia","")
            results = wikipedia.summary(query,sentences = 2)
            speak("Acording To Wikipedia ")
            print(results)
            speak(results)
        elif 'youtube' in query:
            webbrowser.open("https://www.youtube.com/?gl=IN&tab=r11")
        elif 'open google' in query:
            webbrowser.open("https://www.google.co.in/")
        elif 'open linkedin' in query:
            webbrowser.open("https://www.linkedin.com/feed/")
        elif 'open whatsapp' in query:
            path_WA = "C:\\Users\\Karan\\AppData\\Local\\WhatsApp\\WhatsApp.exe"
            os.startfile(path_WA)
        elif 'music' in query:
            musicDir = 'G:\\Music'
            songs = os.listdir(musicDir)
            s1=random.randint(0,479)
            # print(songs)
            os.startfile(os.path.join(musicDir,songs[s1]))
        elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"Hey , the time is {strTime}")
        elif 'vs code' in query:
            codePath = "C:\\Users\\Karan\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe"
            os.startfile(codePath)
        elif 'open gmail' in query:
            webbrowser.open("https://accounts.google.com/b/0/AddMailService")
        elif 'play' in query:
            yt=query.replace('play','')
            speak(f"playing {yt}")
            print(f"playing {yt}")
            pywhatkit.playonyt(yt)
        elif 'exit' in query:
            speak("Thank You for using me !!")
            break
