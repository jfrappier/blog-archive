---
layout: post
author: Jonathan Frappier
title: Comparing Windows Nodes with ScriptRock
permalink: /:title/
modified: 2015-09-09
category: articles
tags: [scriptrock, upguard, nodes, compare]
share: true
---
Recently ScriptRock announced their service is completely free for up to 10 nodes, when I say completely there are no features you have to pay for unlike other freemium offerings. ScriptRock allows you to monitor the configuration of your infrastructure including Windows, and Linux servers, AWS instances, and other cloud services. A really interesting new feature, if you have been interested but not sure how to get started with something like Ansible, is the ability to create an Ansible role/playbook from a monitored host. There is quite a bit you can do with ScriptRock, so let's start by taking a look at how to compare two nodes.

After signing up, you will be asked to add some node to monitor, in this case lets take a look at a Windows node, specifically a Domain Controller (imagine being able to instantly see all of the DC's in your organization are configured exactly the same way!). ScriptRock will use either an agent, or SSH to monitor it, in the case of Windows I'll opt for the agent. Click on Windows

<img src="/images/fulls/scriptrock-add-node.png" class="fit image">

And then download the installer to a location accessible by the Windows node you wish to monitor (or directly to it). In the download window, you will also be given an API key, document this and start the install wizard (pretty basic install) entering the API key when prompted. When the installer finishes, click Continue in the ScriptRock website, it will commence the scan of the node you installed the agent on. Once complete, click view scan - you can now see all the information about the node you just scanned.

You can drill down into each of the categories, for example here is all of the Windows features I have installed

<img src="/images/fulls/script-winfeatures.png" class="fit image">

or all of my environment variables, such as the paths that are set - think of an app that has a specific path requirement to a certain version of JAVA_HOME, be nice to know if that application isn't functioning because it doesn't have the correct path defined!

<img src="/images/fulls/script-winpath.png" class="fit image">

While having a dashboard to see the configuration is nice, what is really cool is being able to compare to nodes. I installed the ScriptRock agent on a second Windows node (not a Domain Controller on purpose); click the Add Node button and running through the same process you did initially. With only 2 nodes it is easy to see all of them under "All Nodes" however there is also a concept of node groups, for example all of your Domain Controllers or SQL Servers. When I drill down into a node group I can "diff" the nodes in that group by clicking on Diff This Group. Within just a few seconds ScriptRock scanned through 713 different configurations and found 245 differences! Now I compared two different servers on purpose to have a better example, but imagine if you found 245 different configuration options across your application servers or Domain Controllers!

<img src="/images/fulls/diff.png" class="fit image">

Not bad for 5 minutes worth of effort eh? In addition to monitoring changes between nodes, you can also use this to monitor changes on a specific node and re-scanning the nodes or waiting for the next schedule scan to run. After making some changes and running a scan, click on the node and select a previous scan to "compare to"

<img src="/images/fulls/diffs.png" class="fit image">

Here you can see I installed IsoBuster and a few files changed. This is just a few minutes into using the tool and already it can provide value from both a documentation, security, and auditing perspective. In my next post I will look to use policies and templates - stay tuned!