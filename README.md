# Voice Based Mail for Blind 




## Features



## Requirements
- Python 
- Required Python packages: Speech Recognition, Easyimap, Pyttsx3, Smtplib, Imaplib.

## Architecture Diagram/Flow

![image](https://github.com/Sandhyacharu/Voice-Based-Mail-for-Blind/assets/75235167/46523294-4137-4b83-97fe-5473b4f3e8d9)

## Installation



## Usage



## Program:
```python3

import speech_recognition as sr
import easyimap as e
import pyttsx3
import smtplib
import imaplib
import email
from email.header import decode_header

username = "voicebasedemail23@gmail.com"
password = "yzua rdge ajyi xnlk"

r = sr.Recognizer()
engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice',voices[1].id)
engine.setProperty('rate',150)

def speak(str):
    print(str)
    engine.say(str)
    engine.runAndWait()

def listen():
    with sr.Microphone() as source:
        r.adjust_for_ambient_noise(source)
        str = "Speak now:"
        speak(str)
        audio = r.listen(source)
        try:
            text = r.recognize_google(audio)
            return text
        except:
            str = "Sorry could not recognize what you said"
            speak(str)

def sendmail():
    str = "Please speak the receiver email id"
    speak(str)
    rec_email = listen()
    rec = rec_email.lower().replace(" ", "")
    str = "You have spoken the message"
    speak(str)
    speak(rec)

    str = "Please speak the subject of your email"
    speak(str)
    sub = listen()

    str = "You have spoken the message"
    speak(str)
    speak(sub)

    str = "Please speak the body of your email"
    speak(str)
    msg = listen()

    str = "You have spoken the message"
    speak(str)
    speak(msg)

    server = smtplib.SMTP_SSL("smtp.gmail.com",465)
    server.login(username, password)
    server.sendmail(username, rec, f"Subject: {sub}\n\n{msg}")
    server.quit()

    str = "The mail has been sent"
    speak(str)


def readmail():
    mail = imaplib.IMAP4_SSL("imap.gmail.com")
    mail.login(username, password)

    mail.select("inbox")

    status, messages = mail.search(None, "(UNSEEN)")

    if status == "OK":
        email_list = messages[0].split()
        speak(f"Number of unread emails: {len(email_list)}")

        for num in email_list:
            _, msg_data = mail.fetch(num, "(RFC822)")
            raw_email = msg_data[0][1]

            msg = email.message_from_bytes(raw_email)

            subject, encoding = decode_header(msg["Subject"])[0]
            subject = subject.decode(encoding) if encoding else subject
            sender = msg.get("From")
            body = ""

            if msg.is_multipart():
                for part in msg.walk():
                    if part.get_content_type() == "text/plain":
                        body = part.get_payload(decode=True).decode("utf-8")
                        break
            else:
                body = msg.get_payload(decode=True).decode("utf-8")
            text = "You have an email with the subject"
            speak(text)
            speak(subject)
            text = "From"
            speak(text)
            speak(sender)
            text = "The message is"
            speak(text)
            speak(body)

str = "Welcome to Voice Based Email Service"
speak(str)

while(1):
    str = "What do you want to do?"
    speak(str)

    str="Speak Send to send mail    Speak read to read inbox   Speak exit to exit "
    speak(str)

    ch = listen()

    if(ch == 'send'):
        str = "You have choosen to send an email"
        speak(str)
        sendmail()

    elif(ch == 'read'):
        str = "You have chosen to read email"
        speak(str)
        readmail()

    elif(ch=='exit'):
        str = "You have choosen to exit, bye bye"
        speak(str)
        exit(1)

    else:
        str = "Invalid choice"
        speak(str)
        speak(ch)
```

## Output:

### Reading an unread email form inbox
![image](https://github.com/Sandhyacharu/Voice-Based-Mail-for-Blind/assets/75235167/0e85841e-b722-4884-9a3a-2c399c91b25b)

![image](https://github.com/Sandhyacharu/Voice-Based-Mail-for-Blind/assets/75235167/ac4d3e7b-7980-48a4-ad99-a05e8406206b)

### Compose and sending a new mail
![image](https://github.com/Sandhyacharu/Voice-Based-Mail-for-Blind/assets/75235167/3e5efd6c-8d05-4882-b931-9a4d95639476)

![image](https://github.com/Sandhyacharu/Voice-Based-Mail-for-Blind/assets/75235167/a3cd5ea6-ca13-4a3f-84f5-219b32e6d635)

## Result:



