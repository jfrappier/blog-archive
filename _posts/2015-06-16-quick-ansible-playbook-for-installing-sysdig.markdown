---
layout: post
author: Jonathan Frappier
title: Quick Ansible playbook for installing Sysdig
permalink: /:title/
modified: 2015-06-16
category: ansible
tags: [sysdig, ansible, playbook, install]
share: true
---
Been thinking about Sysdig, and how it can be used for troubleshooting. One thought I had was to capture events during an Ansible playbook run in the event there were any problems. Now I'm not sure how practical that is just yet, but the first task was getting Sysdig installed. Of course, that meant writing an Ansible playbook to do so (really should have been a role probably but baby steps).

You can find the sysdig.yml file for Ubuntu/Debian in my <a href="https://github.com/jfrappier/ansible-test-playbooks" target="_blank">test playbooks repository on GitHub</a>: <a href="https://github.com/jfrappier/ansible-test-playbooks" target="_blank">https://github.com/jfrappier/ansible-test-playbooks</a>

Playbook is based on the directions from <a href="http://www.sysdig.org/wiki/how-to-install-sysdig-for-linux/" target="_blank">sysdig.org</a>, and tested on Ubuntu 14.04. As always, I am still learning here but feel free to update as you see fit and take it all with a grain of salt.
<pre>
---
- hosts: parent
  vars:
      package: sysdig
      sysdig_key_url: https://s3.amazonaws.com/download.draios.com
      sysdig_key: DRAIOS-GPG-KEY.public
      dl_dir: /downloads
      sysdig_repo: http://download.draios.com/stable/deb stable-$(ARCH)/
      linux_headers: linux-headers-{{ ansible_kernel }}
  remote_user: [ENTERUSER]
  sudo: yes

  tasks:
  - name: Validating download directory
    file: path={{ dl_dir }} state=directory

  - name: Download Sysdig public key
    get_url: url={{ sysdig_key_url }}/{{ sysdig_key }} dest={{ dl_dir }} validate_certs=no

  - name: Installing Sysdig public key
    apt_key: file={{ dl_dir }}/{{ sysdig_key }} state=present

  - name: Adding Sysdig apt repository
    apt_repository: repo='deb {{ sysdig_repo }}' state=present

  - name: Update apt repositories
    apt: update_cache=yes

  - name: Install Linux Headers
    apt: name={{ linux_headers }} state=present

  - name: Install Sysdig
    apt: name={{ package }} state=present
</pre>
Modules used in this playbook:
<ul>
	<li>http://docs.ansible.com/apt_repository_module.html</li>
	<li>http://docs.ansible.com/apt_key_module.html</li>
	<li>http://docs.ansible.com/apt_module.html</li>
	<li>http://docs.ansible.com/file_module.html</li>
	<li>http://docs.ansible.com/get_url_module.html</li>
	<li></li>
</ul>