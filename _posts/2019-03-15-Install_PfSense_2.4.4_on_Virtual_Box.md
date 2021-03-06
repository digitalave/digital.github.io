---
layout: post
authors: [dimuthu_daundasekara]
title: 'Install PfSense 2.4.4 on Virtual Box'
image: /assets/img/post-imgs/pf-install/pfSense-Installation_N.jpg
tags: [pfSense, Firewall, Captive Portal,Virtual Box]
category: devops
comments: true
last_modified_at: 2020-01-31
---

<style>
.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }
</style>
<div class='embed-container'>
    <iframe src='https://www.youtube.com/embed/SEXfqFCh3y0?&autoplay=1' frameborder='0' allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
    </iframe>
</div>

# Install PfSense 2.4.4 on Virtual Box

pfSense is network firewall based on FreeBSD operating system with a custom kernel and includes free third-party packages for additional features. It provides same functionality or more of common commercial firewalls.
In this tutorial, I will install pfSense in VirtualBox it will work as a firewall for our virtual lab environment for future articles as well.

## STEP 1:- Download Pfsense ISO Image.

Download most recent stable release from https://www.pfsense.org/download/

## STEP 2: - Install Pfsense on Virtual box
A. Create a Virtual Machine for Pfsense
<img src="/assets/img/post-imgs/pf-install/PF1.gif" width="auto" alt="Digital Avenue DevOps Tutorials">


Follow The Guideline as seen on GIF image.

<img src="/assets/img/post-imgs/pf-install/PF2.gif" width="auto" alt="Digital Avenue DevOps Tutorials">
Here we have assigned two interfaces with one Bridge Interface (em0) and one Internal Interface Named (em1) LAN2.
Assigned IP as follows…

Em0 - WAN Interface - 192.168.1.100/24
Em1 – LAN Interface – 192.168.100.1/24

DHCP Range – 192.168.100.100 – 192.168.100.200

<img src="/assets/img/post-imgs/pf-install/PF3.gif" width="auto" alt="Digital Avenue DevOps Tutorials">
## STEP 2: - Logging in to Web Interface
Pfsense default username and password is “admin” and “Pfsense”

Complete Pfsense initial setup wizard.

<img src="/assets/img/post-imgs/pf-install/PF4.gif" width="auto" alt="Digital Avenue DevOps Tutorials">


**Little Request:**

> I appreciate you guys taking the time in reading my post. Please check out my YouTube channel and please subscribe for more as it'll help me loads.


<a href="https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ/featured?view_as=subscriber" target="_blank">https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ/featured?view_as=subscriber</a>