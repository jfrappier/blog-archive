---
layout: post
author: Jonathan Frappier
title: Producing automation and configuration management code with UpGuard
permalink: /:title/
modified: 2015-11-03
category: articles
tags: [upguard, devops, ansible, security, configuration management, cm, scriptrock]
share: true
---
While Upguard (formerly known as ScriptRock) is great at monitoring and reporting on configuration state, it is not a configuration management tool in the way that Ansible or CFEngine is. They do, however give you the capability of generating what they called "automation snippets" for many popular configuration management tools to either correct an item that has failed the policy check, or even create entire "automation snippets" to full configure a node.

For example, consider a web server that requires a specific version of Java, if the node fails the policy check because a newer version of Java was installed you could simply right click on the failed item and go to Create Automation Snippet and select the type of snippet - a playbook  for Ansbile, a Manifest for Puppet, etc...

As always, starting with a few nodes added to Upguard and you will need to drill down into one of those nodes. Since my previous posts were demo'd with Windows, I'll use Linux here just to mix it up a bit.

<img src="/images/fulls/scriptrock-inventory.png" class="fit image">

There are several areas you can right click to generate these snippets.

First, you can generate the automation snippet for all items right clicking the grey circle at the top

<img src="/images/fulls/all-snippet.png" class="fit image">

For example if I create an automation snippet here for Ansible, (Create Playbook Snippet) it will give me the code for every package, user, folder, etc... on the node

<img src="/images/fulls/ansible-automation-snippet.png" class="fit image">

This is obviously noisy, but also very complete if you want to bring another node into the same configuration very easily. You could also create an automation snippet for a specific category such as EnvVars, files, or packages. Additionally you can also be as granular as a specific item, a likely scenario for a quick fix to a node that is not in compliance. Here you can see I searched for httpd, expanded Packages and selected the httpd package by itself.

<img src="/images/fulls/snippet-httpd.png" class="fit image">

This will produce, as you might expect, just a snippet to ensure httpd is installed

<img src="/images/fulls/snippet-httpd-only.png" class="fit image">

I think that is all I have for Upguard for now, as you can see this is a very handy tool for several use cases such as troubleshooting, ensuring device configuration, and even vulnerability scanning.