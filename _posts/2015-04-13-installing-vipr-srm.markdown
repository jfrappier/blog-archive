---
layout: post
author: Jonathan Frappier
title: Installing ViPR SRM
permalink: /:title/
modified: 2015-04-13
category: articles
tags: [emc, vipr, virp srm, installation]
share: true
---
<em>*Disclaimer: I am an EMC employee, this post was not sponsored or in any way required by my employer, it is my experience getting to know this particular product.**</em>

In <a title="Getting to know ViPR SRM" href="/getting-to-know-vipr-srm/">my last ViPR SRM post</a>, I introduced you to some of the features if you were not already aware of them. In this post, I will look at installing ViPR SRM 6.5.2. I downloaded ViPR SRM from support.emc.com; while I am an EMC employee, I logged into the support site with my personal email account to download the files. Once logged in, search for ViPR SRM and click on the downloads menu, as I mentioned I will be going with the vApp option versus a binary installation.

<img src="/images/fulls/vipr-srm-search-support.png" class="fit image">

Once downloaded, extract the content of the zip file - you'll have 2 OVF's. One is the 4 VM vApp I mentioned in my last post, the other, a 1VM vApp useful for lab and evaluation purposes. Given I have limited resources in my home lab, I will be deploying the 1 VM vApp.

<a href="/images/fulls/info.png"><img class="alignleft  wp-image-3680" src="/images/fulls/info.png" alt="info" width="50" height="50" /></a>

Important note here, you will need to deploy the OVF to vCenter, not a stand-alone ESXi host as some of the OVF properties will not be exposed properly, causing the deployment to fail.

Follow the OVF deployment wizard, when prompted select the All-In-One configuration:

<img src="/images/fulls/vipr-srm-all-in-one-ovf.jpg" class="fit image">

By default, the VM deploys with 4 vCPU - adjust according to your lab, I have set mine to 2, 16GB RAM and removed the reservation (performance here would not be ideal obviously, but this is for lab purposes only). Once the OVF has been deployed, you should be able to log into <strong>http://&lt;viprsrm DNS or IP&gt;:58080/APG. </strong>Login as admin/change me to access ViPR SRM.

<img src="/images/fulls/vipr-srm-UI.jpg" class="fit image">

By default, you are in the "User" interface, if you click on "Administration" in the upper right corner, you will go to the administration screen. Go ahead and click on Administration &gt;&gt; Centralized Management (on the left nav menu) &gt;&gt; License Management (also on the left nav menu). As you can see you have a 30 day trial license to test out ViPR SRM.

<img src="/images/fulls/vipr-srm-trial-license.jpg" class="fit image">

Close the license window/tab. Notice where the "Administration" menu was, you now see a "User Interface" menu, this will (like the administration link did) take you to the User interface (where you initially landed when you logged in.

In the next post, I will look at connecting ViPR SRM to vCenter and, in my case, XtremIO.