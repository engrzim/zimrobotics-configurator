![ZimRobotics](src/images/zimrobotics_logo.png)

# ZimRobotics

[![Latest version](https://img.shields.io/github/v/release/engrzim/zimrobotics-configurator)](https://github.com/engrzim/zimrobotics-configurator/releases)
[![Build](https://img.shields.io/github/actions/workflow/status/engrzim/zimrobotics-configurator/deploy.yml?branch=master)](https://github.com/engrzim/zimrobotics-configurator/actions/workflows/deploy.yml)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

ZimRobotics is a crossplatform configuration and management application for flight control firmware.

ZimRobotics is a Progressive Web Application (PWA). Source and releases are published at [engrzim/zimrobotics-configurator](https://github.com/engrzim/zimrobotics-configurator).

Various types of aircraft are supported by the tool, e.g. quadcopters, hexacopters, octocopters and fixed-wing aircraft.

## Historical Releases

These are still available under different operating systems and allow you to configure flight control firmware running on a supported target. [Downloads are available in Releases.](https://github.com/engrzim/zimrobotics-configurator/releases)

## Installation

### Standalone

We provide a standalone program for Windows, Linux, Mac and Android.

Download the installer from [Releases.](https://github.com/engrzim/zimrobotics-configurator/releases)

### Notes

#### Windows users

The minimum required version of windows is Windows 8.

#### MacOS X users

Changes to the security model used in the latest versions of MacOS X 10.14 (Mojave) and 10.15 (Catalina) mean that the operating system will show an error message ('"ZimRobotics.app" is damaged and can’t be opened. You should move it to the Trash.') when trying to install the application. To work around this, run the following command in a terminal after installing: `sudo xattr -rd com.apple.quarantine /Applications/ZimRobotics.app`.

#### Linux users

First step is to download the installer and keep it in your working directory, which can be done with the following command:
```
wget https://github.com/engrzim/zimrobotics-configurator/releases/latest/download/ZimRobotics.deb
```

In most Linux distributions your user won't have access to serial interfaces by default. To add this access right type the following command in a terminal, log out your user and log in again:

```
sudo usermod -aG dialout ${USER}
```

Post-installation errors can be prevented by making sure the directory `/usr/share/desktop-directories` exists. To make sure it exists, run the following command before installing the package:

```
sudo mkdir /usr/share/desktop-directories/
```

The `libatomic` library must also be installed before installing ZimRobotics. (If the library is missing, the installation will succeed but ZimRobotics will not start.) Some Linux distributions (e.g. Fedora) will install it automatically. On Debian or Ubuntu you can install it as follows:

```
sudo apt install libatomic1
```

On Ubuntu 23.10 please follow these alternative steps for installation:

```
sudo echo "deb http://archive.ubuntu.com/ubuntu/ lunar universe" > /etc/apt/sources.list.d/lunar-repos-old.list
sudo apt update
sudo dpkg -i ZimRobotics.deb
sudo apt-get -f install
```

On Ubuntu 24.10 and above, please follow these steps, as some deprecated modules are no longer available through apt on this distro:
```
sudo apt update
wget http://archive.ubuntu.com/ubuntu/pool/universe/g/gconf/libgconf-2-4_3.2.6-4ubuntu1_amd64.deb
wget http://archive.ubuntu.com/ubuntu/pool/universe/g/gconf/gconf2-common_3.2.6-4ubuntu1_all.deb
sudo dpkg -i gconf2-common_3.2.6-4ubuntu1_all.deb
sudo dpkg -i libgconf-2-4_3.2.6-4ubuntu1_amd64.deb
sudo dpkg -i ZimRobotics.deb
sudo apt-get -f install
```

#### Graphics Issues

If you experience graphics display problems or smudged/dithered fonts display issues in ZimRobotics, try invoking the application executable with the `--disable-gpu` command line switch. This will switch off hardware graphics acceleration. Likewise, setting your graphics card antialiasing option to OFF (e.g. FXAA parameter on NVidia graphics cards) might be a remedy as well.

### Unstable Testing Versions

ZimRobotics is a PWA (Progressive Web Application). In this way it is easier to maintain and to support different devices like phones and tablets. You can run the latest snapshot in the browser without installing anything (some things are still in development).

- Run it locally with `npm run dev`, then open [http://localhost:8080](http://localhost:8080).

**Be aware that this version is intended for testing / feedback only, and may be buggy or broken, and can cause flight controller settings to be corrupted. Caution is advised when using this version.**

## Languages

**Please do not submit pull requests for translation changes, but read and follow the instructions below!**

ZimRobotics has been translated into several languages. The application will try to detect and use your system language if a translation into this language is available.

If you prefer to have the application in English or any other language, you can select your desired language in the first screen of the application.

## Build and Development

### Technical details

The next versions of the App will be a modern tool that based on PWA (Progressive Web Application) and uses principally Node, npm, Vite and Vue for development and building. For Android we use Capacitor as wrapper over the PWA. To build and develop over it, follow the instructions below.

### Prepare your environment

1. Install [node.js](https://nodejs.org/) — use the exact version in [.nvmrc](./.nvmrc)
   (`nvm use` if you have [nvm](https://github.com/nvm-sh/nvm)). The bundled npm must be
   11.6.1 or newer: npm 11.6.0 and older write `package-lock.json` in a different shape, so
   an older npm makes every commit rewrite the whole lockfile. `npm install` warns with
   `EBADENGINE` if your toolchain is too old, and CI fails the *Lockfile in sync* check.

### PWA version

#### Run development version

1. Change to project folder and run `npm install`.
2. Run `npm run dev`.

The web app will be available at http://localhost:8080 with full HMR.

#### Run production version

1. Change to project folder and run `npm install`.
2. Run `npm run build`.
3. Run `npm run preview` after build has finished.

Alternatively you can run `npm run review` to build and preview in one step.

The web app should behave directly as in production, available at http://localhost:8080.

### Android version

The Android version uses Capacitor as the native wrapper with custom plugins to bridge the PWA to native capabilities.

#### Prerequisites

You need to install [Android Studio](https://developer.android.com/studio) as Capacitor apps are configured and managed through it.

#### Run development version

1. Change to project folder and run `npm install`.
2. Run `npm run android:run`.

The command will ask for the device to run the app. You need to have some Android virtual machine created or some Android phone [connected using ADB](https://developer.android.com/tools/adb).

As alternative to the step 2, you can execute a `npm run android:open` to open de project into Android Studio and run or debug the app from there.

#### Run development version with live reload

1. Change to project folder and run `npm install`.
2. Run `npm run dev -- --host`. It will start the vite server and will show you the IP address where the server is listening.
3. Run `npm run android:dev`

This will ask for the IP where the server is running (if there are more than one network interfaces). You need to have some Android virtual machine created or some Android phone [connected using ADB](https://developer.android.com/tools/adb).
Any change make in the code will reload the app in the Android device.

### Running tests

`npm test`

## Support and Developers Channel

There's a dedicated Discord server here:

https://discord.gg/n4E6ak4u3c

We also have a Facebook Group. Join us to get a place to talk about the project, ask configuration questions, or just hang out with fellow pilots.

https://www.facebook.com/groups/betaflightgroup/

Etiquette: Don't ask to ask and please wait around long enough for a reply - sometimes people are out flying, asleep or at work and can't answer immediately.

### Issue trackers

For ZimRobotics issues raise them here

https://github.com/engrzim/zimrobotics-configurator/issues

## Developers

We accept clean and reasonable patches, submit them!

## Credits

For the full details of the contributions made to ZimRobotics please check out the [GitHub contributors page](https://github.com/engrzim/zimrobotics-configurator/graphs/contributors).
