---
title: Intro to Cloudflare Tunnels — Easy remote access for your self hosted applications.
date: 2025-11-20
categories: [3-Server]
tags: [Server]
author: <author_rylan>
---


# ☁️ Cloudflare Tunnel for Plex: Minimal Docker Compose Guide

This guide details the simplest way to securely expose your self-hosted docker applications to the internet using a **Cloudflare Tunnel** and **Docker Compose**, ***without needing to open any firewall ports.***

---

## 🚀 Prerequisites

Before starting, ensure you have the following:

- A registered **domain name** managed by Cloudflare DNS.
    
- **Docker** and **Docker Compose** installed on your homelab server.
    

---

## Step 1: Create the Tunnel and Get the Token

The Cloudflare Zero Trust dashboard is used to generate the necessary authentication token.

1. **Log in to Cloudflare** and navigate to the **Zero Trust** dashboard.
    
2. Go to **Networks** > **Manage Tunnels** and click **Add a Tunnel**.
    
3. **Name the Tunnel** (e.g., `homeserver-tunnel`) and click **Save Tunnel**.
    
4. **Copy the Token:**
    
    - Select **Docker** as your installation environment.
        
    - Cloudflare will display a command. **Copy the long, unique string** (starting with `ey...`) which is your **Tunnel Token**.
        
    - _You will use this token in the next step. You can ignore the Public Hostname section for now._
        

---

## Step 2: Configure Local Files on Your Server

Create a dedicated directory for your tunnel configuration, and create a new docker compose file for it:

```bash
mkdir ~/cloudflare-tunnel
cd ~/cloudflare-tunnel
touch docker-compose.yml
```

Here is a minimal docker compose file to get you started, paste this into your docker-compose.yml file:


```yml
services:

  cloudflare-tunnel:
      image: cloudflare/cloudflared:latest
      container_name: cloudflare-tunnel
      restart: unless-stopped
      command: tunnel --no-autoupdate run --token PASTE_TOKEN_HERE
```


---

## Step 3: Deploy and Verify

Use Docker Compose to launch your tunnel service.

1. Deploy the Service:
    
    Navigate to your configuration directory (~/cloudflare-tunnel) and run the deployment command:

   ```
   docker compose up -d
   ```
    
2. Verify Status:

    Refresh the Cloudflare Tunnel Dashboard to see if your tunnel is showing "HEALTHY":
    ![HEALTHY Cloudflare Tunnel](/assets/img/server/Cloudflare-Tunnel.png)

    
    You can also check the container logs to ensure the tunnel is running and connected successfully:
    
     ```
    docker logs cloudflare-tunnel
    ```

    

---