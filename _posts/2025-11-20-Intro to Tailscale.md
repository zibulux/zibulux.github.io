**Tailscale** is a modern, zero-config VPN built on top of **WireGuard**.  
It lets your devices connect to each other privately as if they were all on the same LAN — anywhere in the world.

___
### ⚙️ Core Principles:

✔ Your own private network  (Tailnet)
✔ No port forwarding  
✔ No public IP required  
✔ No custom VPN configuration  
✔ Works across Windows, Linux, macOS, iOS, Android, Docker, VMs, and Proxmox, etc.

___
### ✅ Ubuntu / Debian Install
1.  Install
``` bash
curl -fsSL https://tailscale.com/install.sh | sh
```

2. Add the server to your "tailnet"
```bash
sudo tailscale up
```

3. **This will give you a login URL — open it in a browser, log in, and the server joins your network.**

4. You will now see all the machines that are apart of your tailnet, and their associated IP's. (100.100.100.xxx) Add a second device to your tailnet and try to ping!

___
### ⭐ Advanced Options

1. Use server as an exit node:

An **exit node** is a device in your Tailscale network that you choose to route **all of your internet traffic** through. Think of it as a personal VPN server — but without the complexity of setting one up.

```bash
sudo tailscale up --advertise-exit-node
```

2.  Advertise LAN access (subnet router) - replace IP with your home network subnet

```bash
sudo tailscale up --advertise-routes=192.168.x.x/24
```

