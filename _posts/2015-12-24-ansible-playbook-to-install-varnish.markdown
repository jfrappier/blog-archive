---
layout: post
author: Jonathan Frappier
title: Ansible Playbook to install Varnish
permalink: /:title/
modified: 2015-12-24
category: ansible
tags: [ansible, varnish, playbook, example]
share: true
---
If you need to install Varnish, here is a quick little Ansible playbook to get it installed. Updated your host file and user as needed. I am still reading up on Varnish, so haven't got much past the install yet.

<a href="https://github.com/jfrappier/ansible-varnish" target="_blank">https://github.com/jfrappier/ansible-varnish</a>
<pre>---
- hosts: varnish (update to match your hosts file)
  remote_user: virtxpert (update to match your user account
  sudo: yes

  tasks:
  - name: Install apt-transport &amp; curl
    apt: name=apt-transport-https state=present
    apt: name=curl state=present

  - name: Get Varnish key
    apt_key: url=https://repo.varnish-cache.org/ubuntu/GPG-key.txt state=present

  - name: Add Varnish sources to sources.list.d
    copy:
      src=varnish-cache.list
      dest=/etc/apt/sources.list.d/
      backup=yes
    register: aptrepo

  - name: Apt Update
    apt: update_cache=yes

  - name: Install Varnish
    apt: name=varnish state=present
</pre>