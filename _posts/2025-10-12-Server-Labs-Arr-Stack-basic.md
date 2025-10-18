---
layout: post
title: "🧱 Basic ARR Stack Docker Compose — Secure, Automated, and Self-Hosted"
date: 2025-10-12
categories: [3-Server, 3Ser-Labs]
tags: [Server, Docker, ARR-Stack]
author: <author_mpmk>
image:
    path: /assets/img/server/Labs-ARR-Stack.png
---

## Intro
This post is your step-by-step guide to building your own self-hosted ARR Stack — a fully automated media setup that runs privately on your own server.  
You’ll learn how to organize your folders, configure Docker Compose, and connect all the ARR tools through Gluetun, a VPN container that keeps your downloads secure and anonymous.

🎥 Watch this video for the full explanation and setup guide.  
You can scroll down to find the `docker-compose.yml`, `.env`, and `config.toml` files to complete your setup.  
You can also skip the part about **NZBGet** — you won’t really need it.

{% include embed/youtube.html id="twJDyoj0tDc" %}

---

## 🧠 Overview

This setup includes:

- **Gluetun** 🛡️ – VPN gateway for all other services  
- **qBittorrent** ⚡ – Torrent client handling all downloads  
- **Prowlarr** 🕵️ – Central indexer manager  
- **Sonarr** 📺 – TV show automation  
- **Radarr** 🎥 – Movie automation  
- **Bazarr** 💬 – Subtitle downloader  
- **Overseerr / Jellyseerr** 🧭 – Media request managers for Plex or Jellyfin  
- **Deunhealth** ❤️ – Health monitor that restarts unhealthy containers  

All services (except Deunhealth) share the **Gluetun network**, which means all their internet traffic is routed through the VPN.

---

## 🗂️ Folder Directory Overview

Before launching the stack, make sure your directory structure looks like this:

```
arr-stack/
│
├── docker-compose.yml
├── .env
│
├── gluetun/
│   └── config.toml
│
├── qbittorrent-config/
├── prowlarr-config/
├── sonarr-config/
├── radarr-config/
├── bazarr-config/
├── overseerr-config/   # Only if using Plex
├── jellyseerr-config/  # Only if using Jellyfin
│
├── Media-Drive/        # (Optional) External or mounted media drive
│   ├── Movies/
│   ├── TV Shows/
│   └── downloads/
│       └── qbittorrent/
│           ├── completed/
│           ├── incomplete/
│           └── torrents/
```
>💡 Notes:
>- The .env file should contain your VPN credentials, drive paths, and environment variables like PUID, PGID, and TZ.
>- Bach *-config folder will automatically store the configuration data for that specific container.
>- The Media-Drive can be a local folder or a CIFS/SMB mount to a NAS or external drive.

---

## ⚙️ The Docker Compose File

Here’s the full configuration you can use:

```yaml
services:
  gluetun:
    image: qmcgaw/gluetun:v3
    container_name: gluetun
    restart: unless-stopped
    cap_add:
      - NET_ADMIN
    devices:
      - /dev/net/tun:/dev/net/tun
    ports:
      - 8001:8000 # Gluetun
      - 8080:8080 # qbittorrent web interface
      - 6881:6881 # qbittorrent torrent port
      - 9696:9696 # prowlarr
      - 8989:8989 # Sonarr
      - 7878:7878 # Radarr
    volumes:
      - ./gluetun:/gluetun
      - ./gluetun/config.toml:/gluetun/auth/config.toml
    environment:
      - VPN_SERVICE_PROVIDER=${VPN_SERVICE_PROVIDER}
      - VPN_TYPE=${VPN_TYPE}
      - OPENVPN_USER=${OPENVPN_USER}
      - OPENVPN_PASSWORD=${OPENVPN_PASSWORD}
      - SERVER_COUNTRIES=${SERVER_COUNTRIES}
      - SERVER_CITIES=${SERVER_CITIES}
      - TZ=${TZ}
      - UPDATER_PERIOD=24h
      - HEALTH_VPN_DURATION_INITIAL=120s
      # IMPORTANT: Add these if Gluetun's internal firewall is active and blocking inbound connections
      # - FIREWALL_VPN_INPUT_PORTS=8989,7878,9696,8080,6881
    healthcheck:
      test: ping -c 1 www.google.com || exit 1
      interval: 60s
      timeout: 20s
      retries: 5

  deunhealth:
    image: qmcgaw/deunhealth
    container_name: deunhealth
    network_mode: "none"
    environment:
      - LOG_LEVEL=info
      - HEALTH_SERVER_ADDRESS=127.0.0.1:9999
      - TZ=${TZ}
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock

  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:latest
    container_name: qbittorrent
    restart: unless-stopped
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
      - WEBUI_PORT=9090
      - TORRENTING_PORT=6881
    volumes:
      - ./qbittorrent-config:/config
      - Media-Drive:/data # Directory link to a external Hard drive, change path in .env file
    network_mode: service:gluetun

  prowlarr:
    image: lscr.io/linuxserver/prowlarr:latest
    container_name: prowlarr
    restart: unless-stopped
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - ./prowlarr-config:/config
    network_mode: service:gluetun

  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    restart: unless-stopped
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - ./sonarr-config:/config
      - Media-Drive:/data # Directory link to a external Hard drive, change path in .env file
    network_mode: service:gluetun

  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    restart: unless-stopped
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - ./radarr-config:/config
      - Media-Drive:/data # Directory link to a external Hard drive, change path in .env file
    network_mode: service:gluetun

  bazarr:
    image: lscr.io/linuxserver/bazarr:latest
    container_name: bazarr
    restart: unless-stopped
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - ./bazarr-config:/config
      - Media-Drive:/data # Directory link to a external Hard drive, change path in .env file
    network_mode: service:gluetun

  overseerr: # Can remove if use Jellyfin
    image: sctx/overseerr:latest
    container_name: overseerr
    restart: unless-stopped
    ports:
      - 5055:5055
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - ./overseerr-config:/app/config
    
  jellyseerr: # Can remove if use Plex
    image: fallenbagel/jellyseerr:latest
    container_name: jellyseerr
    restart: unless-stopped
    ports:
      - 5065:5055
    environment:
      - LOG_LEVEL=debug
      - TZ=${TZ}
    volumes:
      - /jellyseerr-config:/app/config

volumes:
  Media-Drive:
    driver_opts:
      type: cifs
      o: username=${USERNAME},password=${PASSWORD},uid=${PUID},gid=${PGID}
      device: ${DEVICE_PATH}
```

> 💡 **Note:**  
> If you run into a port conflict (for example, if some ports are already in use on your computer),  
> you can safely change them — just make sure to remember the new values,  
> as you’ll need to update them consistently across your configuration files.
>
> 🧩 **Example:**  
> If you want to change the **Sonarr** port, simply edit the first number in the mapping — for example:  
> ```yaml
> ports:
>   - "8798:8989"
> ```
> Here, `8798` is the external port on your machine, and `8989` is the internal port used inside the container.

---

## ⚙️ .env File Configuration

Next, go to file named .env in the same directory as your docker-compose.yml.
This file stores your environment variables and credentials securely — it keeps your Compose file clean and easy to manage.

```bash
# 🌐 Gluetun VPN Settings
VPN_SERVICE_PROVIDER=                   # Your VPN provider, e.g., protonvpn, nordvpn, mullvad
VPN_TYPE=                               # VPN type, usually openvpn, wireguard
OPENVPN_USER=                           # Your VPN username, e.g., "protonvpn1234"
OPENVPN_PASSWORD=                       # Your VPN password, e.g., "a1b2c3d4e5f6"
SERVER_COUNTRIES=                       # Country to connect to, e.g., Canada
SERVER_CITIES=                          # Specific cities (Toronto,Montreal,Vancouver)

# 💾 Volume Mount Credentials (for network drives)
USERNAME=                               # Network drive username
PASSWORD=                               # Network drive password
DEVICE_PATH=                            # Network path, e.g.: //192.168.1.50/Media-Drive

# 🕒 Global Settings
TZ=America/Toronto                      # Timezone, e.g., America/Toronto
PUID=1000                                # User ID for Linux permissions
PGID=1000                                # Group ID for Linux permissions
```

## 🔗 Find Your VPN Provider Information

To configure Gluetun correctly, you need the right server and login details from your VPN provider.
Here are direct links to some of the most common ones — just click on yours:

- <a href="https://github.com/qdm12/gluetun-wiki/blob/main/setup/providers/nordvpn.md" target="_blank"><strong>NordVPN Setup Guide</strong></a><br>
- <a href="https://github.com/qdm12/gluetun-wiki/blob/main/setup/providers/protonvpn.md" target="_blank"><strong>ProtonVPN</strong></a><br>
- <a href="https://github.com/qdm12/gluetun-wiki/blob/main/setup/providers/surfshark.md" target="_blank"><strong>Surfshark</strong></a><br>
- <a href="https://github.com/qdm12/gluetun-wiki/blob/main/setup/providers/mullvad.md" target="_blank"><strong>Mullvad</strong></a><br>

👉 You can find the complete list of supported VPN providers in the official Gluetun documentation:  
<a href="https://github.com/qdm12/gluetun-wiki/tree/main/setup/providers" target="_blank">Click Here</a>


---

## ⚙️ Gluetun config.toml Setup

Once your .env file is ready, you’ll also need to configure Gluetun’s internal permissions.
This ensures that all your ARR tools (Sonarr, Radarr, etc.) can safely communicate through the VPN connection.
 
Here’s what your **config.toml** should look like 👇

```yaml
[[roles]]
name = "qBittorrent"
routes = ["GET /v1/openvpn/portforwarded"] # May need to change the path if using wiregard go see de documentation
auth = "none"

[[roles]]
name = "Radarr"
routes = ["GET /v1/openvpn/portforwarded"] # May need to change the path if using wiregard go see de documentation
auth = "none"

[[roles]]
name = "Sonarr"
routes = ["GET /v1/openvpn/portforwarded"] # May need to change the path if using wiregard go see de documentation
auth = "none"

[[roles]]
name = "Prowlarr"
routes = ["GET /v1/openvpn/portforwarded"] # May need to change the path if using wiregard go see de documentation
auth = "none"

[[roles]]
name = "Bazarr"
routes = ["GET /v1/openvpn/portforwarded"] # May need to change the path if using wiregard go see de documentation
auth = "none"
```

> Documentation [Here](https://github.com/qdm12/gluetun-wiki/blob/main/setup/advanced/control-server.md)

---

## 🚀 Launch Your ARR Stack

Now that everything is configured — your folders, .env, docker-compose.yml and config.toml — it’s time to bring your stack to life!

Make sure you’re in the root directory of your project (where your docker-compose.yml file is located).
If you followed this guide, that folder should be called arr-stack/.

Then, run the following command 👇

```bash
docker compose up -d
```

This command will start pulling and building all your containers (Gluetun, Sonarr, Radarr, qBittorrent, etc.) in detached mode — meaning they’ll run in the background.

---

## 🧱 Test your stack

Once it’s done, open your browser and check that everything is working:

- Sonarr: <a href="http://localhost:8989" target="_blank">http://localhost:8989
</a>
- Radarr: <a href="http://localhost:7878" target="_blank">http://localhost:7878
</a>
- qBittorrent: <a href="http://localhost:8080" target="_blank">http://localhost:8080
</a>
- Prowlarr: <a href="http://localhost:9696" target="_blank">http://localhost:9696
</a>
- Bazarr: <a href="http://localhost:6767" target="_blank">http://localhost:6767
</a>
- Overseerr: <a href="http://localhost:5055" target="_blank">http://localhost:5055
</a> (if using Plex)
- Jellyseerr: <a href="http://localhost:5065" target="_blank">http://localhost:5065
</a> (if using Jellyfin)

>💡 Note: 
>If your stack is running on another server or machine, simply replace localhost with the IP address of that server.
>Example: http://192.168.1.50:8989 for Sonarr.

>💡 Reminder:
>If you’re using Plex, remove Jellyseerr from your stack.
>If you’re using Jellyfin, remove Overseerr — they serve the same purpose but for different media servers.
>
> 🧩 It’s also recommended to have your **Plex** or **Jellyfin** server set up **before** configuring **Overseerr** or **Jellyseerr**.

--- 

⚙️ Next Step — Configure Your ARR Applications

Now that your containers are up and running, the next step is to configure each application so your stack becomes fully automated.
This process will connect all your ARR tools together — from torrent management to media requests — for a hands-free experience.

Here’s the recommended order to set everything up 👇

1. **⚡qBittorrent Setup** — Configure your download client and set up paths correctly
2. **🕵️ Prowlarr Setup** — Add indexers and link them to Sonarr and Radarr
3. **📺 Sonarr Configuration Guide** — Automate your TV show downloads
4. **🎬 Radarr Configuration Guide** — Automate your movie library
5. **🎟️ Overseerr Setup** — Allow users to request movies and shows (for Plex users)
6. **🍿 Jellyseerr Setup** — Same as Overseerr, but built for Jellyfin
7. **▶️ [Plex Configuration Guide]({% post_url 2025-10-12-Server-Labs-Install-Media-Stream %})** — Set up your media server and link libraries
8. **🎞️ [Jellyfin Configuration Guide]({% post_url 2025-10-12-Server-Labs-Install-Media-Stream %})** — Open-source alternative to Plex for full control

>💡 Important Note:
>If you’re using Plex, remove Jellyseerr (since Overseerr already handles requests).
>If you’re using Jellyfin, remove Overseerr (and keep Jellyseerr instead).

---

## 💭 Final Thoughts

You now have your own self-hosted ARR Stack — automated, private, and running entirely under your control. 🎉
It’s a great way to learn Docker and understand how each service connects together.

If you hit any issues during setup, here are some great YouTube videos to help:

### 🛡️ Gluetun issue:
{% include embed/youtube.html id="9dJPOd0XbN8" %}
{% include embed/youtube.html id="_tz2wUFT8VQ" %}

---

### 🕵️ Overseerr Setup
{% include embed/youtube.html id="TfuwG-OHccI" %}


Once everything’s working, sit back and enjoy — your media is now fully automated. 🚀

---