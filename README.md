import boto3
import tkinter as tk
from tkinter import filedialog, messagebox
import os

# Initialize AWS Polly client
polly_client = boto3.client('polly')

def convert_text_to_speech():
    text = text_entry.get("1.0", "end").strip()
    if not text:
        messagebox.showerror("Error", "Please enter some text.")
        return
    
    try:
        response = polly_client.synthesize_speech(
            Text=text,
            OutputFormat='mp3',
            VoiceId='Joanna'  # You can change the voice here
        )
        output_path = filedialog.asksaveasfilename(
            defaultextension=".mp3",
            filetypes=[("MP3 files", "*.mp3")],
            title="Save the MP3 file"
        )

        if output_path:
            with open(output_path, 'wb') as file:
                file.write(response['AudioStream'].read())
            messagebox.showinfo("Success", f"Audio saved at {output_path}")
    except Exception as e:
        messagebox.showerror("Error", f"Failed to convert text: {e}")

# GUI Initialization
root = tk.Tk()
root.title("Text-to-Speech Converter")

# Text Entry Field
tk.Label(root, text="Enter Text:").pack(pady=5)
text_entry = tk.Text(root, height=10, width=50)
text_entry.pack(pady=10)

# Convert Button
convert_button = tk.Button(root, text="Convert to Speech", command=convert_text_to_speech)
convert_button.pack(pady=10)

# Run the Tkinter Event Loop
root.mainloop()
