---
layout: post
author: Jonathan Frappier
title: Error Supplied System Name is not valid during vCenter Server Appliance 6 installation
permalink: /:title/
modified: 2015-02-09
category: vcsa
tags: [vcsa, vmware, vsphere, vcenter server appliance, vsphere 6, vcsa 6]
share: true
---

During the installation of of the VMware vCenter Server Appliance (VCSA) 6.0 or the Platform Services Controller (PSC) Appliance 6.0, you receive the following message:

Firstboot script execution Error.

And/or

The supplied System Name [name] is not valid

VMware vCenter Server Appliance (VCSA) and Platform Services Controller (PSC) error during installation - supplied system name is not valid

<img src="/images/fulls/vmware-vcsa-psc-appliance-6-error-system-name-not-valid.png" class="fit image">

Additionally, logs found at %USERPROFILE%\AppData\Roaming\VMware\vSphere\vcsa\sessions\session_####\logs do not provide additional details, only

2015-02-06 22:41:09.330738 Progress Controller: [VCSA ERROR] - First Boot error

This problem is likely due to incorrect DNS configuration, either in the DNS server IP address provided during the VCSA or PSC installation or there is no matching DNS record.

Verify that both forward and reverse DNS lookup zones exist and re-run the installation, validating that DNS is working. Below is an example of running nslookup FQDN. The first when the record doesn't exist, the 2nd after it has been added. Ensure you resolve the expected IP address from NSLOOKUP and re-run the installer.

<img src="/images/fulls/dns.png" class="fit image">