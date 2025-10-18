---
title: "🧩 Plex or Jellyfin? Here’s How to Set Up Your Media Server in Minutes"
date: 2025-10-18
categories: [3-Server, 3Ser-Labs]
tags: [Server, Docker, ARR-Stack]
author: <author_mpmk>
image:
    path: /assets/img/server/Labs-ARR-Stack.png
---

## 📺 What is Media Streaming?
Media streaming is the process of delivering video or audio content over the internet in real-time — just like **Netflix**, **Disney+**, or **Spotify**.  
Instead of downloading a file before watching it, streaming allows you to **watch instantly** while the data is continuously transmitted from a media server to your device.

With tools like **Plex** or **Jellyfin**, you can host your **own private Netflix**, streaming your movies, TV shows, and music directly from your home server.

---

## ⚙️ Installation Methods

You can install your media server in several ways depending on your system:

- 🐳 **Container installation (Docker)**  
- 🪟 **Windows installation**  
- 🐧 **Linux installation**  

This guide references both **Jellyfin** and **Plex**, giving you the freedom to choose your preferred setup.

---

### 🎮 GPU Passthrough or Not?

When setting up a media server, you can decide whether to enable **GPU passthrough** for hardware transcoding.  

> ⚠️ **Important:** GPU passthrough is mainly relevant for **Docker containers** or **virtual machines** because these environments need explicit access to the host machine’s GPU resources.  
> If you install your media server directly on a regular OS (without virtualization), you **don’t need to worry about this** — the application will automatically have access to your machine’s resources.

- 🧠 **Without GPU passthrough:**  
  Transcoding is handled by your **CPU**, which may cause minor latency during playback — but it’s the simplest and fastest way to get streaming working.  
  For small home setups or if only a few users stream simultaneously, this method is **efficient and easy**.

- ⚡ **With GPU passthrough:**  
  Hardware acceleration uses your GPU to handle transcoding tasks, greatly improving performance for multiple users or 4K streams.  
  (This setup requires extra configuration and compatible hardware.)


---

## 🐳 Docker container Installation 

### 🐳 Jellyfin Docker Compose (No GPU Passthrough)

```yaml
services:
  jellyfin:
    image: jellyfin/jellyfin
    container_name: jellyfin
    restart: unless-stopped
    ports:
      - 8096:8096
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - ./jellyfin/config:/config
      - ./jellyfin/cache:/cache
      - Media-Drive:/media

volumes:
  Media-Drive:
    driver_opts:
      type: cifs
      o: username=${USERNAME},password=${PASSWORD},uid=${PUID},gid=${PGID}
      device: ${DEVICE}
```
>💡 Note:
> - You can copy everything into a new docker-compose.yml file, or just take the jellyfin: portion and paste it into the arr-stack docker-compose.yml file from the previous post [here]({% post_url 2025-10-12-Server-Labs-Arr-Stack-basic %}).

---

### 🐳 Plex Docker Compose (No GPU Passthrough)

```yaml
services:
  plex:
    image: lscr.io/linuxserver/plex:latest
    container_name: plex
    restart: unless-stopped
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
      - VERSION=docker
      - PLEX_CLAIM=       # optional
    volumes:
      - Media-Drive:/media

volumes:
  Media-Drive:
    driver_opts:
      type: cifs
      o: username=${USERNAME},password=${PASSWORD},uid=${PUID},gid=${PGID}
      device: ${DEVICE}
```
>💡 Notes:
> - You can copy everything into a new docker-compose.yml file, or just take the plex: portion and paste it into the arr-stack docker-compose.yml file from the previous post here.
> - PLEX_CLAIM: You can optionally obtain a claim token from [https://plex.tv/claim](https://plex.tv/claim)
 and insert it here. Keep in mind that these tokens expire within 4 minutes.

---

### ⚙️ .env File Configuration

Next, open (or create) the .env file in the same directory as your docker-compose.yml.
This file securely stores your environment variables and credentials — keeping your Compose file clean and organized.

```bash
# 💾 Volume Mount Credentials (for network drives)
USERNAME=                               # Network drive username
PASSWORD=                               # Network drive password
DEVICE=                                 # Network path, e.g.: //192.168.1.50/Media-Drive

# 🕒 Global Settings
TZ=America/Toronto                      # Timezone, e.g., America/Toronto
PUID=1000                               # User ID for Linux permissions
PGID=1000                               # Group ID for Linux permissions
```
---

## 📦 Host Machine or VM Install

### 🪟 Windows Jellyfin Installation

If you prefer installing your media server directly on Windows, you can follow this video tutorial that walks you through the entire process, step by step.  

🎥 **Watch this video for the Windows setup:**  
{% include embed/youtube.html id="caz_Fv7fUPc" %}  

---

### 🐧 Linux Jellyfin Installation

For Linux users, installing your media server directly on your system is straightforward.  
You can follow a video tutorial that guides you through the full setup process for **Jellyfin**.

🎥 **Watch this video for the Linux setup:**  
{% include embed/youtube.html id="XN_0qmA9mno" %}

---

### 🪟 Windows Plex Installation

If you prefer installing your media server directly on Windows, you can follow this video tutorial that walks you through the entire process, step by step.  

🎥 **Watch this video for the Windows setup:**  
{% include embed/youtube.html id="ayMN-weiv68" %}  

---

### 🐧 Linux Plex Installation

For Linux users, installing your media server directly on your system is straightforward.  
You can follow a video tutorial that guides you through the full setup process for **Plex**.

🎥 **Watch this video for the Linux setup:**  
{% include embed/youtube.html id="QEP5Tq78cHw" %}

---

## 💾 Permanently Mount a Network Drive 

Mounting a network drive is highly recommended for an **ARR-stack** setup (Radarr, Sonarr, etc.) because:

- 📂 **Centralized media storage:** All your media files are stored in a single location accessible by all services.  
- ⚡ **Consistency:** Containers and applications can reliably read/write files without needing to copy them locally.  
- 🔄 **Easier automation:** Tools like Radarr, Sonarr, and Plex/Jellyfin can automatically manage, move, and organize your media directly on the network drive.  
- 🛡️ **Separation of concerns:** Keeps your media separate from the system drive, reducing the risk of filling up your OS disk and making backups simpler.
- 🏠 **NAS usage for reliability:** Using a NAS as your network drive provides **redundancy** and improves **data reliability**, ensuring your media library is safe and always available for streaming.

If you have a network drive, here’s how to map it: 

### 🪟 **Windows:** Watch this video to see how to map your network drive on Windows machine:
{% include embed/youtube.html id="UoAV7CZcHgU" %}

---

### 🐧 Permanently Mounting a Network Drive on Ubuntu

To ensure your network drive is always available on your Ubuntu machine, you can permanently mount it by editing the `fstab` file:

```bash
sudo nano /etc/fstab
```
Then, add the following line at the very end of the file:

```bash
//192.168.1.50/Media_Drive /Change/me cifs username=<username>,password=<password> 0 0  # Update network path, username & password as needed 
```

{% include embed/youtube.html id="v2WMnZoiGog" %}


> 💡 **Tip:** Make sure the media server has proper read/write access to the mapped drives to stream your files correctly.

---

## 🖥️ GPU Passthrough for Virtual Machines (Hypervisors)

If you are running your media server inside a **virtual machine**, you can enable **GPU passthrough** to allow the VM to use the host machine’s GPU.  
This enables hardware transcoding, improving performance for multiple streams or 4K content.

For example, we use **Proxmox** as the hypervisor in this guide because it is **free and open-source**, but other solutions exist, such as **ESXi** or **Unraid**.  
⚠️ **Note:** On Proxmox, only one VM at a time can use a given GPU from the host.  
For other hypervisors, behavior may differ — make sure to do your research according to the hypervisor you are using.

🎥 **Watch this video to learn how to set up GPU passthrough on your Proxmox VM:**  
{% include embed/youtube.html id="Il6HhOCfDjI" %}


> 💡 **Tip:** GPU passthrough is mainly relevant for containers or virtual machines.  
> If your media server is installed directly on your host OS, you don’t need to worry about this — the application automatically has access to your system’s GPU resources.


---