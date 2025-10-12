---
title: "🐧 Installing Docker on Ubuntu Linux — The Complete Beginner Guide"
date: 2025-10-12
categories: [3-Server, 3Ser-Labs]
tags: [Server, Docker]
author: <author_mpmk>
image:
    path: /assets/img/server/Labs-Docker-Linux.png
---

## Installing Docker on Ubuntu Linux — Quick Start

Installing Docker on Linux is straightforward, but it requires a few terminal commands.  
You can follow the **official installation guide** from Docker here:  
👉 <a href="https://docs.docker.com/engine/install" target="_blank">Docker Engine Installation Docs</a>

> 💡 **Tip:** You can skip the video if you want and go straight to the section below to follow the step-by-step installation checklist.

---

## 🎥 Optional: Watch the Video Tutorial

If you prefer a visual guide, **Bret Fisher** walks through the Linux installation process in his Docker course:

{% include embed/youtube.html id='YeF7ObTnDwc' %}

Source: Bret Fisher, Agentic DevOps


---

## ✅ Ubuntu Linux Docker Setup Checklist

### Follow these steps to install Docker properly on Ubuntu Linux:

#### 1. 🧹 **Uninstall any old Docker versions**
  ```bash
    sudo apt remove docker docker-engine docker.io containerd runc
  ```

#### 2. 🧱 Update your system and install required packages:
  ```bash
    sudo apt update && sudo apt install ca-certificates curl gnupg lsb-release
  ```

#### 3. 🔑 Add Docker’s official GPG key:
  ```bash
    sudo mkdir -p /etc/apt/keyrings
  ```
  ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
  ```

#### 4. 📦 Add the Docker repository:
  ```bash
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
      https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  ```

#### 5. ⚙️ Install Docker Engine, CLI, and Compose:
  ```bash
    sudo apt update && sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
  ```

#### 6. 🔓 Enable non-root access (optional but recommended):
  ```bash
    sudo usermod -aG docker $USER
  ```
  ```bash
    newgrp docker
  ```

#### 7. 🚀 Verify the installation
  ```bash
    docker version
  ```
>💡If you see a Docker version number in the terminal output, Docker is successfully installed and ready to use!

---