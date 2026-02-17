---
layout: post
title: Terraform Proxox BPG Setup Guide
date: 2026-02-17
categories: [3-Server]
tags: [Server, Proxmox]
author: <author_mpmk>
---

# Create Clone VM in proxmox

## SSH Into Proxmox node

```bash

ssh root@192.168.x.x

```


## Create folder & file at this location

```bash

mkdir -p /var/lib/vz/snippets
touch /var/lib/vz/snippets/vendor-data.yaml
chmod +x /var/lib/vz/snippets/vendor-data.yaml

cat << EOF > /var/lib/vz/snippets/vendor-data.yaml
#cloud-config
packages:
  - qemu-guest-agent
package_update: true
power_state:
  mode: reboot
  timeout: 30
EOF

```

## Create download folder & script

```bash

mkdir /var/lib/vz/snippets/download
cd /var/lib/vz/snippets/download

wget https://raw.githubusercontent.com/trfore/proxmox-template-scripts/refs/heads/main/scripts/image-update
wget https://raw.githubusercontent.com/trfore/proxmox-template-scripts/refs/heads/main/scripts/build-template

# For systems using ZFS storage, a script with local-zfs defaults is available:
# wget https://raw.githubusercontent.com/trfore/proxmox-template-scripts/refs/heads/main/scripts-zfs/build-template

chmod +x image-update build-template

./image-update -d ubuntu -r 24
./build-template --id 9000 --name ubuntu24 --img /var/lib/vz/template/iso/ubuntu-24.04-server-cloudimg-amd64.img --bios ovmf

```

>[!NOTE]
> If wget doen's work I have copy them into this folder of the repo just import them into your **/var/lib/vz/snippets/download** directory of your proxmox node.

## Create Terraform role/account on Proxmox Node

```bash

pveum user add terraform@pve

pveum role add Terraform -privs "Realm.AllocateUser, VM.PowerMgmt, VM.GuestAgent.Unrestricted, Sys.Console, Sys.Audit, Sys.AccessNetwork, VM.Config.Cloudinit, VM.Replicate, Pool.Allocate, SDN.Audit, Realm.Allocate, SDN.Use, Mapping.Modify, VM.Config.Memory, VM.GuestAgent.FileSystemMgmt, VM.Allocate, SDN.Allocate, VM.Console, VM.Clone, VM.Backup, Datastore.AllocateTemplate, VM.Snapshot, VM.Config.Network, Sys.Incoming, Sys.Modify, VM.Snapshot.Rollback, VM.Config.Disk, Datastore.Allocate, VM.Config.CPU, VM.Config.CDROM, Group.Allocate, Datastore.Audit, VM.Migrate, VM.GuestAgent.FileWrite, Mapping.Use, Datastore.AllocateSpace, Sys.Syslog, VM.Config.Options, Pool.Audit, User.Modify, VM.Config.HWType, VM.Audit, Sys.PowerMgmt, VM.GuestAgent.Audit, Mapping.Audit, VM.GuestAgent.FileRead, Permissions.Modify"

pveum aclmod / -user terraform@pve -role Terraform

pveum user token add terraform@pve provider --privsep=0


```

> [!NOTE]
> Make sure you copy the token value, as it will not be displayed again.

---

## Go back into your Computer

## Copy variable.tfvars.example and change them

CD into your DevFlow directory

```bash

cp terraform/variable.tfvars.example terraform/variable.tfvars

```

### Change all the variable for your setup

- proxmox_url
- node_name
- proxmox_api_token

---