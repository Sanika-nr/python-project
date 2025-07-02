# python-project
from tkinter import *
import pyttsx3
import requests
from PIL import Image, ImageTk
# Initialize the speech engine
engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice',voices[0].id)
engine.setProperty('rate',150)

def speak(text):
    engine.say(text)
    engine.runAndWait()

def get_meaning():
    word = word_var.get()
    url = f"https://api.dictionaryapi.dev/api/v2/entries/en/{word}"
    try:
        response = requests.get(url)
        data = response.json()
        definition = data[0]['meanings'][0]['definitions'][0]['definition']
        meaning_var.set(definition)
        speak(f"The meaning of {word} is: {definition}")
    except Exception as e:
        meaning_var.set("No meaning found.")
        speak("No meaning found.")

root = Tk()
root.title("Speak the Meaning")
root.geometry("800x600")

# Load the background image
bg_image = Image.open(r"C:\Users\Shariff\OneDrive\画像\Saved Pictures\bgimage.jpg")  # Replace with your image file
bg_image = bg_image.resize((800,600))
bg_image = ImageTk.PhotoImage(bg_image)

# Create a label with the background image
bg_label = Label(root, image=bg_image)
bg_label.place(x=0, y=0, relwidth=1, relheight=1)

word_var = StringVar()
meaning_var = StringVar()

# Header label
header_label = Label(root, text="DICTIONARY ASSISTANT", bg='#87CEEB', fg='black', font=('Arial',20,'bold'))                                               
header_label.place(relx=0.5, rely=0.1, anchor=CENTER)

             
input_frame = Frame(root, bg='#87CEEB')
header_label.place(relx=0.5, rely=0.1, anchor=CENTER)

# Input frame
input_frame = Frame(root, bg='#87CEEB')
input_frame.place(relx=0.5, rely=0.3, anchor=CENTER)
Label(input_frame, text="Enter a word:", bg='#87CEEB',font=('Arial',14)).pack(side=LEFT,padx=5)                                                  
Entry(input_frame, textvariable=word_var, font=('Arial', 14), width=20).pack(side=LEFT, padx=5)

button_frame = Frame(root, bg='#87CEEB')
button_frame.place(relx=0.5, rely=0.5, anchor=CENTER)
Button(button_frame, text="Get Meaning", command=get_meaning, font=('Arial', 14, 'bold'), bg='green', fg='white').pack()

# Meaning label
meaning_label = Label(root, textvariable=meaning_var, bg='#87CEEB', wraplength=500, font=('Arial', 14))
meaning_label.place(relx=0.5, rely=0.7, anchor=CENTER)

root.mainloop()
