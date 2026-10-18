# Student Timer Clock v0.1

Timers, exam countdowns, stopwatches with named laps, customisable desktop widgets, a fullscreen classroom board, "when done" actions and study statistics. Built for Windows 10/11 (x64).

## Download / install

| File | What it is |
|---|---|
| `release/StudentTimerClock-Setup-0.1.exe` (47 MB) | **Installer (recommended).** Includes everything; you don't need to install .NET separately. |
| `release/StudentTimerClock-0.1-portable-needs-NET8.exe` (0.6 MB) | Single exe with no installer. It needs the [.NET 8 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/8.0) (x64). |

### What the installer does
1. **Welcome**
2. **Terms & Conditions.** The *Next* button stays disabled until you tick **"I have read and accept the terms and conditions"**.
3. **Install folder.** The default is `%LOCALAPPDATA%\Programs\StudentTimerClock`. It installs just for you, so no administrator rights are needed.
4. **Components.** All of these are ticked by default:
   - Program
   - **Start automatically with Windows**
   - Start Menu shortcut
   - Desktop shortcut
5. **Install → Finish.** You can choose "Start Student Timer Clock now".

- **Autostart:** the installer adds `HKCU\…\CurrentVersion\Run\StudentTimerClock = "…\StudentTimerClock.exe" --autostart`.
- **Turning autostart off:** use *File → Settings → Start with Windows*, or Task Manager → Startup apps.
- **Portable exe:** on its first run it also turns autostart on by default.
- **Uninstall:** use *Settings → Apps* or the Start Menu. Uninstalling removes:
  - the program files
  - the shortcuts
  - the autostart entry

  It then asks whether to delete your data too.
- **Your data** is stored in `%APPDATA%\StudentTimerClock\data.json`.
- Windows SmartScreen may warn you because the exe isn't code-signed. Click *More info → Run anyway*.

## What's new in 0.1

### Widgets you can customise (timers *and* stopwatches)
- To open the style editor, do any of these:
  - hover a widget and click the 🖌 button
  - right-click a widget → *Font, colours & background…*
  - use the ribbon's **Widget Style** button
- **Layouts:**
  - **Digital**, with **DAY / HOUR / MIN / SEC** labels above the digits
  - **Clock dial**: an orange pie shows the time left, with the numbers around the dial, a hand, a digital readout, and ▶ / ↺ buttons
  - **Compact bar**
- **Title text** can be anything, e.g. *"Good morning my dear"*. You can set the **title font** and **digit font** (any installed font or the built-in LCD), plus bold and italic.
- **Colours:**
  - digit colour, label colour, dial colour
  - background colour, **gradient**, or a **picture** (fill or stretch, with optional darkening)
  - colour themes: Light / Dark / Navy / Sky / Classroom / Item colour
- **Below the timer:**
  - the **date**
  - the **finish time in AM/PM**, e.g. a 5-minute timer started at 10:00 shows **"Ends at 10:05 PM"**. You can switch to 24-hour.
  - a progress bar
- **Resizing:** drag the bottom-right grip, or set the size in the dialog.
- **Window options:** opacity, always on top, lock position, rounded corners.
- You can apply a style to **one widget**, to **all widgets**, or **save it as the default** for new widgets.

### Timer editor (tabs)
- **Days** field for long durations.
- **Count down to a date & time**, for exams, holidays and deadlines. The sample *"Final exam"* shows *44 DAY 15 HOUR 03 MIN*.
- **When done (actions)** tab. The Action dialog lists actions on the left and their settings on the right, with **Test / OK / Cancel**. The actions are:
  - **Start next timer** (chains timers, e.g. lesson segments or Pomodoro). The widget moves on to the next timer automatically.
  - Start timer, Run program, Open URL/file, Send email
  - Lock, Log off, Restart, Sleep, Hibernate, Shut down
  - Turn off monitor, Screen saver, End process, Press keys, Play/pause music
  - **Pause (wait)** between actions
- **Safety countdown:** restart, shut down, log off, sleep and hibernate first show a **30-second countdown** that you can cancel.

### Board view (Ctrl+B)
- Shows timers as big coloured tiles with DAY/HOUR/MIN/SEC and "Ends at …".
- You can choose what to show: Running / All timers / All stopwatches / Everything / a single group.
- **F11 fullscreen** is meant for a classroom projector. Double-click a tile to start or pause it.

### Main grid
- New **Ends at** column (e.g. *Today 5:59:15 PM*) and **When done** column (e.g. *Start next timer*).

### Also
- Help → **Terms & Conditions** and **About** tabs.
- New icon (a stopwatch with a graduation cap).

## Building it yourself
```bash
cd src
# installer payload (self-contained folder)
dotnet publish -c Release -r win-x64 --self-contained true -p:PublishSingleFile=false -p:DebugType=none -o ../publish
# small exe (needs .NET 8 Desktop Runtime)
dotnet publish -c Release -r win-x64 --self-contained false -p:PublishSingleFile=true -p:DebugType=none -o ../small
cd ../installer
makensis -DPAYLOAD=../publish StudentTimerClock.nsi     # → StudentTimerClock-Setup-0.1.exe
```
Needs the .NET 8 SDK (it can build from Linux or Windows) and NSIS 3.
