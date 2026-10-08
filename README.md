# Linux Deployment Server

**Automated Ubuntu PXE deployment infrastructure using iPXE, nginx and Autoinstall.**

<p align="center">
  <img src="https://cdn.simpleicons.org/ubuntu" height="32" alt="Ubuntu">&nbsp;Ubuntu
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="https://cdn.simpleicons.org/linux" height="32" alt="Linux">&nbsp;Linux
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="https://cdn.simpleicons.org/gnubash" height="32" alt="Bash">&nbsp;Bash
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="https://cdn.simpleicons.org/nginx" height="32" alt="nginx">&nbsp;nginx
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="https://cdn.simpleicons.org/yaml" height="32" alt="YAML">&nbsp;YAML
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="https://cdn.simpleicons.org/fortinet" height="32" alt="Fortinet">&nbsp;Fortinet
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="https://cdn.simpleicons.org/rustdesk" height="32" alt="RustDesk">&nbsp;RustDesk
</p>

---

> [!NOTE]
> Internal deployment infrastructure. Server: `lns-ds-001` (`172.16.110.10/24`).

---

## 📖 Overview

This repository contains the configuration and deployment assets for an Ubuntu PXE environment. Physical PCs boot through **dnsmasq → iPXE**, select an installation profile, load the Ubuntu kernel and initrd over HTTP, and complete installation through **Autoinstall / NoCloud**.

### Key Features

* **PXE Boot** – DHCP/TFTP bootstrapping with `dnsmasq` and iPXE.
* **HTTP Installation** – Ubuntu media and deployment data served by nginx.
* **Ubuntu Autoinstall** – Desktop, Dual-Boot and Server profiles.
* **Desktop Provisioning** – SSSD/LDAP, GPFS/NFS, FortiClient and RustDesk.
* **Dual-Boot Support** – Interactive storage while preserving the existing Windows installation.

---

## 🏗️ Architecture

```text
Physical PC
    │
    │ PXE / DHCP
    ▼
 dnsmasq
    │
    │ iPXE
    ▼
 boot.ipxe
    │
    ├── Ubuntu Desktop 26.04.1
    ├── Ubuntu Desktop 26.04.1 - Dual Boot
    └── Ubuntu Server 26.04.1
             │
             ▼
      Ubuntu kernel + initrd
             │
             ▼
       Autoinstall / NoCloud
             │
             ▼
        Ubuntu 26.04.1
```

### Deployment Profiles

| Profile       | Purpose                                    |
| ------------- | ------------------------------------------ |
| **Desktop**   | Ubuntu Desktop + workstation configuration |
| **Dual-Boot** | Ubuntu Desktop while preserving Windows    |
| **Server**    | Minimal Ubuntu Server installation         |

---

## 📂 Repository Structure

```text
/var/www/html/
├── boot.ipxe
├── ldsblacklogo.png
│
├── ubuntu/
│   ├── desktop/26.04.1/
│   └── server/26.04.1/
│
└── autoinstall/
    ├── desktop/
    ├── server/
    ├── forticlient/
    ├── rustdesk/
    ├── fstab
    ├── fstab-departments/
    ├── sssd.conf
    ├── idmapd.conf
    └── certificates
```

PXE / boot configuration:

```text
/etc/dnsmasq.d/pxe.conf
/srv/tftp/
/var/www/html/boot.ipxe
```

---

## ⚙️ Services

| Service            | Role                         |
| ------------------ | ---------------------------- |
| **dnsmasq**        | DHCP / PXE / TFTP            |
| **iPXE**           | Boot menu and HTTP boot      |
| **nginx**          | Ubuntu media and Autoinstall |
| **Technitium DNS** | Local DNS                    |
| **apt-cacher-ng**  | APT cache                    |
| **Squid**          | Installer proxy              |

---

## 🔄 Deployment Flow

```text
PXE
 ↓
iPXE
 ↓
OS selection
 ↓
Hostname
 ↓
Ubuntu kernel + initrd
 ↓
Ubuntu ISO
 ↓
Autoinstall
 ↓
Reboot
```

---

<p align="center">
  Ubuntu 26.04.1 · PXE · iPXE · Autoinstall
</p>
