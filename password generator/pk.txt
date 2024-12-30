import random
import string
import tkinter as tk
from tkinter import messagebox

# Function to generate password
def generate_password():
    try:
        length = int(length_entry.get())
        if length < 4:
            raise ValueError("Password length must be at least 4 for a secure mix.")
        
        # Character pools
        uppercase = string.ascii_uppercase
        lowercase = string.ascii_lowercase
        digits = string.digits
        special_chars = string.punctuation
        
        # Ensuring the password includes at least one of each type
        all_chars = uppercase + lowercase + digits + special_chars
        password = [
            random.choice(uppercase),
            random.choice(lowercase),
            random.choice(digits),
            random.choice(special_chars),
        ]
        # Fill the remaining length with random choices
        password += random.choices(all_chars, k=length - 4)
        random.shuffle(password)
        
        password_str = ''.join(password)
        password_entry.delete(0, tk.END)
        password_entry.insert(0, password_str)
    except ValueError as ve:
        messagebox.showerror("Error", str(ve))

# Function to copy password to clipboard
def copy_to_clipboard():
    password = password_entry.get()
    if password:
        root.clipboard_clear()
        root.clipboard_append(password)
        root.update()
        messagebox.showinfo("Copied", "Password copied to clipboard!")
    else:
        messagebox.showerror("Error", "No password to copy!")

# Setting up the tkinter window
root = tk.Tk()
root.title("Secure Password Generator")

# UI Elements
frame = tk.Frame(root, padx=10, pady=10)
frame.pack(padx=10, pady=10)

tk.Label(frame, text="Password Length:").grid(row=0, column=0, sticky="w", pady=5)
length_entry = tk.Entry(frame, width=10)
length_entry.grid(row=0, column=1, pady=5)

tk.Button(frame, text="Generate Password", command=generate_password).grid(row=1, column=0, columnspan=2, pady=10)
tk.Label(frame, text="Generated Password:").grid(row=2, column=0, sticky="w", pady=5)

password_entry = tk.Entry(frame, width=40)
password_entry.grid(row=2, column=1, pady=5)

tk.Button(frame, text="Copy to Clipboard", command=copy_to_clipboard).grid(row=3, column=0, columnspan=2, pady=10)

# Main loop
root.mainloop()
