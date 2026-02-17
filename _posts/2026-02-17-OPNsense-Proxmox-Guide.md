---
layout: post
title: OPNsense Proxmox Install Guide
date: 2026-02-17
categories: [3-Server]
tags: [Server, Proxmox]
author: <author_rylan>
---


1. **Download the OPNsense ISO**

	- [OPNsense.img.bz2](https://pkg.opnsense.org/releases/26.1/OPNsense-26.1-vga-amd64.img.bz2)
	
	- unzip the .bz2 file with ``` bunzip2 OPNsense-26.1-vga-amd64.img.bz2 ```
    - OR just extract if you're on windows.

2. **Upload the img file to Proxmox.**
	- You cannot upload `.img` files via the Proxmox web GUI's "ISO Images" upload button (it only accepts `.iso`). Therefore we will use SCP
	
	- Upload the OPNsense-25.x-OpenSSL-dvd-amd64.img file to a directory like /tmp on the Proxmox host.

```bash
scp .OPNsense-26.1-vga-amd64.img root@10.x.x.x(proxmox-ip):
```
## 3. **Create an empty VM**
- You can do this in the terminal for simplicity
```bash
qm create 105 --name OPNsense-Router --memory 2048 --net0 virtio,bridge=vmbr0 --ostype l26
```

## 4. **Import the Disk**
-  Reopen a shell on your Proxmox Node
- Run the import command. Replace 100 with your VM ID, filename with your actual .img filename, and local-lvm with your local storage, (could be local-zfs or just local)
 
``` bash 
qm importdisk 105 OPNsense-dvd-amd64.img local-lvm
```

## 5. Attach and resize the disk

The imported disk is currently "detached."

1. Go back to the **Proxmox GUI**.
    
2. Select your OPNsense VM > **Hardware**.
    
3. You will see an **Unused Disk 0** at the bottom. Double-click it.
    
4. **Bus/Device:** Select **VirtIO Block**.
    
5. Click **Add**.
    
6. **Resize:** The `.img` file is likely tiny (approx 3GB). You need to expand it immediately.
    
    - Click the newly added Hard Disk.
        
    - Click **Disk Action > Resize**.
        
    - Size Increment: Add `32` GiB (or however much space you want).
        
7. **Boot Order:**
    
    - Go to **Options > Boot Order**.
        
    - Check/Enable the new `virtio0` disk and drag it to the top.

## 6. First Boot

- Start the VM and open the **Console**.
        
- Login (User: `root` / Password: `opnsense`).
    
- **Expand the Partition:** Since we resized the disk in Proxmox, the OS still thinks it's on a 3GB drive.
    
    - If the OPNsense dashboard/console menu appears, look for an option that says **"Grow filesystem"** or similar (often under System or specific shell commands depending on the image type).
        
    - If there is no menu option, drop to the Shell (Option 8) and run:
        
        Bash
        
        ``` bash
        # This forces OPNsense to resize the root partition to fill the disk
        /usr/local/sbin/opnsense-growfs
        ```
        
    - Reboot the VM: `reboot`.