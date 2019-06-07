---
layout: post
author: Jonathan Frappier
title: Installing Ansible via Git
permalink: /:title/
modified: 2014-11-25
category: ansible
tags: [ansible, git, devops]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Enabling Disaster Recovery with Ansible", 
    "DevOps IRL Boston VMUG Usercon with @szelechoski"
]
---
**Please note these directions were based on Ansible 1.8. Please check Ansible documentation for updated information**

I'll admit, Git is a completely foreign language to me but it is something I am going to need to learn.  In an attempt to do that, I am going to take something I sort of know how to do manually-ish - install Ansible, but this time install it via git.  Once this is wrapped up I am going to catch up on Matthew Brender's Git #vBrownBag which he did as part of the DevOps series.  Hopefully this post, if nothing else, can help you get Ansible setup and running.  Now Ansible is fairly well documented, but like most open source projects I find they assume a bit to much in their documentation to get you completely up and running.  For example you might have trouble following getting everything needed to install by following <a href="http://docs.ansible.com/intro_installation.html" target="_blank">their directions.</a>

To start, you'll need a linux box to use, I have a CentOS 6.4 setup already so I will be using that.  To get started, we will need git and Python on our linux box - pretty easy to do (When prompted, press y to install the various packages needed):
<pre>yum install git</pre>
Ansible also requires Python, so now to install Python
<pre>yum install python python-setuptools python-devel</pre>
Almost there, just a few more things Ansible needs get up and running.  Since python-setuptools is installed you can setup pip using a python tool called easy_install, then pip to install the others
<pre>sudo easy_install pip</pre>
Now that pip is installed (there is no yum package I could find for this)
<pre>sudo pip install paramiko PyYAML Jinja2 httplib2</pre>
Now that Python is installed we can move on to installing Ansible.  This is where we will start to use git, from / run
<pre>git clone git://github.com/ansible/ansible.git --recursive</pre>
With Ansible now cloned, its time to setup the environment which Ansible was nice enough to provide a script for; cd to /ansible and run:
<pre>source ./hacking/env-setup</pre>
By default, Ansible will look for an inventory file in /etc/ansible/hosts but the clone process or env-setup does not create that for us so;
<pre>mkdir /etc/ansible &amp;&amp; touch /etc/ansible/hosts &amp;&amp; echo "127.0.0.1" &gt; /etc/ansible/hosts</pre>
Now we have a host file which is where we will store all the systems we want to manage with Ansible and added our localhost IP to the file.  Next up you should be able to run a command, the documentation suggests
<pre>ansible all -m ping --ask-pass</pre>
But as you can see we are still missing something, a package called sshpass

<img src="/images/fulls/ansible-almost.png" class="fit image">

We need just a couple more packages to complete this test example assuming you are on a clean install of CentOS
<pre>yum install wget</pre>
<pre>wget http://dl.fedoraproject.org/pub/epel/6/x86_64/sshpass-1.05-1.el6.x86_64.rpm</pre>
<pre>rpm -Uvh sshpass-1.05-1.el6.x86_64.rpm</pre>
Now...
<pre>ssh -l root 127.0.0.1</pre>
Accept the certificate, enter the password for your root user and log out

NOW.....we have success

<img src="/images/fulls/ansible-working.png" class="fit image">

Now that we have Ansible setup and working, it's time to go back and review the <a href="http://professionalvmware.com/2014/11/vbrownbag-devops-follow-up-learning-ansible-with-jeff-geerling-geerlingguy/" target="_blank">#vBrownBag Jeff Geerling</a> (@geerlingguy) did on it to take it to the next level. My next goal is to create a simple inventory file to perform tasks on multiple systems.

Of course, that is the manual way, check out the <a href="http://docs.ansible.com/intro_installation.html#latest-release-via-yum" target="_blank">Ansible documentation for installing via Yum or Apt-Get for example</a>.