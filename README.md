# IconLib

## A bash script to restore the macOS application icons clipped by the new "squircle" shape


* [Introduction](#introduction)
* [Installation](#installation)
    * [Homebrew (recommended)](#homebrew-recommended)
    * [Manual download](#manual-download)
* [Customization](#customization)
* [Usage](#usage)
    * [Homebrew installation](#homebrew-installation)
    * [Manual installation](#manual-installation)
    * [Log file](#log-file)
    * [Reverting changes](#reverting-changes)
* [Notes](#notes)
* [Acknowledgements](#acknowledgements)


## Introduction

With the latest version of macOS, 26/Tahoe, Apple introduced a new icon format, that must now fit into the predefined _squircle_ shape. All application icons that do not fit within the squircle are shrunk and placed above an ugly grey background.

However, the icons of several applications are beautifully crafted and do not fit well into this format. Examples include Audio Hijack, BBEdit, Alfred, Amphetamine, VLC, NValt, Gemini 2, HandBrake and Keyboard Maestro. The conversion carried out by Tahoe renders all these icons unattractive and often barely visible.

You have two options:

- either **Wait** for the developers to update their applications with new Tahoe-compatible icons,
- or **Restore** the original pre‑Tahoe icon yourself.

Doing the latter manually (right-click an application, select `Get Info` and replace the  icon thumbnail with a different image), can work for a few apps, but quickly becomes tedious (and error-prone). IconLib automates the whole process.

Even when developers update their applications with new, Tahoe-compatible icons, the result is often visually unappealing and you may therefore prefer to revert to the pre-Tahoe icons.


## Installation

> Important: IconLib is intended for macOS 26 (Tahoe). It will also run on Sonoma and earlier releases, but it won't produce any visual changes here.


### Homebrew (recommended)

```
brew install sabinomaggi/sm/iconlib
```

Homebrew will also install the `fileicon` CLI,  which actually manipulates the icon resources.
Once installation is complete, you can jump straight to the Customization section.


### Manual download

Download the [latest Release](https://github.com/sabinomaggi/IconLib/releases) from GitHub and extract the archive to a convenient location, e.g. `~/Documents/IconLib`,

```
mkdir -p ~/Documents/IconLib 
tar -xzf ~/Downloads/IconLib‑<version>.tar.gz -C ~/Documents/IconLib  --strip-components 1
```

In the remainder of this guide we will assume that the project resides in that directory. Change the path accordingly if you use another directory.

To allow macOS to locate the script, add `~/Documents/IconLib` to your `$PATH` (or `cd` into it before each use)

```
export PATH="$HOME/Documents/IconLib:$PATH"
```

_Tip_: Put the export line in ~/.zshrc or ~/.bash_profile for persistence.

The archive includes its own copy of `fileicon`; keep it in the same folder as `iconlib`.


## Customization

IconLib needs a plain text **support** file that lists the apps whose icons you want to fix.  Each line follows the format

```
<app name>, <icon file>
```

- Lines beginning with `#` are treated as comments.
- Blank lines are ignored.


_Any filename and extension_ works, as long as the file follows the `<app name>, <icon file>` CSV format.

An example file (`applist.txt`) ships with the release and can also be [fetched from the repository](https://github.com/sabinomaggi/IconLib/blob/main/applist.txt). Edit it to:

- **Remove** (or comment out) entries for applications you don’t have.
- **Add** missing applications, pointing to the exact `.icns` file you wish to use.

If the icon filename matches the application name, IconLib can locate it automatically; otherwise you’ll need to supply the explicit filename.

> Finding the correct .icns file
>
> Right‑click the application → Show Package Contents.
> Open the Contents/Resources folder.
> Locate the appropriate .icns file (some apps contain several icons; pick the one that matches the original look).


## Usage

### Homebrew installation

```
iconlib -f /path/to/applist.txt
```

### Manual installation

```
cd ~/Documents/IconLib
./iconlib -f /path/to/applist.txt
```

### Log file

Each execution creates a new log file in the current working directory, listing every app whose icon was altered (or restored).


### Reverting changes

All changes can be reverted by running `iconlib` with the `-u` switch

```
iconlib -f /path/to/applist.txt -u      # homebrew
./iconlib -f /path/to/applist.txt -u    # manual
```


## Notes

- **First-run permission**. The first time the `iconlib` script is run, macOS will refuse to perform the required operations, displaying a warning:
    _'Terminal' has been prevented from modifying apps on your Mac_
    Click on the `Allow...` button, which opens `System Settings` > `Privacy & Security` > `App Management` and toggle the switch next to the `Terminal`, Rerun the script afterwards.

- **App Store / Installer‑based apps**. Changing icons for apps installed via the App Store or a signed installer requires `sudo` privileges

    ```
    sudo [./]iconlib -f /path/to/applist.txt
    ```

- The script does **not modify the application binaries**; it merely replaces the icon resource that macOS displays in Finder and the Dock, automating the changes that can be made by right-clicking the app in the Applications folder and choosing `Get Info`.



## Acknowledgements

IconLib relies on the fantastic `fileicon` macOS CLI for managing custom icons for files and folders, created by Michael Klement: [https://github.com/mklement0/fileicon](https://github.com/mklement0/fileicon).


_Happy icon restoring!_
