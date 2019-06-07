---
layout: post
author: Jonathan Frappier
title: Configure CentOS 7 Kerberos Authentication for Ansible
permalink: /:title/
modified: 2016-07-21
category: ansible
tags: [ansible, windows]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career.", 
    "Installing Ansible 2.1 devel from source on Ubuntu 15.10 with pycrypto errors", 
    "Managing ESXi host DNS using Ansible 2.0"
]
---
I have long maintained that Ansible's documentation is some of the best, if not best out there. However it is impossible to cover every single corner case in documentation which brings me to setting up Ansible to manage Windows, and authenticate via WinRM using Kerberos.

I wanted to work more with the Ansible Windows modules, so set out to build a new clean Ansible control machine. I set this up on CentOS 7 following the <a href="http://docs.ansible.com/ansible/intro_windows.html"> official documentation</a>.

For CentOS 7, which I was using for my control machine, I needed an additinal python dependency in order to support Kerberos. When following the documentation, and told to run:

<pre>yum -y install python-devel krb5-devel krb5-libs krb5-workstation</pre>

You need to also add python-requests-kerberos, so your yum command would be:

<pre>yum -y install python-devel krb5-devel krb5-libs krb5-workstation python-requests-kerberos</pre>

This should allow you to authenticate to Windows machines with domain accounts, as opposed to local user accounts by following the remaining Ansible docs.

If you are unfamiliar with how to join Linux to Active Directory, check out this <a href="http://www.hexblot.com/blog/centos-7-active-directory-and-samba"> blog post</a>, specifically the Joining Active Directory portion since you aren't looking to create shares in Linux.

Here you can see the win_ping module successfully running
<img src="/images/fulls/ansible-kerberos-auth-command.png" class="fit image">

One quick additional note, the documentation also states you need to add

<pre>ansible_winrm_server_cert_validation: ignore</pre>

In your group_vars file if you have Python 2.7.9+ - but I have confirmed on my test machine that this is also required for 2.7.5, so be sure to include that. I have a pull request in to make that change.



