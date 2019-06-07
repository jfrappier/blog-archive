---
layout: post
author: Jonathan Frappier
title: Ansible Role for Sensu
permalink: /:title/
modified: 2015-11-20
category: ansible
tags: [monitoring, sensu, role, install]
share: true
---
I am looking into various monitoring products, and since I may need to install them again, that means automation. With some help from Sarah Zelechoski and Larry Smith, I have the first pass done on an Ansible role for Sensu. There may be better ones out there, or you <a href="https://sensuapp.org/docs/latest/installation-overview" target="_blank">might just want to follow the directions manually</a> but so far this role gets the install working up through the base install with examples. This is just as much about me getting better with Ansible, don't like how I did something? PRs welcome as that is how I will learn from those with more experience.

The role is still a work in progress, so very much as some clean up to do, but should get you going quickly on Ubuntu 14.04. Here is a screenshot of the sensu server log:

<img src="/images/fulls/sensu-running.png" class="fit image">

You can grab it from https://github.com/jfrappier/ansible-sensu-role

<a href="https://github.com/jfrappier/ansible-sensu-role"><img class="fit image" src="/images/fulls/github-sensu-role.png" /></a>