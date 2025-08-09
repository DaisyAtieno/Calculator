import tkinter as tk
import winsound  # For beep sound (Windows only)

# Create the main window
root = tk.Tk()
root.title("Basic Calculator")
root.geometry("300x400")
root.configure(bg="#003366")  # Dark blue background

expression = ""

# Function to play a beep
def beep():
    winsound.Beep(1000, 100)  # (frequency in Hz, duration in ms)

# Function to update expression
def press(num):
    global expression
    beep()
    expression += str(num)
    equation.set(expression)

# Function to evaluate final expression
def equalpress():
    global expression
    try:
        beep()
        total = str(eval(expression))
        equation.set(total)
        expression = total
    except:
        equation.set("Error")
        expression = ""

# Function to clear the input
def clear():
    global expression
    beep()
    expression = ""
    equation.set("")

# Text input field
equation = tk.StringVar()
entry_field = tk.Entry(root, textvariable=equation, font=('Arial', 20), bg='white', fg='black', bd=10, relief='sunken')
entry_field.grid(columnspan=4, ipadx=8, ipady=25, padx=10, pady=10)

# Button config
button_config = {
    "padx": 20,
    "pady": 20,
    "bd": 4,
    "fg": "black",
    "bg": "#cce6ff",
    "font": ('Arial', 14)
}

# Button layout
buttons = [
    ('7', 1, 0), ('8', 1, 1), ('9', 1, 2), ('/', 1, 3),
    ('4', 2, 0), ('5', 2, 1), ('6', 2, 2), ('*', 2, 3),
    ('1', 3, 0), ('2', 3, 1), ('3', 3, 2), ('-', 3, 3),
    ('0', 4, 0), ('.', 4, 1), ('+', 4, 2), ('=', 4, 3),
    ('C', 5, 0)
]

# Create buttons dynamically
for (text, row, col) in buttons:
    action = lambda x=text: press(x) if x not in ['=', 'C'] else (equalpress() if x == '=' else clear())
    tk.Button(root, text=text, command=action, **button_config).grid(row=row, column=col, padx=5, pady=5, sticky='nsew')

# Make layout responsive
for i in range(6):
    root.grid_rowconfigure(i, weight=1)
for j in range(4):
    root.grid_columnconfigure(j, weight=1)

root.mainloop()
