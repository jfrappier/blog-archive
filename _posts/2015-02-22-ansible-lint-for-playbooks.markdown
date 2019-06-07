---
layout: post
author: Jonathan Frappier
title: Ansible-lint for playbooks
permalink: /:title/
modified: 2015-02-22
category: ansible
tags: [ansible, devops, lint]
share: true
---
During the #vBrownBag DevOps series after-show from my Using Ansible to provision VM's in vCenter, Mike Marseglia asked about options for <a href="http://stackoverflow.com/questions/8503559/what-is-linting" target="_blank">linting</a> Ansible playbooks. Since I didn't know, I thought it would be worthwhile to look into it. There is an <a href="https://github.com/willthames/ansible-lint" target="_blank">Ansible-Lint repo on GitHub</a>, reading through the information, it seemed straight forward. Here I am going to have a look at installing and using it against some example playbooks.

Installation should be easy, assuming you've got the correct packages installed, see my previous <a title="Installing Ansible via Git" href="http://www.virtxpert.com/installing-ansible-via-git/" target="_blank">Ansible posts</a> - if you got through that install, you should be able to install this with a single line:
<pre>pip install ansible-lint</pre>
Once installed you should now be able to do something like this:
<pre>ansible-lint clone-vm.yml</pre>
The clone-vm.yml is from my #vBrownBag series. As you can see in this screenshot, it suggests I have some trailing whitespace

<img src="/images/fulls/ansible-lint-whitespace.png" class="fit image">

Once I tiddy up the extra whitespace in the playbook, no suggestions are returned.

<img src="/images/fulls/ansible-lint-fixed.png" class="fit image">

That is a pretty basic example, let's say I've missed something such as a { when using vars_prompt, here you can see I have a missing backet for vm

<img src="/images/fulls/ansible-lint-example2.png" class="fit image">

Once again, now that it is fixed, no suggestions are returned. One thing that at least this specific tool does not help with is spacing errors, so your playbook will need to be valid, running ansible-lint here for example where my spacing is incorrect results in an general Ansible error, though it does point out where the error likely is:

<img src="/images/fulls/ansible-lint-error.png" class="fit image">

Going forward I'll certainly be looking into using this when writing a playbook to ensure general recommended practices are adhereed to. I'm still on the lookout for a tool that can help with spacing though!