# Honeypot using T-Pot

This repository outlines my steps to deploy and manage a **T-Pot Honeypot** on **Google Cloud Platform (GCP)**. T-Pot is a multi-honeypot platform designed to monitor, capture, and visualize various types of cyberattacks. 


## Table of Contents
1. [Introduction](#introduction)
2. [Project Setup](#project-setup)

## Introduction
A **honeypot** is essentially a trap designed to attract adversaries. It works as a real vulnerable computer system, which any adversary can attack, and their attempts benefit us by allowing us to learn more about their attack mechanisms. Due to the sensitive nature of honeypots, it's highly recommended to follow certain best practices when it comes to deploying and managing a honeypot safely. 

## Project Setup
### VM Instance Configuration
- After creating an account on GCP, I created a new VM instance under the `Computer Engine` section, which includes virtual machines, GPUs, etc. I kept most of the default settings except I changed the `machine type` to a `custom e2`, which included 2 vCPU and around 10 GB of memory. An `e2-standard` configuration would also work just fine. I also enabled `HTTP/HTTPS traffic` under the Firewalls section, as it was necessary for T-Pot to function. The `boot disk` included an `Ubuntu 22.04` image with 40 GB of storage. For a visual reference: <img src="https://github.com/mushy2005/Honeypot/blob/main/images/enable-firewall.png"><br><img src="https://github.com/mushy2005/Honeypot/blob/main/images/bootdisk.png">

### Firewall Rules
- Proper firewall rules ensure the safe deployment of T-Pot. I created a new firewall rule called `tpot-firewall` with the following settings: <img src="https://github.com/mushy2005/Honeypot/blob/main/images/firewall-rule.png" width=40% height=40%>
- Here's what all of the firewall rules look like: <img src="https://github.com/mushy2005/Honeypot/blob/main/images/all-rules.png">

### Installation
- After configuring everything, I followed through ![T-Pot's GitHub](https://github.com/telekom-security/tpotce) for instructions to install. The whole process was quite simple; though I did land into some problems, I managed to solve them with ease.

## T-Pot
- Once the installation was completed, I was able to access T-Pot's webpage using the external IP address assigned and `port 64297`. The link would look something like `https://ip-address:64297`. I was also able to SSH using `port 64295`. This was quite useful because there were times when I wanted to check the status of T-Pot. I checked that by running `sudo systemctl status tpot`. Do note that when running T-Pot for the first time, it does take some time to setup services and gather attacks. I left the instance running for about 24 hours, though it's recommended to leave it for at least a few days, as that'll help with spotting any trends in the attacks.
- After letting it run for 24 hours, I visited the same link and landed on the homepage: <img src="https://github.com/mushy2005/Honeypot/blob/main/images/tpot.png" width=90% height=90%>

### Attack Map
- One of the T-Pot's most useful features is the attack map, which as the name implies, consists of a whole map that display where each attack is coming from, as well as additional information. This is what it looks like: <img src="https://github.com/mushy2005/Honeypot/blob/main/images/attackmap.png" width=90% height=90%>
- Firstly, on the very top, it shows us the number of attacks that occurred during the last 24 hours. This specific instance of T-Pot was attacked approximately `133,311` times in the last 24 hours.
- Your eyes probably went to the actual map first, as it covers a huge space of the page. The purpose of the map is to visually show, or give us a rough idea, of which parts of the world our instance is getting attacked from. When you click on the small dots that appear on the map, it will display either one of the following:<br><img src="https://github.com/mushy2005/Honeypot/blob/main/images/known.png"><br><img src="https://github.com/mushy2005/Honeypot/blob/main/images/rep-unknown.png"><br>
This gives us two crucial details about the attacker: the IP address and the "reputation". For example, an attacker that resides in the US with the IP address of `43.153.8.12` is a known attacker, according to T-Pot. The other one that resides in Martinique with an IP address of `93.176.6.160` is an unknown attacker. T-Pot aggregates this type of information from various threat databases. This is another great feature of T-Pot as it could likely save time from researching the history of a certain IP address.
- At the bottom section, we get more information about the attacks. For example, we could see which country attacked the instance the most, also known as `hits`. We also see the type of services the attackers targeted, such as FTP or HTTP. Overall, attack map is a great feature that includes some form of visual data regarding attacks, as well as more surface-level information regarding the attacks. 


