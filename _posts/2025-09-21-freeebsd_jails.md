---
title: "FreeBSD thinJail troubleshooting"
header:
  overlay_image: /assets/images/dp.jpg
  overlay_filter: rgba(140,140,140, .6)
  caption: "Photo credit: [**PEXELS**](https://PEXELS.com)"
  actions:
    #- label: "Learn more"
    #  url: "https://unsplash.com"
categories:
  - Linux
toc: true
toc_sticky: true

tags:
  - Linux
  - FreeBSD
  - Rock64
#last_modified_at: 2018-04-23T16:00:52-04:00
---

Install FreeBSD Jails

#Follow the instructions in freeBSD Docs

### 1. Issue: unable to start jail 
```bash
# service jail restart -vvvv testjail
Stopping jails:.
Starting jails: cannot start jail  "-vvvv": 
testjail: run command: /sbin/ifconfig dwc0 inet 192.168.1.235 netmask 255.255.255.255 alias
testjail: run command: /sbin/mount -t nullfs -o ro /usr/local/jails/templates/14.2-RELEASE-base /usr/local/jails/testjail-nullfs-base/
mount_nullfs: /usr/local/jails/testjail-nullfs-base: Resource deadlock avoided
jail: testjail: /sbin/mount -t nullfs -o ro /usr/local/jails/templates/14.2-RELEASE-base /usr/local/jails/testjail-nullfs-base/: failed
testjail: run command: /sbin/ifconfig dwc0 inet 192.168.1.235 netmask 255.255.255.255 -alias
 cannot start jail  "testjail": 
mount_nullfs: /usr/local/jails/testjail-nullfs-base: Resource deadlock avoided
jail: testjail: /sbin/mount -t nullfs -o ro /usr/local/jails/templates/14.2-RELEASE-base /usr/local/jails/testjail-nullfs-base/: failed

```
FIX:
```bash
umount -f /usr/local/jails/testjail-nullfs-base
service jail start testjail
```

### 2. Issue: pkg command not working 
```bash
# pkg search mg
pkg: An error occurred while fetching package: No error
pkg: An error occurred while fetching package: No error
pkg: An error occurred while fetching package: No error
pkg: An error occurred while fetching package: No error
pkg: An error occurred while fetching package: No error
pkg: An error occurred while fetching package: No error
```

FIX: working on it

