---
layout: post
author: Jonathan Frappier
title: PowerCLI to Clone a vApp
permalink: /:title/
modified: 2014-11-09
category: articles
tags: [vmware, vsphere, vcenter, powercli, clone vapp, vapp, clone]
share: true
---
So had a need to clone a vApp several times, I finally got around to automating that task thanks again to PowerCLI.  A few things I had to consider; with the New-VApp cmdlet you cannot select portgroups so I had to do that after the vApp was cloned and also needed to put the vApp into a specific folder after it was cloned.  Other than those two considerations, it was actually kind of easy to figure out (at least based on what I needed to accomplish).  Two thing I could not do in this script - place the cloned vApp into a datastore cluster and allow storage DRS to make the initial placement.  Instead, I am relying to SDRS to balance the datastores after the power on operation.  Also I could not force the virtual disk format size, I want them all thick, eager zeroed so instead I ensured the source vApp was set properly.

For this basic script I opted to use a CSV file to list the various values I needed to pass in.  The CSV file contains columns for
<ul>
	<li>name - the name of the cloned vApp</li>
	<li>datastore - the data store I want the vApp placed in</li>
	<li>cluster - the cluster I want the vApp placed in</li>
	<li>template - the source template to clone</li>
	<li>portgroup - the VDS port group I want the VMs attached to (thankfully for me they are all the same, and the Set-NetworkAdapter cmdlet will handle as many virtual NICs you have on the virtual machine - this worked for me)</li>
	<li>folder - the folder I want the vApp placed in</li>
</ul>
It is a manual consideration at this time for the datastore location, which again I am letting SDRS handle balancing after they are powered on.

Here it is, in case you need to accomplish it as well :)
<pre>#Get vApp names and port groups
$CSVfile = "c:\admin\scripts\ehc_vapps.csv"

# Set PowerCLI Options
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore -Confirm:$false | Out-Null

$EHC_vApps = Import-Csv -Path $CSVfile
ForEach ($EHC_vApp in $EHC_vApps)
{
#Creates new vApp
New-VApp -Name $EHC_vApp.name -Datastore $EHC_vApp.datastore -Location $EHC_vApp.cluster -VApp $EHC_vApp.template

#Get list of vApp VMs to set network card
$vApp_vms = Get-VApp $EHC_vApp.name | Get-VM
ForEach ($vApp_vm in $vApp_vms)
{
Get-VM -Location $EHC_vApp.name $vApp_vm | Get-NetworkAdapter | Set-NetworkAdapter -Portgroup $EHC_vApp.portgroup -Confirm:$false
}

#Move vApp to StudentPod folder
Move-VApp -Destination $EHC_vApp.folder -VApp $EHC_vApp.name
}
</pre>