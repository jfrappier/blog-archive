---
layout: post
author: Jonathan Frappier
title: Configure Fabric Groups - vRealize Automation Series Part 9
permalink: /:title/
modified: 2014-11-19
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
Time for Fabric Groups, and no a fabric group is not what Grandma does on Saturday afternoons at the senior center.  Fabric groups in the vRealize Automation / vCloud Automation Center world is a collection of resources, this tends to send folks who have been storage focused for a long time down a different path as they start thinking about zoning and switches.

There can be multiple fabric groups with different purposes, for example you may assign clusters to different business groups to ensure performance, or at least that one group does not "hog" all of the resources available (though as we'll see later there are other ways to control that).

As this is a resource being configured, we again log in with someone that as the Infrastructure admin role, in our case the iaasadmin user :
<ul>
	<li>Click on the Infrastructure tab &gt;&gt;  Groups &gt;&gt; Fabric Groups</li>
	<li>Click on New Fabric Group</li>
	<li>Provide a name for your fabric group, in my case I'll use vxprt</li>
	<li>Assign a user to as a fabric administrator - remember we may have different groups using different fabrics and you may want to have someone in engineering manage their resources and a separate person in QA to manage the resources in their fabric group.  Or you could have a single fabric group that is assigned to various users.  The choice is yours.  In my case I am going to assign the iaasadmin user as the fabric administrator.  Start typing the name in, click the magnifying glass icon then click on the user</li>
	<li>In compute resources, you will see the cluster from the vCenter server you added when you added the vSphere endpoint, had you not followed the last blog post you would have no endpoint, thus no resources - unless you already knew to do that on your own of course!  Select your cluster and click the OK button</li>
</ul>
For fun, click on New Fabric group again - did you think your cluster assigned to your previous admin group would still be available?  Logic might suggests that once I assign a cluster to a fabric group I should not be able to reuse it, however fabric groups are not how we control resource consumption, they are used for administration by other users.

That's it, pretty easy.  If you like you can create multiple fabric groups to mimic what you might do in a production environment or play around with adding different clusters if you have those kinds of resources.  In my next post we will setup machine prefixes and business groups - which have to be done in that order, you can't create a business group without a machine prefix (seems out of order to me but hey I'm not a programmer).