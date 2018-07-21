---
layout: post
section_id: academy
title:  "Install your first Nimiq Node"
date:   2016-07-18 10:00:00
excerpt: Learn how to set up your first Nimiq Node
type: lesson
course: nimiqprogrammerbasic
author: Richy
images: 
  - "/images/academy/nodejscode.jpg"
---

## Requirements

This guide uses Linux Debian as the base operating system. 



## Download and install the package

The official nimiq node can be downloaded in the [Nimiq Website](https://nimiq.com/#downloads).

<code>wget https://repo.nimiq.com/deb/pool/main/n/nimiq/nimiq_1.2.0-1_amd64.deb</code>

Install the package 

<code>dpkg -i nimiq_1.2.0-1_amd64.deb</code>

Configure the node according to your needs.

Start the miner

<code>systemctl start nimiq</code>

Check the logs for output

<code>tail -f /var/log/syslog</code>

If your configuration is correct you should see the node connecting to the network and reaching consensus.