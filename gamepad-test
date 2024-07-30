import pygame
import tkinter as tk
from tkinter import ttk

# Initialize Pygame
pygame.init()
pygame.joystick.init()

# Check if any joysticks are connected
joystick_count = pygame.joystick.get_count()
if joystick_count == 0:
    raise Exception("No joysticks connected")

# Initialize the first joystick
joystick = pygame.joystick.Joystick(0)
joystick.init()

class GamepadTester:
    def __init__(self, root):
        self.root = root
        self.root.title("Gamepad Tester")

        self.create_widgets()
        self.update_gamepad_status()

    def create_widgets(self):
        self.label = ttk.Label(self.root, text="Move or press any button on the gamepad.", font=("Helvetica", 14))
        self.label.grid(row=0, column=0, columnspan=2, pady=10)

        self.axis_frame = ttk.LabelFrame(self.root, text="Axes", padding="10")
        self.axis_frame.grid(row=1, column=0, padx=10, pady=10, sticky="nsew")

        self.button_frame = ttk.LabelFrame(self.root, text="Buttons", padding="10")
        self.button_frame.grid(row=1, column=1, padx=10, pady=10, sticky="nsew")

        self.hat_frame = ttk.LabelFrame(self.root, text="Hats", padding="10")
        self.hat_frame.grid(row=2, column=0, columnspan=2, padx=10, pady=10, sticky="nsew")

        self.root.columnconfigure(0, weight=1)
        self.root.columnconfigure(1, weight=1)
        self.root.rowconfigure(1, weight=1)
        self.root.rowconfigure(2, weight=1)

        self.axis_labels = []
        self.axis_bars = []
        for i in range(joystick.get_numaxes()):
            label = ttk.Label(self.axis_frame, text=f"Axis {i}:")
            label.grid(row=i, column=0, sticky="w")
            self.axis_labels.append(label)
            bar = ttk.Progressbar(self.axis_frame, orient="horizontal", length=200, mode="determinate")
            bar.grid(row=i, column=1, sticky="ew")
            self.axis_bars.append(bar)

        self.button_labels = []
        for i in range(joystick.get_numbuttons()):
            label = ttk.Label(self.button_frame, text=f"Button {i}: Released")
            label.grid(row=i, column=0, sticky="w")
            self.button_labels.append(label)

        self.hat_labels = []
        for i in range(joystick.get_numhats()):
            label = ttk.Label(self.hat_frame, text=f"Hat {i}: (0, 0)")
            label.grid(row=i, column=0, sticky="w")
            self.hat_labels.append(label)

    def update_gamepad_status(self):
        pygame.event.pump()

        # Update axes
        for i in range(joystick.get_numaxes()):
            axis = joystick.get_axis(i)
            self.axis_labels[i].config(text=f"Axis {i}: {axis:.2f}")
            self.axis_bars[i]['value'] = (axis + 1) * 50  # Convert -1 to 1 range to 0 to 100

        # Update buttons
        for i in range(joystick.get_numbuttons()):
            button = joystick.get_button(i)
            state = "Pressed" if button else "Released"
            self.button_labels[i].config(text=f"Button {i}: {state}")

        # Update hats (D-pad)
        for i in range(joystick.get_numhats()):
            hat = joystick.get_hat(i)
            self.hat_labels[i].config(text=f"Hat {i}: {hat}")

        # Schedule the next update
        self.root.after(100, self.update_gamepad_status)

# Create the main window
root = tk.Tk()
gamepad_tester = GamepadTester(root)
root.mainloop()

# Quit Pygame
pygame.quit()
