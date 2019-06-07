---
layout: post
author: Jonathan Frappier
title: Installing VMware Photon
permalink: /:title/
modified: 2016-07-06
category: vmware
tags: [vmware, photon, container]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career.", 
    "Installing Ansible 2.1 devel from source on Ubuntu 15.10 with pycrypto errors", 
    "Managing ESXi host DNS using Ansible 2.0"
]
---
Photon is a minimal operating system designed to run containers (for example using Docker). You can learn more about Photon from <a href="https://vmware.github.io/photon/" target="_blank">the offical GitHub project page</a>

In this post, I will review the steps used to install Photon OS. In my case, I will be installing as a VM in Workstation. Once you have downloaded the ISO, create a VM (Workstation 12 has a Photon OS type available) and mount the ISO

Once powered on, you will see the installation splash screen
<img src="/images/fulls/photon-install-1.PNG" class="fit image">

Press enter to start the installation and accept the license agreement. I accepted the default 8GB partition, so I have only a single option available to install the OS.
<img src="/images/fulls/photon-install-partition-2.PNG" class="fit image">

Confirm you want to erase the disk and select the installation type, in my case I am going with minimal. The full installation includes utilities to create and publish containers, while the minimal provides the necessary packages to run and manage containers.
<img src="/images/fulls/photon-install-install-type-3.PNG" class="fit image">

Select your install type, provide a hostname, and root password; the installation will now begin.
<img src="/images/fulls/photon-install-progress-4.PNG" class="fit image">

Once complete, press a key to restart the VM. Once restarted, you can log into your VM and explore the OS. 

For example, to start start docker you will need to start and enable docker

<pre>systemctl start docker && systemctl enable docker</pre>

Now you could start a Docker container from Dockers public registry - <a href="https://hub.docker.com/" target="_blank">hub.docker.com</a>, the old standby seems to be running NGINX

<pre>docker run -d -p 8080:80 nginx</pre>

This will download, specifically do a docker pull on the image, and run it.
<img src="/images/fulls/photon-docker-run-5.PNG" class="fit image">

Once the pull completes, you can run the <pre>docker ps</pre> command to see that it is running
<img src="/images/fulls/photon-docker-ps-6.PNG" class="fit image">

As well as view the running instance on the port specified in the docker run command, in this case 8080.
<img src="/images/fulls/photon-docker-nginx-7.PNG" class="fit image">



