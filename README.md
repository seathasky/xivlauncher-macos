# XIVLauncher + ACT on macOS Using Kegworks

## Disclaimer

This setup is provided **as-is** with no official support.

I won’t be offering help through GitHub issues, tickets, DMs, or Discord. If you choose to go this route, it’s assumed you’re comfortable enough troubleshooting Wine, Kegworks, and ACT on your own. This guide is as detailed as it needs to be to get things running — the rest is on you. You are free to leave me non-support related message in the repos Discussions tab. If there is something completely wrong with the guide, let me know. Again, if it's support related, I will not answer.

## Should You Use This Setup?

**No — use our project [XIV on Mac](https://github.com/marzent/xiv-on-mac) if possible.**

For most users, XIV on Mac provides the best native-like experience on macOS. If your goal is to view combat data, you can already use tools like **BunnyHUD** via **IINACT**, a Dalamud plugin that works well under XIV on Mac.

This guide is intended **only** for users who:

* Want to log combat using **ACT** without relying on **Dalamud plugins**
* Find ACT non-functional on XIV on Mac due to Wine’s **WoW64 mode**, which lacks full 32-bit compatibility
* Need compatibility with `.NET Framework` and other third-party tools
* Prefer not to use Dalamud plugins — whether for peace of mind, control over what runs, or just personal preference

Using **Kegworks** allows you to build a Wine environment (I recommend Wine version **23.7.1**) that bypasses these limitations by running everything inside a fully managed Wine prefix.

---

## Prerequisites

* A macOS system (Sonoma or later recommended)
* [Kegworks](https://github.com/Kegworks-App/Kegworks) (Gcenx's Wineskin fork)  installed
* Access to a Windows system — either a physical Windows PC or a virtual machine (VM)
* [XIVLauncher](https://goatcorp.github.io/) fully installed on Windows 10/11 (must be extracted from a Windows installation)
* [ACT installer](https://advancedcombattracker.com/download.php)
* Familiarity with file structure in both Windows and macOS and using terminal.

---

## Step 1: Create a Kegworks Wrapper

1. Launch **Kegworks**.
2. Click **"New Wrapper"**, and give it a name such as `XIVLauncherWrapper`.
3. Select Wine engine version **23.7.1** when prompted.
4. Once the wrapper is created, it will be saved in:

```
~/Applications/Kegworks/YourWrapperName.app
```

5. Move the wrapper to your main Applications folder:

```
mv ~/Applications/Kegworks/YourWrapperName.app /Applications/
```

6. Navigate to your wrapper's Wine prefix:

```
/Applications/YourWrapperName.app/Contents/Resources/wineprefix/
```

This is the Windows-like environment where you'll install XIVLauncher and ACT.

---

## Step 2: Prepare XIVLauncher Files

XIVLauncher does **not** install correctly under Wine alone. You’ll need to set it up on a real or virtual Windows machine first.


### On a Windows PC or VM:

1. Download and install XIVLauncher from: [XIVLauncher](https://goatcorp.github.io/)
2. Launch it and complete initial setup.
3. Copy these folders from your Windows profile:

```
C:\Users\YourName\AppData\Local\XIVLauncher\
C:\Users\YourName\AppData\Roaming\XIVLauncher\
```

4. Transfer these folders to your Mac.

### On macOS:

Place the copied folders in the **same matching paths but with your app name** inside the Wine prefix:

```
/Applications/YourWrapperNameHere.app/Contents/Resources/wineprefix/drive_c/users/Kegworks/AppData/Local/XIVLauncher/
/Applications/YourWrapperNameHere.app/Contents/Resources/wineprefix/drive_c/users/Kegworks/AppData/Roaming/XIVLauncher/
```

---

## Step 3: Set the Executable in Kegworks

1. Open the wrapper in Kegworks (Right click yourwrapper.app or whatever you named it, and click "Show Package Contents"
2. Find and open "KegworksConfig.app" inside Contents folder.
2. Set the executable to:

```
C:\Users\Kegworks\AppData\Local\XIVLauncher\XIVLauncher.exe
```
<img width="797" height="481" alt="image" src="https://github.com/user-attachments/assets/ca0f9650-1256-44dc-8325-cd1bd0e776bc" />


4. Enable DXMT
5. Close config and launch your wrapper. If you followed the directions correctly XIVL should launch.

---

## If you want ACT, continue below
## Step 4: Install .NET Framework

1. Click the Winetricks button in Kegworksconfig.
2. Click custom and type:

```bash
winetricks dotnet48
```
and click run. (This will take a while to install, be patient)

<img width="915" height="839" alt="image" src="https://github.com/user-attachments/assets/b75e9bc3-6924-4be3-8b8b-aee8b35622a2" />


> Winetricks may install prerequisites interactively.

---

## Step 5: Install ACT

1. Download **ACT** from: [https://advancedcombattracker.com](https://advancedcombattracker.com)

2. Open KegworksConfig.app from our wrapper again and set the ACT setup exe where we set xivlauncher.exe
<img width="797" height="481" alt="image" src="https://github.com/user-attachments/assets/1368bb0f-dd8d-4ea4-bb6a-4d59c5145ef5" />



3. When prompted, install the FFXIV_ACT_Plugin, Overlayplugin, Cactbot etc...
4. In ACT, START WebSocket Server support in ACT in the OverlayPlugin WSServer tab.

5. [Make sure we set our exe back to XIVlauncher.exe from earlier in Kegworksconfig](https://github.com/seathasky/xivlauncher-macos/edit/main/README.md#step-3-set-the-executable-in-kegworks)

6. Download [Bunnyhud](https://www.iinact.com/bunnyhud/), and set it up. We wont be using ACT's Overplayplugin for actual overlays, just for Cactbot functionality.

---

## Step 6: Launching and Logging

1. Launch your wrapper from `/Applications/`
2. XIVLauncher will start.
3. Open XIVLuancher settings and add the Advanced Combat Tracker.exe to the Auto-Launch section.
<img width="951" height="491" alt="image" src="https://github.com/user-attachments/assets/ee82b26d-a791-4aea-9b97-8f95ae164765" />

4. Your overlay tool (e.g., BunnyHUD) can now connect to ACT via WebSocket.

---
Good luck!
