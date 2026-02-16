---
title: Cloudflare Tunnels: Minimal Docker Compose Guide
date: 2025-11-20
categories: [3-Server]
tags: [Android, Mobile]
author: <author_rylan>
---


# The Easiest Guide to Setting Up Maestro UI Testing on Windows
Mobile automation testing used to be a headache. Maestro changes that. It is the simplest, most effective UI testing framework for Android and iOS, relying on simple YAML files instead of complex code.

While Maestro has native support for Mac, setting it up on Windows requires a few manual steps. This guide will get you running in under 10 minutes.

# 🛠️ Prerequisites
Before installing Maestro, we need two things: Java and ADB (Android Debug Bridge).

## 1. Install Java (JDK 17+)
Maestro runs on the Java Virtual Machine. We recommend the standard "Eclipse Temurin" distribution.

Go to Adoptium.net.

Click Latest LTS Release (Download JDK 17 or 21).

Run the installer.

Crucial Step: When asked about custom setup, ensure you select "Set JAVA_HOME variable" (change the red X icon to "Will be installed on local hard drive").

Finish installation.

## 2. Install ADB
This tool allows your PC to talk to your Android device.

Download the SDK Platform-Tools for Windows.

Extract the zip file to C:\platform-tools.

Open your Command Prompt (search "cmd" in the start menu).

Copy and run this command to add ADB to your system path:

```setx PATH "%PATH%;C:\platform-tools```

Close and reopen your Command Prompt, then type adb version to verify.

# 🚀 Installing Maestro
Now for the main event. Since Windows support is currently manual, we will do a quick "Download & Path" setup.

## Step 1: Download Maestro
Download the latest Windows release here: 
[maestro.zip](https://github.com/mobile-dev-inc/maestro/releases/latest/download/maestro.zip)

## Step 2: Extract
Extract the downloaded zip file to your user folder.

Target Location: C:\Users\YOUR_USERNAME\maestro

(Make sure the bin folder is inside. The path should look like C:\Users\YOUR_USERNAME\maestro\maestro\bin)

Note *** Check to see where your actual download folder is before updating system path

## Step 3: Update System Path
We need to tell Windows where to find the maestro command.

Open Command Prompt.

Run the following command (this automatically sets the path for your specific user profile):


```setx PATH "%PATH%;%USERPROFILE%\maestro\maestro\bin"```
## Step 4: Verify
Close your Command Prompt and open a fresh one. Run:


```maestro --version```
If you see a version number (e.g., 1.39.0), you are ready to go!

# 📱 Your First Test
Let's run a test on the Settings app, as every Android device has it.

Connect Device: Plug in your Android phone (USB debugging on) or launch an Android Emulator via Android Studio.

Verify: Type adb devices in your terminal to ensure it is connected.

Create File: Create a new file on your desktop called flow.yaml.

## Write Test: Paste the following code into it:

``` YAML
appId: com.android.settings
---
- launchApp
- tapOn: "Battery" 
- assertVisible: "Battery usage" # Verifies this text exists on screen
```
## Run: Execute the test from your terminal:

``` shell
cd Desktop
maestro test flow.yaml
You should see the Settings app open and the clicks happen automatically!
```