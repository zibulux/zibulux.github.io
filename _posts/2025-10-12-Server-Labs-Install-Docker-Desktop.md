---
title: "🪟 Installing Docker Desktop on Windows — Step by Step"
date: 2025-10-12
categories: [3-Server, 3Ser-Labs]
tags: [Server, Docker]
author: <author_mpmk>
image:
    path: /assets/img/server/Labs-Docker-Desktop.png
---

## Installing Docker Desktop on Windows — Quick Start

Installing Docker Desktop is quite simple!  
You can follow the **official Docker Hub documentation** for a complete step-by-step guide:  
<a href="https://docs.docker.com/desktop/setup/install/windows-install" target="_blank">
👉 Docker Desktop Installation Docs
</a>

> 💡 **Tip:** You can skip the video if you want and go straight to the section below to follow the step-by-step installation checklist.

---

## 🎥 Optional: Watch the Video Tutorial

If you prefer a visual guide, **Bret Fisher** walks through the Docker Desktop installation process in his course:

{% include embed/youtube.html id='rATNU0Fr8zs' %}

Source: Bret Fisher, Agentic DevOps

---

## ✅ Docker Desktop Setup Checklist

### Make sure you complete the following steps to install Docker Desktop properly on Windows:

#### 1. ⚙️ **Enable Virtualization in BIOS**  
   - Usually found under CPU or Advanced settings.  
   - Required for Docker to run containers efficiently.

#### 2. 💾 **Download and run the Docker Desktop `.exe` installer**  
- → It will guide you through the setup wizard.

#### 3. 🧱 **Enable WSL 2 (Windows Subsystem for Linux)**  
   - Run PowerShell as Administrator:  
     ```bash
     wsl --install
     ```
   - Reboot your system if prompted.

#### 4. 🐧 **Link Docker Desktop with WSL 2**  
   - Open Docker Desktop → **Settings → General** → ensure  
     ✅ *“Use the WSL 2 based engine”* is checked.  
   - Under **Resources → WSL Integration**, select which distros Docker can access.

#### 5. 🚀 **Verify the installation**
   - Open a terminal(CMD) and run:  
     ```bash
     docker version
     ```
>💡If you see a Docker version number in the terminal output, Docker is successfully installed and ready to use!

---
