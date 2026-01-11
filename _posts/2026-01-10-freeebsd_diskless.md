---
title: "FreeBSD Diskless on Raspberry Pi (8GB) — Troubleshooting Guide"
header:
  overlay_image: /assets/images/dp.jpg
  overlay_filter: rgba(140,140,140, .6)
  caption: "Photo credit: [**PEXELS**](https://pexels.com)"
categories:
  - FreeBSD
  - Linux
  - Networking
tags:
  - FreeBSD
  - Diskless
  - NFS
  - Raspberry Pi
toc: true
toc_sticky: true
---

## Overview

This post documents a **working diskless FreeBSD setup on a Raspberry Pi (8GB)** using a **UniFi NAS (Debian-based)** as the NFS server.  
It also highlights common pitfalls, UniFi NAS limitations, and required workarounds.

If you're trying to boot FreeBSD with:

- `/` mounted **read-only** over NFS
- `/var` mounted **read-write** over NFS

this guide should save you several hours of troubleshooting.

---

## References

Before starting, review the official documentation:

- 📘 **FreeBSD Handbook – Diskless Operation**  
  https://docs.freebsd.org/en/books/handbook/advanced-networking/#network-diskless

---

## NFS Server Configuration (UniFi NAS)

### Design

| Mount Point | NFS Path | Mode |
|------------|---------|------|
| `/`        | `fbsdbase/fb143` | Read-only |
| `/var`     | `rwvar/rpi8gig`  | Read-write |

> ⚠️ **Note**  
> UniFi NAS does **not** expose `no_root_squash` in the UI.  
> You must edit the exports file manually.

---

### `/etc/exports.d` Configuration

```bash
root@UNAS-2:/etc/exports.d# cat shared-*.exports

/var/nfs/shared/server/fbsdbase/fb143 192.168.1.0/24(
  rw,sync,no_subtree_check,no_root_squash
)

/var/nfs/shared/server/rwvar/rpi8gig 192.168.1.0/24(
  rw,sync,no_subtree_check,no_root_squash
)

root@UNAS-2:/etc/exports.d# exportfs -ra
```

---

## Diskless Client Configuration

### Editing `fstab`

1. Mount the diskless root filesystem on another machine
2. `chroot` into it
3. Edit `/etc/fstab`

```bash
sudo chroot /mnt/server
```

```fstab
# NFS root (read-only)
192.168.1.60:/var/nfs/shared/server/fbsdbase/fb143  /     nfs  ro  0  0

# NFS /var (read-write)
192.168.1.60:/var/nfs/shared/server/rwvar/rpi8gig  /var  nfs  rw  0  0
```

### Memory Disk Size

```bash
cat /conf/base/etc/md_size
32768
```

➡️ This allocates a **32 MB memory disk** for early boot operations.

---

## `/var` Remount Configuration

```bash
cat conf/base/var/diskless_remount
192.168.1.60:/var/nfs/shared/server/rwvar/rpi8gig
```

---

## System Configuration (`rc.conf`)

```bash
hostname="diskless"

ifconfig_DEFAULT="DHCP inet6 accept_rtadv"

sshd_enable="YES"

# Disable Sendmail
sendmail_enable="NONE"
sendmail_submit_enable="NO"
sendmail_outbound_enable="NO"
sendmail_msp_queue_enable="NO"

growfs_enable="YES"
powerd_enable="YES"

# Time sync
ntpdate_enable="YES"
ntpd_enable="YES"
ntpd_sync_on_start="YES"

# NFS / RPC
rpc_statd_enable="YES"
rpc_lockd_enable="YES"
```

---

## syslogd Dependency Fix

Edit `/etc/rc.d/syslogd` and ensure `netif` is included:

```diff
- REQUIRE: mountcritremote FILESYSTEMS newsyslog
+ REQUIRE: mountcritremote FILESYSTEMS newsyslog netif
```

This prevents syslog startup failures during diskless boot.

---

## Rebuilding `/etc` Image

Whenever **anything under `/etc` changes**, rebuild the archive:

```bash
tar -c -v -f conf/base/etc.cpio.gz --format cpio --gzip etc
```

---

## DHCP, TFTP & NetBoot

Ensure your router is configured with:

- DHCP
- TFTP
- Next-server / bootfile options

Example (UniFi UI):

![UniFi DHCP Configuration](/assets/images/unifi_dhcp.png)

---

## Final Notes

✅ Root filesystem over NFS (RO)  
✅ `/var` over NFS (RW)  
✅ Stable reboots  
✅ Works reliably on Raspberry Pi 8GB

---

*Happy hacking & diskless booting!* 🚀
