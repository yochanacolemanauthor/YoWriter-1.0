import tkinter as tk
from tkinter import filedialog, messagebox

# Create the main window
root = tk.Tk()
root.title("Writing Desk")
root.geometry("1000x700")
root.minsize(600, 400)

# Configure the window layout
root.columnconfigure(0, weight=1, uniform="columns")
root.columnconfigure(1, weight=4, uniform="columns")
root.rowconfigure(1, weight=4, uniform="rows")
root.rowconfigure(3, weight=1, uniform="rows")

# Create writing boxes
def make_box(label, row, column, columnspan=1):
    
    title = tk.Label(
        root,
        text=label,
        anchor="w",
        font=("Arial", 11)
    )
    
    title.grid(
        row=row - 1,
        column=column,
        columnspan=columnspan,
        sticky="w",
        padx=8,
        pady=(8, 0)
    )
    
    frame = tk.Frame(root)
    
    frame.grid(
        row=row,
        column=column,
        columnspan=columnspan,
        sticky="nsew",
        padx=8,
        pady=4
    )
    
    box = tk.Text(
        frame,
        wrap="word",
        font=("Arial", 14),
        undo=True
    )
    
    scroll = tk.Scrollbar(
        frame,
        command=box.yview
    )
    
    box.config(yscrollcommand=scroll.set)
    
    scroll.pack(side="right", fill="y")
    box.pack(side="left", fill="both", expand=True)
    
    return box

# Create the three writing areas
keywords_box = make_box("Keywords", 1, 0)
article_box = make_box("Article", 1, 1)
tagline_box = make_box("Tagline", 3, 0, columnspan=2)

# Section markers for saving files
SECTION_MARKERS = (
    "=== KEYWORDS ===",
    "=== ARTICLE ===",
    "=== TAGLINE ===",
)

# Update the word counter
def word_count(event=None):
    
    text = article_box.get("1.0", "end-1c")
    
    words = len(text.split())
    
    status.config(
        text=f"Word count: {words}"
        
    )
    
# Save writing to a text file
def save_file():
    path = filedialog.asksaveasfilename(
        defaultextension=".txt",
        filetypes=[("Text files", "*.txt")]
    )
    
    if not path:
        return
    
    try:
        with open(path, "w", encoding="utf-8") as f:
            
            f.write(
                SECTION_MARKERS[0] + "\n"
                + keywords_box.get("1.0", "end-1c")
                + "\n"
            )
            
            f.write(
                SECTION_MARKERS[1] + "\n"
                + article_box.get("1.0", "end-1c")
                +"\n"
            )
            
            f.write(
                SECTION_MARKERS[2] + "\n"
                + tagline_box.get("1.0", "end-1c")
                +"\n"
            )
            
        messagebox.showinfo(
            "Saved",
            "Your fiendish masterpiece has been saved. The Borg are trembling."
        )
        
    except OSError as error:
        
        messagebox.showerror(
            "Save Error",
            str(error)
        )
        
# Open an existing writing file
def open_file():
    path = filedialog.askopenfilename(
        filetypes=[("Text files", "*.txt")]
    )
    
    if not path:
        return
    
    try:
        with open(path, "r", encoding="utf-8") as f:
            text = f.read()
            
    except (OSError, UnicodeError) as error:
        
        messagebox.showerror(
            "Open Error",
            str(error)
        )
        
        return
    
    parts = {}
    current = None
    
    for line in text.splitlines():
        if line in SECTION_MARKERS:
            
            current = line
            parts[current] = []
        
        elif current is not None:
            
            parts[current].append(line)
            
    # Check that the file has the expected sections
    
    if not all(marker in parts for marker in SECTION_MARKERS):
        
        messagebox.showerror(
            "Open Error",
            "This is not a valid Writing Desk file."
        )
        
        return
    
    boxes = (
        keywords_box,
        article_box,
        tagline_box
    )
    
    for marker, box in zip(SECTION_MARKERS, boxes):
        box.delete("1.0", "end")
        
        box.insert(
            "1.0",
            "\n".join(parts[marker]).strip("\n")
        )
        
    word_count()
    
# Create the button bar
bar = tk.Frame(root)

bar.grid(
    row=4,
    column=0,
    columnspan=2,
    sticky="ew",
    padx=8,
    pady=8
)

# Open button
open_button = tk.Button(
    bar,
    text="Open",
    command=open_file,
    font=("Arial", 12),
    width=5,
    height=1
)

open_button.pack(side="left")

# Save Button
save_button = tk.Button(
    bar,
    text="Save",
    command=save_file,
    font=("Arial", 12),
    width=5,
    height=1
)

save_button.pack(side="left", padx=6)

# Word count display
status = tk.Label(
    bar,
    text="Word count: 0",
    font=("Arial", 12),
)

status.pack(side="right")

# Update wordcount when text changes
def check_word_count():
    
    if article_box.edit_modified():
        word_count()
        article_box.edit_modified(False)
        
article_box.bind(
    "<<Modified>>",
    lambda event: check_word_count()
)

article_box.edit_modified(False)

# Start the program
root.mainloop()




