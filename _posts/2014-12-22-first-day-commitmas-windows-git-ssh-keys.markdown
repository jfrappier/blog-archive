---
layout: post
author: Jonathan Frappier
title: On the first day of Commitmas - Windows Git, SSH, and Keys
permalink: /:title/
modified: 2014-12-22
category: articles
tags: [git, github, commitmas]
share: true
---
<a href="/images/fulls/SamSnowman.jpg"><img class="  wp-image-3370 alignleft" src="/images/fulls/SamSnowman.jpg" alt="SamSnowman" width="330" height="252" /></a>I don't know what I would have done without Commitmas to pull us through. Anyway - uh, Commitmas? Huh, could it be that some of you are not acquainted with the story of Commitmas?

Yesterday was <a href="http://neckbeardinfluence.com/join-me-in-learning-to-share-code-during-commitmas/" target="_blank">the first day of Commitmas</a>, a community event thought up by Matt Brender to help us all get used to <a href="https://www.youtube.com/watch?v=fWRLSkUAUss" target="_blank">sharing code and working with GitHub</a>. The challenges vary based on your level of comfort - I am starting in the beginner track and hoping to work my comfort level up to being a "beginner intermediate" by the end. Now I am no developer but I see the train coming, and for all the vCommunity out there I hope you see it coming at well. The future is code and scripts; Commitmas is a great way to prepare.

Last month I published a few posts on Ansible, as part of that I created a repository on GitHub to put my playbooks in. To get back into the swing of GitHub I decided to install Git on Windows, clone my Ansible repository and create a simple README file.

What little knowledge and hands on with GitHub I have has all been from a Linux based system. On Windows you lack some of the common tools you have with Linux such as the ability to create SSH keys or an SSH client. The Git install for Windows provides these for you. Installing Git for Windows is easy, thanks for course to <a href="https://chocolatey.org/" target="_blank">Chocolatey.org</a>; if you have not used Chocolatey before installing it is also quite simple. Open a cmd prompt as admimistrator and run

<code>@powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" &amp;&amp; SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin</code>

Now packages are as easy to install as running

<code>choco install git
</code>

Once installed, we need to verify a few system settings:
<ul>
	<li>Go to Advanced system settings (Start &gt;&gt; Control Panel &gt;&gt; System &gt;&gt; Advanced system settings)</li>
	<li>Click on Environment Variables</li>
	<li>In the System variables section locate 'Path' and verify C:\Program Files (x86)\Git\cmd; is there</li>
	<li>Add C:\Program Files (x86)\Git\bin; directly after C:\Program Files (x86)\Git\cmd; with no spaces after the ; (make sure you also end with a ;)</li>
	<li>For SSH commands to work later, you need to add a variable.  In the User variables for <em>user</em> section, click the New... button</li>
	<li>Create a variable named HOME with a value of %USERPROFILE%</li>
	<li>Click OK three times and reboot the computer. After the reboot you</li>
</ul>
After the reboot you should be able to open a cmd prompt and run ssh and not get a "ssh is not a recognized command" message

<img src="/images/fulls/ssh-windows.png" class="fit image">

We are now ready to setup GitHub on Windows. If you haven't done so already, <a href="https://help.github.com/articles/signing-up-for-a-new-github-account/" target="_blank">create a user account on GitHub</a>. There are a few commands we need to run to get everything ready.
<ul>
	<li>git config --global user.name <em>username </em>[where username is your actual username, for example jfrappier]</li>
	<li>git config --global user.email <em>user@example.com </em>[where user@example.com is your actual email that you signed up for GitHub with]</li>
	<li>ssh-keygen -t rsa -C <em>user@example.com</em></li>
	<li>Select the location to save the key, I accepted the default</li>
	<li>Enter a passprhase</li>
</ul>
CD to %userprofile%\.ssh; you should see two files - id_rsa and id_rsa.pub.
<ul>
	<li>Open id_rsa.pub in note pad and copy the contents</li>
	<li>Log into GitHub and click on Settings (the gear icon in the upper right corner) &gt;&gt; ssh keys</li>
	<li>Click Add SSH key</li>
	<li>Provide a name, paste the contents from id_rsa.pub into the key textbox, and click Add key</li>
</ul>
Now switch back to your cmd prompt window
<ul>
	<li>Type ssh -T git@github.com</li>
	<li>Type yes to accept the certificate</li>
	<li>Type the passphrase you set for your key previously</li>
</ul>
You are now authenticated with GitHub, you can now enjoy the 12 days of Commitmas!

<img src="/images/fulls/ssh-github-windows.png" class="fit image">