# macOS Pop-Ups

This repo serves as a collection of Red Team techniques and administrative tasks for various macOS versions that cause popups, what those popups look like, what permissions are being requested, where they're stored, and hopefully how to check for them before causing popups.

All scenarios consider a basic macOS host with userA, userB, and root users.

When testing, you can reset the permissions back to default with `tccutil reset All` or you can specify a specific service. All TCC information for the current user is saved in an sqlite database located at `~/Library/Application Support/com.apple.TCC/TCC.db`. 

# Table of Contents
- [macOS Mojave (10.14)](#macos-mojave)
  - [InterProcess Apple Events](#interprocess-apple-events)
- [macOS Catalina (10.15)](#macos-catalina)
  - [File Accesses](#file-accesses)
  - [Auto Approved Accesses](#auto-approved-accesses)

## macOS Mojave

### InterProcess Apple Events

## macOS Catalina

- Using ObjC APIs to capture the screen causes a popup for `"Terminal.app" would like to record this computer's screen`. To approve it, the user must go to System Preferences, Screen Recording, and select the checkbox next to `Terminal.app`. Even then, `Terminal.app` will not be able to record the contents of your screen until it is quit dialog box is presented to the user.
- Using ObjC APIs to read the clipboard seems fine.
- Using AppleScript to read security settings from System vents causes popup for `"Terminal.app" wants access to coontrool "System Events.app". Allowing coontrol will proovide access to documents and data in "System Events.app", and to perform actions within that app"`. Provides a simple "OK" or "Don't Allow" box. Adds a new entry to "Automation" TCC entry with a specific pairing for "Terminal" and "System Events.app"

### File Accesses
|Current User | Process | Path | Popup | TCC permission | Details | Version |
| ----------- | ------- | ---- | ----- | -------------- | ------  | ------- |
| userA | ls | ~/Desktop | "Terminal.app" would like to access files in your Desktop folder. | Files and Folders | Specific pairing between Terminal and Desktop Folder| 10.15.2 |
| userA | ls | ~/Downloads | "Terminal.app" would like to access files in your Downloads folder. | Files and Folders | Specific pairing between Terminal and Downloads Folder| 10.15.2 |
| root | osascript using ObjC APIs from Terminal.app | /Users/userA/Library | "Terminal.app" would like to access your reminders | Reminders | Entry for terminal for reminders | 10.15.2 |

### Auto Approved Accesses
| binary | TCC permission | Details | version |
| -----  | -------------  | ------- | ------- |
| sshd-keygen-wrapper | Full Disk Access | Enabling "Remote Login" via Sharing for SSH access | 10.15.2 |
