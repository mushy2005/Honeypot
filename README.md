# Honeypot using T-Pot

This repository outlines my steps to deploy and manage a **T-Pot Honeypot** on **Google Cloud Platform (GCP)**. T-Pot is a multi-honeypot platform designed to monitor, capture, and visualize various types of cyberattacks. 


## Table of Contents
1. [Introduction](#introduction)
2. [Project Setup](#project-setup)

## Introduction
A **honeypot** is essentially a trap designed to attract adversaries. It works as a real vulnerable computer system, which any adversary can attack, and their attempts benefit us by allowing us to learn more about their attack mechanisms. Due to the sensitive nature of honeypots, it's highly recommended to follow certain best practices when it comes to deploying and managing a honeypot safely. 

## Project Setup
### VM Instance Configuration
- After creating an account on GCP, I created a new VM instance under the `Computer Engine` section, which includes virtual machines, GPUs, etc. I kept most of the default settings except I changed the `machine type` to a `custom e2`, which included 2 vCPU and around 10 GB of memory. An `e2-standard` configuration would also work just fine. I also enabled `HTTP/HTTPS traffic` under the Firewalls section, as it was necessary for T-Pot to function. The `boot disk` included an `Ubuntu 22.04` image with 40 GB of storage.

### Firewall Rules
- Proper firewall rules ensure the safe deployment of T-Pot. I created a new firewall rule called `tpot-firewall` with the following settings: 
- Here's what all of the firewall rules look like:

### Installation
- After configuring everything, I followed through ![T-Pot's GitHub](https://github.com/telekom-security/tpotce) for instructions to install. The whole process was quite simple, though I did land into some problems but managed to solve them with ease.

## T-Pot
- Once the installation was completed, I was able to access T-Pot's webpage using the external IP address assigned and `port 64297`. The link would look something like `https://ip-address:64297`. I was also able to SSH using `port 64295`. This was quite useful because there were times where I wanted to check the status of T-Pot. I checked that by running `sudo systemctl status tpot`. Do note that when running T-Pot for the first time, it does take some time to setup services and gather attacks. I left the instance running for about 24 hours, though it's recommended to leave it for at least a few days, as that'll help with spotting any trends in the attacks.
- After letting it run for 24 hours, I visited the same link and landed on the homepage:


