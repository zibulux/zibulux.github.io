---
layout: post
title: "🎬 ARR Stack 101 : Understanding the Theory Before You Try It"
date: 2025-10-12
categories: [3-Server, 1Ser-Novice]
tags: [Server, Docker, ARR-Stack]
author: <author_mpmk>
image:
    path: /assets/img/server/Theory-ARR-Stack.png
---

## 🤔 Why This is Called an ARR Stack

An **ARR Stack** is called that because it’s a **collection of automated applications** that work together to manage, download, and organize your media content. Each letter **ARR** represents a key type of tool in the ecosystem:

1. **A = Automation / Apps (Sonarr, Radarr, Bazarr, etc.)** 🔄  
   - These apps automate the searching, downloading, and organizing of media.

2. **R = Requests (Prowlarr, Overseerr, etc.)** 🕵️  
   - Request apps manage indexers or content requests, helping you find or request specific media.

3. **R = Retrieval / Downloads (qBittorrent, Transmission, etc.)** ⚡  
   - These apps handle the actual retrieval of media automatically via torrents or Usenet.

Additional tools like **Docker** 🐳 and **Gluetun** 🔒 are often included:  
- **Docker**: Runs all the services easily in isolated containers.  
- **Gluetun**: Secures your downloads through a VPN. 🌐  

So an **ARR Stack** is essentially a **coordinated ecosystem of automated services** that manage your media from discovery to playback. 🎉

---

## 🐳 Docker Is Used for the ARR Stack

Most ARR services run inside **Docker containers** because it makes installation and management super easy.  
With Docker, each service runs in its own isolated environment, so you don’t have to worry about messy dependencies, system conflicts, or complex setups.  
You can update, remove, or rebuild apps in seconds — perfect for home setups. ⚡  

For more information about Docker, go see **[click here](#)**.

---

## 🎬 Plex vs Jellyfin

Both **Plex** and **Jellyfin** are powerful media server solutions that let you organize, stream, and watch your movies, TV shows, and music on almost any device.  
They serve as the final piece of your ARR Stack — where all your downloaded and organized media becomes accessible through a clean, easy-to-use interface.

---

### ⭐ Plex
- **Ease of Use:** One of the biggest advantages of Plex is its super polished and user-friendly interface. It’s plug-and-play — perfect for beginners.  
- **Device Compatibility:** Works on almost every platform imaginable — Smart TVs, phones, consoles, web browsers, and streaming devices like Roku or Fire TV. 📱💻📺  
- **Features:** Offers extras like live TV, remote access, mobile sync, and metadata fetching with rich visuals.  
- **Plex Pass (Paid Option):** Unlocks premium features such as hardware transcoding, intro skipping, mobile sync, and better remote streaming performance. 💎  
- **Privacy:** Your Plex account connects to Plex’s cloud for remote access, which makes setup easier but slightly less private.

> 🧡 **Best for:** Users who want a polished, feature-rich, and easy-to-maintain media experience with minimal setup.

---

### 🐧 Jellyfin
- **Open-Source & Free:** 100% free forever — no subscriptions, no hidden paywalls. 🔓  
- **Privacy-Focused:** All your data and user accounts stay **local** on your own server. No cloud tracking, no external connections, complete control. 🛡️  
- **Customization:** Fully customizable interface, user permissions, themes, and plugins.  
- **Performance:** Efficient and lightweight, though remote streaming setup might require a bit more manual configuration compared to Plex.  
- **Community-Driven:** Constantly improved by a passionate open-source community — new features and plugins appear frequently.

> 💚 **Best for:** Users who value **privacy**, **open-source control**, and **deep customization** over convenience.

---

### ⚖️ Plex vs Jellyfin — Quick Comparison

| Feature | Plex 💎 | Jellyfin 🐧 |
|----------|----------|-------------|
| **Price** | Free + optional Plex Pass | 100% Free & Open-Source |
| **Ease of Setup** | Very easy | Moderate (DIY setup) |
| **Remote Access** | Automatic via Plex Cloud | Manual setup (more private) |
| **Privacy** | Cloud-linked | Fully local |
| **Customization** | Limited | Extensive |
| **Plugins/Extensions** | Some official | Many community-made |
| **Transcoding Support** | Excellent (with Plex Pass) | Hardware support via plugins |
| **Best For** | Convenience & polish | Privacy & control |

---

> 💡 **In short:**  
> - Choose **Plex** if you want a smooth, plug-and-play experience with rich features.  
> - Choose **Jellyfin** if you prefer open-source freedom, total privacy, and control over your setup.

---

## ⚙️ Common ARR Tools and What They Do

Here’s a quick breakdown of the most used apps in the ARR ecosystem:

1. **Prowlarr 🕵️** – Manages indexers (search sources) for all your other ARR apps.  
2. **Sonarr 📺** – Automates finding and downloading new TV show episodes.  
3. **Radarr 🎥** – Does the same as Sonarr but for movies.  
4. **Bazarr 💬** – Automatically finds and downloads subtitles for your content.  
5. **qBittorrent ⚡** – A torrent client used to handle the actual downloads.  
6. **Gluetun 🔒** – A lightweight VPN container that routes your download traffic securely through a VPN. It helps protect your privacy, hides your IP, and keeps your torrenting traffic safe while your automation runs. 🌐  



---

## 🧩 How the ARR Stack Works Together

Here’s a simple diagram showing how the tools interact:

    Prowlarr (Indexers) ──▶ Sonarr (TV Shows) ─┐
                                                │
                                                ├──▶ Gluetun (VPN/Privacy) ─▶ qBittorrent (Downloads) ─▶ Plex / Jellyfin (Media Server)
                                                │
                                                └──▶ Bazarr (Subtitles)                                            
    Prowlarr (Indexers) ──▶ Radarr (Movies) ───┘

### 🔹 Workflow Explanation

1. **Prowlarr 🕵️**  
   - Searches indexers for available TV shows or movies.  

2. **Sonarr 📺 / Radarr 🎥**  
   - Receive information from Prowlarr and decide which episodes or movies to download.  

3. **Gluetun 🔒**  
   - Routes all download traffic through a secure VPN for privacy.  

4. **qBittorrent ⚡**  
   - Handles the actual downloading of torrents to your media folders.  

5. **Bazarr 💬**  
   - Monitors completed downloads to automatically fetch subtitles.  

6. **Plex / Jellyfin 🎬**  
   - Reads the downloaded files and subtitles, making your media available for streaming on all devices.  

> There are also other ARR tools that can complement the stack; these will be covered in the **Advanced ARR-Stack** section on this website. 📚

## 💡 Why You Should Try an ARR Stack at Home

Building an **ARR Stack** is an awesome way to explore **Docker** 🐳 and understand why it’s so useful in real-world setups.  
It’s also a fun project that teaches you **automation**, **networking**, and **self-hosting** — while giving you your very own **media hub** at home. 🏡🎉