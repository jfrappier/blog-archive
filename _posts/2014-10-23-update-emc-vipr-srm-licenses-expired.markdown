---
layout: post
author: Jonathan Frappier
title: Update EMC ViPR SRM licenses after they have expired
permalink: /:title/
modified: 2014-10-23
category: vipr
tags: [vipr, vipr srm, license]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career,", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
If you are running EMC ViPR SRM, and your license key expires you will no longer be able to log into the UI where you could have installed a new license key.  Instead you will need to update the license(s) via the command line.  The directions I had found had <del>a mistake</del> were unclear, so thought I'd publish the steps that worked for me here.

First and foemost, obtain your new license key by submitting a SR (Service Request) via support.emc.com and follow the steps below.
<ol>
	<li>Launch WinSCP or your file copy tool of choice</li>
	<li>Connect to your ViPR SRM front end server and login in as a user who can elevate privileges (Default root/Changeme1!)</li>
	<li>Navigate to /opt/APG/</li>
	<li>Upload the license key zip file (which may have multiple license files
<ol>
	<li>If not already, name the file licenses.zip - It gave me an error when it was  not named that</li>
</ol>
</li>
	<li>SSH to your ViPR SRM FE server</li>
	<li>Login as  a user who can elevate privileges</li>
	<li>Run:</li>
</ol>
<p style="padding-left: 60px;">/opt/APG/bin/manage-licenses.sh install /opt/APG/licenses.zip</p>
<p style="padding-left: 60px;">/opt/APG/bin/manage-modules.sh service restart tomcat</p>
You should now be able to log in

<img src="/images/fulls/viprsrm.png" class="fit image">

Now that I was able to log in, I still had to upload my licenses through the UI and synchronize my licenses. Click on Administration in the upper right corner, then click on Licenses Management. Click the Upload button and re-upload the license zip file (in some cases I have heard that expired licenses needed to be removed first). Now click the Synchronize button and OK.  You should now be able to use all licenses again.

<img src="/images/fulls/vipr-srm-license-syncronize.png" class="fit image">