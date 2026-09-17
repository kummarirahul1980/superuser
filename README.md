# superuser


## I know, we fought very well, and you don't talk to me again. but still I forgot to tell you this ..

📱 Termux — The Pocket Computer

A phone is already an ARM64 computer.

Termux opens access to that computer.

No Linux distribution is installed here.

No virtual machine.

No emulation.

The graphical side is kept small:

```text
Android
   │
   ▼
Termux
   │
   ├── Applications
   ├── X11 display
   ├── xfwm4
   └── VNC
          │
          ▼
        bVNC
```
"xfwm4" manages the windows.

VNC carries the display to the screen.

The rest is simply Termux.

---

🚀 Part I — First Boot

Two applications are needed on the phone.

Termux

[Download Termux](https://f-droid.org/repo/com.termux_1022.apk)

bVNC

[Install bVNC](https://play.google.com/store/apps/details?id=com.iiordanov.freebVNC)

Once both are installed, the pocket computer is ready.

---

1. Preparing Termux

Termux is opened.

Its packages are updated:

pkg update
pkg upgrade

The graphical repository is enabled:

pkg install x11-repo

The VNC server and window manager are installed:

pkg install tigervnc
pkg install xfwm4

The two main graphical applications are installed:

pkg install firefox
pkg install code-is-code-oss

There is deliberately no desktop environment here.

"xfwm4" is enough to manage the windows.

---

🔐 2. Creating the VNC password

The display needs a password.

vncpasswd

A new password is created when requested.

This password belongs to the VNC display.

bVNC will use it when connecting.

---

🖥️ 3. Starting the display

The graphical display is created:

vncserver :1

The display is:

:1

Its VNC port is:

5901

The display now exists.

---

🎯 4. Selecting the display

Graphical applications need to know where their windows should appear.

The current display is selected:

export DISPLAY=:1

The window manager is started:

xfwm4 &

Now the graphical session has a window manager.

---

📱 5. Connecting bVNC

bVNC is opened.

A new VNC connection is created.

The address is:

localhost:5901

The password created earlier is entered.

The connection is opened.

The graphical display appears.

There may not be a traditional desktop, wallpaper, application menu, or panel.

That is intentional.

This is a minimal graphical workspace.

Applications create their own windows, and "xfwm4" manages those windows.

---

🌐 6. Opening Firefox

The first graphical application can now be launched:

firefox &

Firefox appears on the VNC display.

The complete path is:

Firefox
   ↓
X display :1
   ↓
VNC server
   ↓
localhost:5901
   ↓
bVNC
   ↓
Phone screen

Firefox is now running directly inside the Termux environment.

---

💻 7. Opening Code OSS

Code OSS is available in the same way:

code &

It appears on the same display.

Only one heavy graphical application should normally be used at a time.

For example:

Firefox
   ↓
close Firefox
   ↓
Code OSS

rather than keeping several heavy graphical applications open together.

---

⚠️ The Phone Is Still a Phone

A graphical display is real work for the phone.

VNC continuously handles graphical data.

Firefox can use considerable RAM and CPU.

Code OSS can also use considerable RAM.

The phone may become warm during extended graphical use.

For smoother operation:

«Keep one heavy graphical application open at a time.»

Close applications when they are no longer needed.

When the graphical session itself is no longer needed, stop the VNC server.

---

⚠️ Termux Process Controls

The graphical system is made from ordinary processes running inside Termux.

That means the terminal's process controls still matter.

Action| Meaning
"Ctrl+C"| Stops the current foreground process
"Ctrl+Z"| Suspends the current foreground process
"bg"| Continues a suspended job in the background
"fg"| Brings a job back to the foreground
"&"| Starts a command in the background

For example:

firefox &

starts Firefox in the background.

But:

Ctrl+Z

suspends the current foreground process.

Those are not the same thing.

If a process was accidentally suspended:

fg

brings it back.

The VNC server and graphical processes should not be casually stopped or suspended.

---

🔌 8. Reconnecting Later

The VNC display can remain running even when bVNC is closed.

Check the displays:

vncserver -list

If ":1" is still running, bVNC can reconnect to:

localhost:5901

No new display is required.

If ":1" has been stopped, create it again:

vncserver :1

Then select it:

export DISPLAY=:1

and start the window manager:

xfwm4 &

bVNC reconnects to:

localhost:5901

---

🛑 9. Closing the Graphical Session

When the graphical session is finished:

vncserver -kill :1

The VNC display disappears.

Termux remains available.

The phone is back to its lightweight terminal mode.

The next graphical session starts again with:

vncserver :1
export DISPLAY=:1
xfwm4 &

Then bVNC connects to:

localhost:5901

---

🧠 Part II — What Just Happened?

The first half created the pocket computer.

Now the pieces can be separated.

---

📦 Termux

Termux is the foundation.

It provides the environment in which the programs are running.

There is no separate Linux distribution here.

The programs were installed directly through Termux:

Termux
 ├── Firefox
 ├── Code OSS
 ├── xfwm4
 └── TigerVNC

Everything is running inside the same Termux environment.

---

🖼️ The Display

A graphical application needs a destination.

That destination is represented by:

DISPLAY=:1

The command:

export DISPLAY=:1

places that value into the current shell's environment.

After that:

firefox &

knows that its graphical output belongs on display ":1".

The same applies to:

code &

---

🔢 What ":1" Means

A graphical system can have different displays:

:0
:1
:2
:3

Here, the VNC server creates:

vncserver :1

so the graphical display is:

:1

The corresponding VNC port is:

5901

So:

Display :1
     │
     └── VNC port 5901

The two numbers are related but are not the same thing.

":1" identifies the graphical display.

"5901" identifies the VNC network port.

---

🪟 What "xfwm4" Does

"xfwm4" is a window manager.

It manages the windows produced by graphical applications.

For example:

┌──────────────────────────────┐
│ Firefox                  ─ □ ×│
│                              │
│          Browser             │
│                              │
└──────────────────────────────┘

The application creates the contents.

"xfwm4" manages the window around those contents.

It handles things such as:

- Moving windows
- Resizing windows
- Focusing windows
- Minimizing windows
- Maximizing windows
- Window borders
- Window placement

There is no Xfce desktop environment involved.

"xfwm4" is being used directly as the window manager.

---

🔌 What VNC Does

The graphical session exists inside Termux.

VNC provides a way to see and control it.

The path is:

GUI application
      │
      ▼
   X display
      │
      ▼
   xfwm4
      │
      ▼
 VNC server
      │
      ▼
 localhost:5901
      │
      ▼
    bVNC
      │
      ▼
 Android screen

VNC is therefore not the desktop.

It is the connection to the graphical display.

---

🔢 Multiple Displays

More than one display can exist.

For example:

vncserver :1
vncserver :2

could create:

:1 → 5901
:2 → 5902

The destination can then be changed:

export DISPLAY=:2

A graphical application started from that shell will target display ":2".

This is why "DISPLAY" matters.

It connects an application to a particular graphical session.

---

⚙️ Processes

Everything running is a process.

The VNC server is a process.

"xfwm4" is a process.

Firefox is a process.

Code OSS starts processes of its own.

The Termux shell is also a process.

The shell controls its jobs using commands such as:

jobs
fg
bg

and the "&" operator.

For example:

firefox &

starts Firefox as a background job.

Whereas:

Ctrl+Z

suspends a foreground process.

That distinction becomes important when the graphical system is running.

---

🔥 Why the Phone Gets Warm

A terminal command can be very light.

A graphical application is different.

The system may now be doing:

Application
     ↓
Graphical rendering
     ↓
X display
     ↓
Window management
     ↓
VNC encoding
     ↓
VNC connection
     ↓
bVNC
     ↓
Android display

Firefox adds its own workload.

Code OSS adds its own workload.

The phone is therefore doing genuine computer work.

The ARM64 architecture makes the programs native to the phone's processor.

It does not make a graphical desktop free of CPU, RAM, battery, or heat.

---

💤 The Lightweight Mode

When the graphical session is no longer needed:

GUI applications closed
        ↓
VNC stopped
        ↓
Termux

The phone returns to its lightweight Termux state.

When the graphical computer is needed again:

vncserver :1
export DISPLAY=:1
xfwm4 &

bVNC reconnects to:

localhost:5901

The computer was never gone.

Its graphical display was simply stopped.

---

🛠️ Quick Reference

Start the display

vncserver :1

Select the display

export DISPLAY=:1

Start the window manager

xfwm4 &

Start Firefox

firefox &

Start Code OSS

code &

Check VNC displays

vncserver -list

Connect with bVNC

localhost:5901

Stop the display

vncserver -kill :1

See shell jobs

jobs

Bring a job forward

fg

Continue a suspended job in the background

bg

---

🌱 The Pocket Computer

Nothing was installed inside a virtual computer.

Nothing was emulated.

No Linux distribution was placed underneath Termux.

The phone's own ARM64 environment was used directly.

             📱 Android
                  │
                  ▼
             ARM64 CPU
                  │
                  ▼
               Termux
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
      Firefox   Code OSS   xfwm4
                            │
                            ▼
                       display :1
                            │
                            ▼
                        VNC :5901
                            │
                            ▼
                          bVNC
                            │
                            ▼
                       🖥️ Screen

The computer was already in the pocket.

The display was simply waiting to be opened.