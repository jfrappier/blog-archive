---
layout: post
author: Jonathan Frappier
title: Using ansible-galaxy init to create roles
permalink: /:title/
modified: 2015-06-11
category: ansible
tags: [vcsa, vmware, vsphere, vcenter server appliance, vsphere 6]
share: true
---
Ansible held a free online 2 hour introduction session, and while I'm not an expert I do feel I have a good handle on some of the items such as inventory files, and playbook formats. However there is always something to learn! One thing I took away from todays training was an ansible-galaxy command.

This command can save a lot of manual effort up front when creating roles. It will create the basic folder structure and files necessary for an Ansible role - something up until know I've been doing by hand. To use it is simple, just type the command followed by the role you are creating. For example if you were creating a role for PostgreSQL you would simply type:
<pre>ansible-galaxy init postgres</pre>
And this would create the folders such as handlers or tasks, and a main.yml file where appropriate. It was new to me, so thought I'd share!