---
layout: post
author: Jonathan Frappier
title: Basic PowerCLI examples for common VMware vSphere Tasks
permalink: /:title/
modified: 2013-12-28
category: powercli
tags: [vmware, vsphere, vcenter, powercli, powershell, automation]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
These examples are not meant to reflect best practices, or even efficiency, rather they are barebones PowerCLI examples of how multiple commands can be used within a script to perform common tasks.  Only where necessary where items such as variables or inputs used.

<strong>Live Demo</strong>:  This script was used at the Providence VMUG to do a quick demo of PowerCLI.  It uses Read-Host to prompt the user for input, rescans storage, uses the Get-ScsiLun cmdlet to find all new LUNs that are equal to 40GB then creates a new datastore on my demo host using the CanonicalName property (e.g. path).  You'll notice -VMHost and the size of the datastore in the Get-ScsiLun cmdlet are static, you should either prompt for input on these or read them from a CSV.
<pre>#Prompt user for datastore name
$DSName = Read-Host "Enter the name for the new datastore"

#Rescan HBAs for new storage devices
Get-VMHostStorage -RescanAllHba

#Look for newly added LUNs
$LUNPaths = Get-ScsiLun | Where-Object {$_.CapacityGB -eq 40}

ForEach ($LUNPath in $LUNPaths)
{
New-Datastore -VMHost 192.168.239.128 -Vmfs -Name $DSName -Path $LUNPath.CanonicalName
}

#Prompt user for NIC and switch name
$VSSNIC = Read-Host "Enter the NIC you wish to use for the new vSwitch"
$VSSName = Read-Host "Enter the name for the new switch"

#Create new vSwitch
New-VirtualSwitch -Name $VSSName -Nic $VSSNIC

#Create QA Port Group
New-VirtualPortgroup -VirtualSwitch $VSSName -Name QA
</pre>
<strong>Create a new VM</strong>:  This script prompts the user for specific selections and passes them to the New-VM commandlet.  This example from the VMware vSphere Resource Management book assumed the network names were static, however we could have also added a Get-VirtualPortGroup command to show all the available portgroups and ask the user to enter the port group name as we did for VMName and HOSTName.
<pre>$VMName = Read-host "Enter the name of the VM you wish to create"

#Lists host for the connected vCenter, prompts user to enter the desired host location for the VM

Get-VMHost | Format-Wide
$HOSTName = Read-host "Enter the Name of the Host you wish to create the VM on"

#Lists datastores for the connected vCenter, prompts user to enter the desired datastore location for the VM

Get-Datastore | Format-Wide
$DSName = Read-Host "Enter the name of the datastore you wish to create the VM on"

#Lists VM folders for the connected vCenter, prompts user to enter the desired folder location for the VM

Get-Folder | Format-Wide
$FOLDERName = Read-host "Enter the name of the folder you wish to create the VM in"

#Takes input from above steps and passes them to the New-VM command for VM creation

New-VM -Name $VMName -Location $FOLDER Name -VMHost $HOSTName -Datastore $DSName -Version v9 -GuestId windows8Server64Guest -NumCpu 2 -MemoryMB 8192 -DiskMB 40960 -DiskStorageFormat Thin -NetworkName Development, "VM Network"

#Once VM creation is completed, VM network adapters are converted to vmxnet3

Get-VM $VMName | Get-NetworkAdapter | Set-NetworkAdapter -Type vmxnet3</pre>