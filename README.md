# Honeypot using T-Pot

This repository outlines my steps to deploy and manage a **T-Pot Honeypot** on **Google Cloud Platform (GCP)**. T-Pot is a multi-honeypot platform designed to monitor, capture, and visualize various types of cyberattacks. 

## Table of Contents
1. [Introduction](#introduction)
2. [Project Setup](#project-setup)
3. [T-Pot](#t-pot)
4. [Conclusion](#conclusion)

## Introduction
A **honeypot** is essentially a trap designed to attract adversaries. It works as a real vulnerable computer system, which any adversary can attack, and their attempts benefit us by allowing us to learn more about their attack mechanisms. Due to the sensitive nature of honeypots, it's highly recommended to follow certain best practices when it comes to deploying and managing a honeypot safely. I chose T-Pot specifically due to its ease of deployment, as it combines multiple honeypots into one whole solution, and it provides a detailed picture of potential malicious activity using data visualization. It achieves this by integrating the ELK Stack (Elasticsearch, Logstash, Kibana) into its framework, which allows security analysts to organize and analyze all of the aggregated data. 

## Project Setup
### VM Instance Configuration
- After creating an account on GCP, I created a new VM instance under the `Computer Engine` section, which includes virtual machines, GPUs, etc. I kept most of the default settings except I changed the `machine type` to a `custom e2`, which included 2 vCPU and around 10 GB of memory. An `e2-standard` configuration would also work just fine. I also enabled `HTTP/HTTPS traffic` under the Firewalls section, as it was necessary for T-Pot to function. The `boot disk` included an `Ubuntu 22.04` image with 40 GB of storage. For a visual reference: <img src="https://github.com/mushy2005/Honeypot/blob/main/images/enable-firewall.png"><br><img src="https://github.com/mushy2005/Honeypot/blob/main/images/bootdisk.png">

### Firewall Rules
- Proper firewall rules ensure the safe deployment of T-Pot. I created a new firewall rule called `tpot-firewall` with the following settings: <img src="https://github.com/mushy2005/Honeypot/blob/main/images/firewall-rule.png" width=40% height=40%>
- Here's what all of the firewall rules look like: <img src="https://github.com/mushy2005/Honeypot/blob/main/images/all-rules.png">

### Installation
- After configuring everything, I followed through ![T-Pot's GitHub](https://github.com/telekom-security/tpotce) for instructions to install. The whole process was quite simple; though I did land into some misconfiguration problems, I managed to solve them with ease.

## T-Pot
- Once the installation was completed, I was able to access T-Pot's webpage using the external IP address assigned and `port 64297`. The link would look something like `https://ip-address:64297`. I was also able to SSH using `port 64295`. This was quite useful because there were times when I wanted to check the status of T-Pot. I checked that by running `sudo systemctl status tpot`. Do note that when running T-Pot for the first time, it does take some time to set up services and gather attacks. I left the instance running for about 24 hours, though it's recommended to leave it for at least a few days, as that'll help with spotting any trends in the attacks.
- After letting it run for 24 hours, I visited the same link and landed on the homepage: <img src="https://github.com/mushy2005/Honeypot/blob/main/images/tpot.png" width=90% height=90%>

### Attack Map
- One of the T-Pot's most useful features is the attack map, which as the name implies, consists of a whole map that displays where each attack is coming from, as well as additional information. This is what it looks like: <img src="https://github.com/mushy2005/Honeypot/blob/main/images/attackmap.png" width=90% height=90%>
- Firstly, on the very top, it shows us the number of attacks that occurred during the last 24 hours. This specific instance of T-Pot was attacked approximately `133,311` times in the last 24 hours.
- Your eyes probably went to the actual map first, as it covers a huge space of the page. The purpose of the map is to visually show, or give us a rough idea, of which parts of the world our instance is getting attacked from. When you click on the small dots that appear on the map, it will display either one of the following:<br><img src="https://github.com/mushy2005/Honeypot/blob/main/images/known.png"><br><img src="https://github.com/mushy2005/Honeypot/blob/main/images/rep-unknown.png"><br>
This gives us two crucial details about the attacker: the IP address and the "reputation". For example, an attacker who resides in the US with an IP address of `43.153.8.12` is a known attacker, according to T-Pot. The other one, who resides in Martinique and has an IP address of `93.176.6.160,` is an unknown attacker. T-Pot aggregates this type of information from various threat databases. This is another great feature of T-Pot as it could likely save time when researching the history of a certain IP address.
- At the bottom section, we get more information about the attacks. For example, we could see which country attacked the instance the most, also known as `hits`. We also see the type of services the attackers targeted, such as FTP or HTTP. Overall, an attack map is a great feature that includes some form of visual data regarding attacks, as well as more surface-level information regarding the attacks. 

### Kibana
- Another great feature is Kibana, which plays an essential part in the ELK Stack. Kibana helps to visualize all of the data collected among many of the different honeypot services included with T-Pot, such as Cowrie. Here's what Kibana looks like when you access it through T-Pot's homepage:<img src="https://github.com/mushy2005/Honeypot/blob/main/images/kibana.png" width=90% height=90%>
As we can see, Kibana comes bundled with various dashboards to output the data sent by the honeypots in a visually appealing manner. For now, we'll explore the `T-Pot Dashboard`, as that's the main service.
- Upon clicking the `T-Pot` option, we'll be presented by:<img src="https://github.com/mushy2005/Honeypot/blob/main/images/tpot-kib.png">
- Here, we're presented with a bunch of analytics. First and foremost, the numbers on the very top indicate the number of times each honeypot service has been targeted. For example, Cowrie, which is an SSH and Telnet honeypot, was attacked `38,491` times. All of these attacks on different services sum up to the total number of attacks, which is 133,461.
- Looking at the graph below, we can observe the honeypot service that got attacked the most. In our case, `Ddospot`, which is a honeypot that tracks UDP-based Distributed Denial of Service (DDoS) attacks, got the most hits. We can also see that Cowrie nearly got as many hits as Ddospot, which helps us to determine the service that most of the attackers target.<img src="https://github.com/mushy2005/Honeypot/blob/main/images/bar.png">
- We're also able to see where the majority of the attacks came from, as well as the services that got attacked during a certain interval of time. These two pieces of information could assist in an investigation, as they reveal background information regarding the attacks.<img src="https://github.com/mushy2005/Honeypot/blob/main/images/map.png" width=70% height=70%><br><img src="https://github.com/mushy2005/Honeypot/blob/main/images/histogram.png" width=70% height=70%>
- When we scroll down, we're presented with much more information, though I'll only focus on two graphs. Along with providing us with geographic information regarding the attacks, Kibana also provides us with the common usernames and passwords used by attacks to gain access to our system. The following shows just that. <img src="https://github.com/mushy2005/Honeypot/blob/main/images/usernames.png" width=70% height=70%><br><img src="https://github.com/mushy2005/Honeypot/blob/main/images/pwds.png" width=70% height=70%>
- As you can guess, the bigger ones are the most used ones. All of this information that Kibana provides us with can be used to pinpoint general trends, which is crucial when making decisions regarding security.

### Spiderfoot
- Spiderfoot is an Open Source Intelligence (OSINT) integrated into T-Pot to help analyze data regarding malicious activity. One key feature of Spiderfoot is that it can determine the reputation of an IP address or domain by checking countless sources, such as security databases. Here's what its homepage looks like:<img src="https://github.com/mushy2005/Honeypot/blob/main/images/spider.png">
- To scan an IP address or domain, we simply click the `Scan` button, which will take us to another page: <img src="https://github.com/mushy2005/Honeypot/blob/main/images/spiderscan.png" width=70% height=70%>

## Conclusion
Overall, honeypots serve as a great tool to analyze trends in malicious activity. Since T-Pot has the ELK Stack integrated into it, all of the data aggregated through various honeypots could easily be displayed using dashboards, which is what Kibana excels at. 
