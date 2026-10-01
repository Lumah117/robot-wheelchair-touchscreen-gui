# Robotic Wheelchair Touchscreen GUI

A Python touchscreen graphical user interface developed as part of a university robotic-wheelchair project.

The interface was designed for an LCD touchscreen mounted to the wheelchair and provides the user with access to wheelchair-related functions including environmental mapping, reversing-camera functionality, horn/buzzer control and an emergency-stop interface.

The project combines **Python GUI development, human–robot interaction, camera integration and assistive robotics**.

---

## Project Overview

The objective was to create a simple touchscreen interface through which a wheelchair user could access important system functions.

The interface was designed around a central information/display area surrounded by large touchscreen controls.

Conceptually:

```text
+------------------------------------------+
|                                          |
|  Mapping &              Reversing        |
|  Environment              Camera         |
|                                          |
|  +------------------------------------+  |
|  |                                    |  |
|  |                                    |  |
|  |        CENTRAL DISPLAY             |  |
|  |                                    |  |
|  |                                    |  |
|  +------------------------------------+  |
|                                          |
|  Horn / Buzzer           EMERGENCY STOP  |
|                                          |
+------------------------------------------+
| Menu | Settings | About | Exit           |
+------------------------------------------+
```

The GUI was configured for a compact:

```text
420 × 420
```

window suitable for the intended touchscreen interface.

---

## Technologies

- Python
- Tkinter
- OpenCV
- Pillow
- Pygame
- Threading
- Graphical User Interface development
- Camera integration
- Human–Robot Interaction
- Assistive Robotics

---

## Repository Structure

```text
robotic-wheelchair-touchscreen-gui/
│
├── README.md
├── LICENSE
│
└── src/
    └── Wheelie_1.1.py
```

---

# Interface Design

The main `WheelchairGUI` class constructs the touchscreen interface and manages user interaction.

The primary controls are:

```text
Wheelchair GUI
│
├── Mapping & Environment
├── Reversing Camera
├── Horn / Buzzer
├── Emergency Stop
│
├── Central Display
│
└── Bottom Menu
    ├── Menu
    ├── Settings
    ├── About
    └── Exit
```

Large buttons were used for the main wheelchair functions so that the interface could be operated through a touchscreen rather than a conventional mouse-and-keyboard interface.

---

# Mapping & Environment

The interface includes a:

```text
Mapping & Environment
```

control.

In the recovered prototype, selecting the button updates the central display to indicate that the mapping/environment function has been selected.

The intention was for this region of the interface to provide access to environmental and navigation information associated with the robotic wheelchair.

---

# Reversing Camera

The GUI includes a dedicated:

```text
Reversing Camera
```

control.

The recovered code contains an experimental OpenCV camera implementation intended to display live camera footage within the GUI.

The camera is initialised using:

```python
cv2.VideoCapture(0)
```

and camera processing can be started using a separate thread.

Conceptually:

```text
Camera
   |
   v
OpenCV Capture
   |
   v
Read Frame
   |
   v
Resize
   |
   v
BGR -> RGB
   |
   v
Pillow Image
   |
   v
ImageTk
   |
   v
Touchscreen GUI
```

Each captured frame is resized to:

```text
400 × 300
```

before being converted from OpenCV's BGR representation into RGB.

The frame is then converted into an `ImageTk.PhotoImage` for use within the Tkinter interface.

---

## Camera Threading

Camera processing is designed to operate using Python's `threading` module.

```text
Main GUI Thread
      |
      +----------------------+
      |                      |
      v                      v
GUI Events             Camera Thread
                             |
                             v
                       Capture Frame
                             |
                             v
                       Process Image
                             |
                             v
                       Update Display
```

The intention was to prevent continuous camera capture from blocking normal GUI interaction.

The application also contains cleanup logic that stops the camera loop, waits for the camera thread to terminate and releases the OpenCV camera resource when the application closes.

---

# Emergency Stop Interface

One of the more distinctive interface components is the custom emergency-stop button.

Rather than using a standard rectangular Tkinter button, a custom:

```python
HexagonalButton
```

class was created using `tk.Canvas`.

The button calculates the coordinates of a regular hexagon and draws the emergency control as a red polygon.

Conceptually:

```text
        ______
       /      \
      /        \
     | EMG STOP |
      \        /
       \______/
```

A mouse/touch event is bound to the canvas so that pressing the polygon triggers the associated emergency-stop callback.

This provided experience with extending standard GUI components rather than relying solely on built-in widgets.

---

# Horn / Buzzer

The interface also contains a:

```text
Horn/Buzzer
```

control.

The recovered implementation includes experimental audio functionality using:

```python
pygame.mixer
```

with functions for playing both horn and emergency-warning audio.

Placeholder audio-file paths are retained in the original source.

The active prototype button currently displays the selected function in the central information area rather than invoking the audio implementation directly.

---

# Central Display

The central region of the GUI is implemented using a Tkinter text widget.

When one of the primary controls is selected, the display is updated with the selected function.

For example:

```text
Selected: MAPPING & ENVIRONMENT
```

or:

```text
Selected: EMERGENCY STOP ACTIVATED!
```

The implementation also contains functionality intended to centre the displayed text within the content area.

---

# Bottom Navigation

A bottom banner provides additional interface controls:

```text
+-------------------------------------+
| Menu | Settings | About | Exit      |
+-------------------------------------+
```

The recovered prototype provides callbacks for each button.

The `Exit` control performs application shutdown and invokes the camera cleanup routine before destroying the Tkinter window.

---

# GUI Architecture

The software can be represented at a high level as:

```text
                     WheelchairGUI
                           |
          +----------------+----------------+
          |                |                |
          v                v                v
      User Input      Central Display   Camera System
          |                                 |
   +------+------+                          v
   |      |      |                       OpenCV
   |      |      |                          |
   v      v      v                          v
 Map   Camera   Horn                    Threading
   |
   v
Emergency Stop
```

The interface therefore acts as a potential human-facing layer between the wheelchair user and the wider robotic system.

---

# Human–Robot Interaction

Unlike many of my earlier robotics projects, this project focused primarily on the **human interface to a robotic system**.

A robotic wheelchair must not only perform autonomous or assisted movement; it must also provide a clear way for the user to interact with the system.

This project therefore introduced considerations including:

- Touchscreen interaction
- Button size and placement
- Visual feedback
- Safety-related controls
- Camera feedback
- User-accessible navigation functions
- Application shutdown and resource management

---

# Concepts Demonstrated

This project provided practical experience with:

- Python
- Tkinter
- GUI development
- Object-oriented programming
- Custom GUI widgets
- Event-driven programming
- OpenCV
- Camera capture
- Image processing
- Pillow / ImageTk
- Multithreading
- Pygame audio
- Resource cleanup
- Human–robot interaction
- Assistive robotics
- Touchscreen interface design

---

# Original Implementation

The source contained in this repository preserves the original university project implementation.

It represents a **prototype touchscreen interface**, rather than a complete production wheelchair-control system.

Some functions were implemented experimentally and were not connected to the active GUI controls in the recovered version.

For example:

- The reversing-camera capture pipeline exists, but the active Reversing Camera button displays a status message rather than starting the camera thread.
- Horn and emergency audio functions exist, but the active buttons use GUI status callbacks.
- Menu, Settings and About currently provide placeholder callback behaviour.
- Mapping functionality is represented within the interface but is not integrated with a mapping/navigation system in this recovered source.

These limitations are documented intentionally rather than presenting prototype functionality as a completed system.

---

# Safety Note

This software was created as an educational robotic-wheelchair interface prototype.

The graphical **Emergency Stop** control should not be interpreted as a safety-rated emergency-stop implementation.

A real powered mobility system would require an appropriately engineered hardware safety architecture independent of the graphical user interface and general-purpose software.

---

# Retrospective

Reviewing this project with my current robotics and software-development experience highlights several areas I would approach differently today.

## GUI Architecture

The original application places most GUI functionality within a single `WheelchairGUI` class.

For a larger application I would separate:

```text
                Presentation Layer
                       |
                       v
                 GUI Controller
                       |
          +------------+------------+
          |            |            |
          v            v            v
       Camera      Navigation     Wheelchair
       Service       Service       Control
```

This would prevent the user-interface code from becoming tightly coupled to individual hardware interfaces.

---

## Camera Processing

The original prototype uses a dedicated Python thread for camera acquisition.

A modern implementation would carefully separate camera capture from Tkinter widget updates, ensuring that GUI operations occur safely within the main GUI event thread.

For a robotics system, the camera might instead be provided through a dedicated ROS/ROS2 component, with the GUI acting as a consumer of the processed image stream.

---

## Emergency Stop

The GUI emergency-stop button is useful as a user-interface control, but software alone should not form the safety mechanism for a real powered wheelchair.

A production design would separate:

```text
GUI Stop Request
      |
      v
High-Level Control


Physical E-Stop
      |
      v
Independent Safety Circuit
      |
      v
Motor Power / Drive Inhibit
```

The physical emergency-stop system would therefore remain capable of placing the wheelchair into a safe state even if the GUI, operating system or higher-level control software failed.

---

## Application State

Rather than using individual callbacks to replace central-screen text, a larger implementation could define explicit interface states such as:

```text
HOME
 |
 +--> MAPPING
 |
 +--> CAMERA
 |
 +--> SETTINGS
 |
 +--> ABOUT
 |
 +--> EMERGENCY
```

This would make navigation between interface screens easier to manage as functionality expanded.

---

## Touchscreen Design

For a real accessibility-focused interface I would also formally evaluate:

- Minimum touch-target size
- Contrast
- Font size
- Colour dependence
- User feedback
- Accidental activation
- Glove/limited-dexterity operation
- Screen readability
- Emergency-control placement
- Confirmation requirements
- Accessibility needs of the intended users

These considerations become particularly important when designing interfaces for assistive robotic systems.

---

## Portfolio Context

This project adds a different dimension to my robotics portfolio.

Many robotics projects focus on what happens internally:

```text
Sensors
   |
   v
Perception
   |
   v
Decision Making
   |
   v
Control
   |
   v
Actuators
```

This project instead focuses on the **human-facing side**:

```text
                     USER
                       |
                       v
                 TOUCHSCREEN
                       |
                       v
               Wheelchair GUI
                       |
          +------------+------------+
          |            |            |
          v            v            v
       Camera       Mapping       Controls
                       |
                       v
                ROBOTIC SYSTEM
```

It therefore demonstrates experience not only with robot software, but also with designing an interface through which a human can interact with a robotic platform.

That combination of **robotics, Python, GUI development, computer vision and human–robot interaction** makes this project a useful part of my wider software and robotics portfolio.
