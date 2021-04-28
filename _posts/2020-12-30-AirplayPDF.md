---
title: "Install a Virutal PDF Airplay Printer"
#description: "Install a Virtual PDF printer in Network, so that you directly save anything from iphone as PDF in network storage"
header:
  overlay_image: /assets/images/print.jpg
  overlay_filter: rgba(140,140,140, .6)
  caption: "Photo credit: [**Pexels**](https://pexels.com)"
  actions:
    #- label: "Learn more"
    #  url: "https://unsplash.com"
categories:
  - Linux
toc: false
toc_sticky: false

tags:
  - Linux
  - VirtualPrinter
  - CUPS
#last_modified_at: 2018-04-23T16:00:52-04:00
---

"Install a Virtual PDF printer in Network"

### 1. Hardware and Software

 
 1. ubuntu server [Any debian distro should work]
 2. Raspberry pi
 3. IPhone [To Test]
 
Install CUPS and CUPS-PDF Package

```bash
sudo apt-get install cups
sudo apt-get install printer-driver-cups-pdf
```
Edit cups Daemon config
```bash
sudo nano /etc/cups/cupsd.conf
```
Enable the cups web interface to accesss from anywhere 

Search for:
```bash
Listen localhost:631
```
Modify to :
```bash
Port 631
```
Allow access from any host in the network

Modify the Following sections as below
```bash
<Location />
  # Allow remote access...
  Order allow,deny
  Allow @local
</Location>
<Location /admin>
  Order allow,deny
  Allow	@local
</Location>
<Location /admin/conf>
  AuthType Default
  Require user @SYSTEM
    Order allow,deny
  Allow	@local
</Location>
```
Save the file and Quit

Give Permission  local user to add printer 
```bash
sudo usermod -a -G lpadmin <username>
```
 Restart Cups Server
```bash
sudo service cups restart
```
Configire the Virtual PDF Printer
```bash
sudo nano /etc/cups/cups-pdf.conf
```
Go to Path Settings -> Change the value of `Out` <br>
This is the location which PDF files are saved for the default user
```bash
Out ${HOME}/PDF
```
Change the value of AnonDirName <br>
Location of all the PDF files printer by other users
```bash
AnonDirName /home/ubuntu/dev/PDF
```
Goto Filename Settings > Label change to 1

 1: label all documents with a preceeding "job_#-"
```bash
Label 1
```
Goto User settings  > AnonUser <br>
I am not sure this chage is need or not
```bash
AnonUser ubuntu
```
Save the file and exit <br>
Now add the Virtual pdf printer through CUPS interface

Access the Cups Web Interface <br>
```bash
1.http://<Ip address of the cups server>:631
2.Goto Administrataion > Add Printer
```
<figure>
   <img src="{{ '/assets/images/CupsWeb1.png' | relative_url }}" alt="CupsConfig1">
   <img src="{{ '/assets/images/CupsWeb2.png' | relative_url }}" alt="CupsConfig2">
   <img src="{{ '/assets/images/CupsWeb3.png' | relative_url }}" alt="CupsConfig3">
   <img src="{{ '/assets/images/CupsWeb4.png' | relative_url }}" alt="CupsConfig4">
</figure>

Click on Add Printer
<figure>
  <img src="{{ '/assets/images/CupsWeb5.png' | relative_url }}" alt="CupsCOnfig5">
</figure>

Once printer is added, Modify settings like above. 
Click Change Settigs. It will save and restart the server <br>
Now open the iphone print screen you should be able to see virtual pdf printer
<figure>
  <img src="{{ '/assets/images/CupsWeb6.png' | relative_url }}" alt="CupsCOnfig6">
</figure>

Give a print and you should see the PDFs in:
```bash
cd ~/PDF
```

Extra Commands:

To Restart the cups
```bash
 sudo service cups restart
```
To see the Cups Service Status
```bash
sudo service cups status
```
Default logs Location
```bash
cd /var/log/cupbbs
```

