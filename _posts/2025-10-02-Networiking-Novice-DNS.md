---
title: 🌐 DNS (Domain Name System)
date: 2025-10-02
categories: [2-Networking, 1Net-Novice]
tags: [networking, services]
author: <author_mpmk>
---

## 🧾 Introduction to DNS
**What is DNS?**  
DNS converts **domain names** (e.g., `google.com`) into **IP addresses** (e.g., `172.217.160.142`).  
It works like the **phone book of the Internet**.    

---

## ⚙️ DNS Components
- **Forwarders**: Relay queries to external DNS servers.  
- **Zones**:  
  - Forward lookup zones → Hostname/Domain → IP address.  
  - Reverse lookup zones → IP address → Hostname/Domain.  
- **DNS Records**:  
  - `A` → Domain to IPv4  
  - `AAAA` → Domain to IPv6  
  - `CNAME` → Domain alias  
  - `MX` → Mail server  
  - `TXT` → Textual information  

---

## 🔄 How DNS Works
1. **DNS Resolver Server** → Usually provided by the ISP.  
2. **DNS Root Server** → The “master” of DNS hierarchy.  
3. **TLD DNS Server (Top-Level Domain)** → Manages `.com`, `.org`, `.net`, etc.  
4. **DNS Authoritative Server** → Holds the DNS records of the destination.

![DNS hierarchy diagram](/assets/img/networking/DNS-Schema.png){:width="512px"}

---

## 🔐 DNS Security and Management
- **DDNS (Dynamic DNS)** → Allows computers to automatically update their DNS records.  
- **DNSSEC (Domain Name System Security Extensions)** → Protects against threats such as **DNS cache poisoning**.  

---

## 🛠 CLI Tools for DNS
### `nslookup`
Query DNS servers to get information about domain names.  
- ✅ Verify DNS records for a domain  
- ✅ Determine the IP address of a domain  
- ✅ Test DNS resolution  

⚠️ **Limitation**:  
`nslookup` is considered **obsolete** and has been replaced by **`dig`** (Domain Information Groper) in many systems.  

### `dig`
- Provides more **advanced features**  
- Offers more **detailed output**  

---

## ❓ Questions?
