---
title: "Open VPN Full Tunnel config"
#description: "Set Open VPN config for full tunnel"
header:
  overlay_image: /assets/images/print.jpg
  overlay_filter: rgba(140,140,140, .6)
  caption: "Photo credit: [**Pexels**](https://pexels.com)"
  actions:
    #- label: "Learn more"
    #  url: "https://unsplash.com"
categories:
  - Networking
toc: false
toc_sticky: false

tags:
  - VPN
  - OpenVPN
  - Full Tunnel
  - Mobile VPN
#last_modified_at: 2018-04-23T16:00:52-04:00
---

### Configuration for Open VPN Full Tunnel

 
1. Configure your VPN Server, and download the VPN config File

2. Open VPN config file in text Editor

3. Add the followling line to config file, add after initial set of configuration, Before the certificate.
```bash
redirect-gateway def1
dhcp-option DNS 8.8.8.8
```
 *  if you have any private dns server provide that address
4. Save the configuration file
5. Send to VPN client
6. Load the VPN config on OpenVPN APP
