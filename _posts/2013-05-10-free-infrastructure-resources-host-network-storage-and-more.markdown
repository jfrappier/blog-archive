---
layout: post
author: Jonathan Frappier
title: Free Infrastructure Resources - Host, Network, Storage, DevOps, and more
permalink: /:title/
modified: 2013-05-10
category: free
tags: [free tools, free, vmware, devops]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
<em>Updated 2/28/2015</em>

Below is a list of free infrastructure resources available to those building labs or on a shoestring budget.  Heck maybe some of these tools can save you a few bucks on certain areas of your project so you can invest in other areas.

<strong>Hypervisors</strong>
<ul>
	<li><a href="http://www.vmware.com/products/vsphere-hypervisor/overview.html" target="_blank">VMware vSphere Hypervisor (ESXi)</a>:  The best platform, IMO, hopefully you can spring for at least Essentials plus</li>
	<li><a href="http://www.citrix.com/products/xenserver/features/editions.html?ntref=next" target="_blank">Citrix Xen Server</a>:  Let's face it, at a startup or SMB we may not be able to afford a commercial license, Xen provides Live Migration for free but good luck supporting it.</li>
	<li><a href="http://www.linux-kvm.org/page/Main_Page" target="_blank">KVM</a>:  Opensource platform, not for the faint of heart</li>
</ul>
<strong>Storage</strong>
<ul>
	<li><a href="http://www.nexenta.com/corp/downloads/download-community-edition" target="_blank">Nexenta Community Edition</a>:  Turn bare metal equipment into a fully functional NAS.  18TB limit on the community edition would be fine for most (all?) SMBs.</li>
	<li><a href="http://www.freenas.org/" target="_blank">FreeNAS</a>:  A very solid open source project with years off support and development behind it.</li>
	<li><a href="http://www.nas4free.org/key_features.html" target="_blank">NAS4Free</a>:  I haven't used it, looks similar to FreeNAS without the name recognition, its still being maintained.</li>
	<li><a href="http://www.falconstor.com/download-iscsi-san-for-free" target="_blank">FalconStor Virtual SAN appliance</a>:  Missing some features (snapshots and HA) but yet another storage solution.</li>
	<li><a href="http://www.quadstor.com/" target="_blank">QuadStor</a>:  An open source application to turn a linux VM or physical machine into a very fully features storage appliance.  I really need to get this in my lab.  HA, Dedup, VAAI!</li>
	<li><a href="http://nickapedia.com/2011/11/03/straighten-up-with-a-new-uber-tool-presenting-uberalign/" target="_blank">UberAlign</a>:  A utility from Nicholas Weaver (@lynxbat) to help fix alignment issues for VMs.</li>
</ul>
<strong>Backup and Recovery</strong>
<ul>
	<li><a href="http://www.vmware.com/solutions/datacenter/business-continuity/data-protection.html" target="_blank">VMware Data Protection (VDP)</a>:  Free with many versions of vSphere and based on EMC Avamar.</li>
	<li><a href="http://www.vmware.com/products/datacenter-virtualization/vsphere/replication.html#glance" target="_blank">VMware Replication</a>:  Another item included for free with many versions of vSphere, get a copy of your data offsite in case of a disaster.</li>
	<li><a href="http://www.phdvirtual.com/disaster-recovery-assurance/editions " target="_blank">PHD Virtual ReliableDR</a>:  What good is backup if your office is gone, Free version has some limitations but like VMware Replication some DR is better than no DR.</li>
	<li><a href="http://www.unitrends.com/unitrends-enterprise-backup/download" target="_blank">Unitrends Enterprise Backup (Free and NFR)</a>:  Fairly limited on the free version, a bit more room for VCP's.</li>
	<li><a href="http://www.quantum.com/Products/vmbackup/vmproSoftware/Index.aspx" target="_blank">Quantum vmPRO</a> and <a href="http://forms.quantum.com/Software/V1000/download.aspx" target="_blank">DXi V1000 Appliance</a>:  15 TB's in the appliance, which includes the ability to replicate to other appliances!</li>
	<li><a href="http://www.symantec.com/connect/blogs/free-fully-licensed-copy-backup-exec-2012-v-ray-edition-vexperts" target="_blank">Symantec BackupExec V-Ray Edition</a>:  Supports unlimited VM's per host!  Free for vExperts and VCP's.</li>
	<li><a href="http://www.trilead.com/Editions/" target="_blank">Trilead</a>:  Backup, more, limited, free.</li>
	<li><a href="http://redobackup.org/" target="_blank">Redo Backup</a>:  Create and restore bare metal images (yes we still have bare metal systems - admit it!)</li>
	<li><a href="https://portal.evault.com/account/register?source=Web&amp;SourceDetail=Free%20Cloud%20Backup" target="_blank">Seagate eVault</a>:  Only good for 1 server, but maybe all you need is 1 server!</li>
	<li><a href="http://nakivo.com/en/MBVMUG1973NFR.htm" target="_blank">Naviko Backup &amp; Replication</a>:  Free for VMUG members, VCPs, vExperts and VCI's</li>
	<li><a href="http://www.clearpathsg.com/backup-AWS-S3-Calculator" target="_blank">ClearpathSG Backup Estimate for S3</a>:  A tool to allow organizations to estimate their cloud backup costs when using Amazon S3/Glacier</li>
</ul>
<strong>Monitoring</strong>
<ul>
	<li><a href="http://www.vmware.com/products/datacenter-virtualization/vcenter-operations-management/compare-editions.html" target="_blank">vCenter Operations Manager Foundation</a>:  Another freebie when you purchase vSphere.  You paid for the awesomest hypervisor, why not take advantage of the tools you got included?</li>
	<li><a href="http://www.veeam.com/virtual-server-management-one-free.html" target="_blank">Veeam One</a>:  You virtualized your infrastructure which is great, but you can't just peak your head into the datacenter and see if everything is flashing green.  Veeam allows you to monitor your virtual infrastructure.</li>
	<li><a href="http://www.robware.net/" target="_blank">RVTools</a>:  I haven't had a chance to test this yet, lots of people swear by it.</li>
	<li><a href="http://xangati.com/" target="_blank">Xangati</a>:  Another handy tool to watch over your gear, free version is good for one server which, if you have one server, is actually great.</li>
	<li><a href="http://www.nagios.org/" target="_blank">Nagios </a>and <a href="https://www.icinga.org/" target="_blank">Icinga</a>:  The defacto open source monitoring tool and its fork.  Developers can't get along forever.</li>
	<li><a href="http://www.opsview.com/solutions/core" target="_blank">Opsview</a>:  Based on Nagios, provides a nicer GUI, though honestly its been 4 years since I used Nagios core.</li>
	<li><a href="http://www.op5.com/explore-op5-monitor/price-and-comparison/" target="_blank">op5 Monitor</a>:  Another based on Nagios, open source code for unlimited monitoring and a free version with limited support for up to 20 devices.</li>
	<li><a href="http://www.quest.com/foglight-for-virtualization-free-edition/" target="_blank">Dell Foglight for Virtualization (Free)</a>:  A package of several free resources from Dell/Quest.  I am glad to see Dell is still maintaining the free products.  Vladan Seget has a nice write up <a href="http://www.vladan.fr/foglight-for-virtualization/" target="_blank">here</a> on the features.</li>
	<li><a href="www.monitor.us/sign-in">Monitor.us</a>:  Web based monitoring application, some limitations as all freemium products do.</li>
	<li><a href="http://www.scalextreme.com/products/basic-edition" target="_blank">ScaleXtreme</a>:  Web based monitoring application, some limitations as all freemium products do.</li>
	<li><a href="http://www.virtu-al.net/vcheck-pluginsheaders/vcheck/" target="_blank">vCheck</a>:  A PoSH script by <a href="https://twitter.com/alanrenouf" target="_blank">Alan Renouf</a> that emails you status of your environment.</li>
	<li><a href="http://www.netwrix.com/change_reporter_for_vmware_infrastructure_3.html" target="_blank">Netwrix VMware Change Reporter</a>:  Similar to Alan's script (not actually tested) but monitors changes to VM settings.</li>
	<li><a href="http://www.toutvirtual.com/products/viq_525.php" target="_blank">VirtualIQ Free</a>:  For up to 2 hosts/5 CPU and 25 VM's.</li>
	<li><a href="http://indeni.com/healthcheck" target="_blank">Indeni Health Check</a>:  Free, light version of Indeni's Dynamic Knowledge base.</li>
	<li><a href="http://www.ignitefree.com/">IgniteFree</a>:  Database monitoring tool from Confio for MS SQL and Oracle.  Wish there was a PostgreSQL version!</li>
	<li><a href="http://www.lucd.info/2013/09/02/event-o-matic/" target="_blank">Event-O-Matic</a>:  A PoSH/PowerCLI script generator that assists in the collection of specific log files from Luc Dekens</li>
	<li><a href="http://www.extrahop.com/discovery/" target="_blank">ExtraHop Discovery Edition</a>:  Watch your apps for real time performance problems</li>
	<li><a href="http://www.solarwinds.com/alertcentral.aspx" target="_blank">SolarWindows Alert Central</a>:  Helps you escalate alerts to certain individuals or groups versus sending to groups.</li>
	<li><a href="http://www.alienvault.com/open-threat-exchange/projects" target="_blank">AlienVault OSSIM OpenSource</a>:  Awful lot of free monitoring tools huh, this one attempts to correlate logs/monitoring from various open source tools to help you identify the cause of an issue.  This may also be the easiest Snort install you could do!</li>
	<li><a href="http://www.tripwire.com/securescan/" target="_blank">Tripwire SecureScan</a>:  Free for up to 100 IP's</li>
	<li><a href="http://www.opvizor.com/editions/" target="_blank">Opvizor</a>: Free edition supports a single cluster with up to 48 hours of data</li>
	<li><a href="http://try.opvizor.com/snapwatcher/" target="_blank">Opvizor Snapwatcher</a>: Free during beta, find and nuke snapshots, even corrupt ones</li>
	<li><a href="http://www.cloudphysics.com/pricing/" target="_blank">CloudPhysics</a>:  Community edition of the powerful monitoring solution that backs alerts with aggregated data</li>
	<li><a href="https://www.datadoghq.com/pricing/" target="_blank">DataDog</a>:  Monitor up to 5 hosts with various dashboards.  Free version only keeps data for 1 day</li>
	<li><a href="http://vmturbo.com/product/virtualization-monitoring-software/?utm_source=virtxpert&amp;utm_medium=cpmdisplay&amp;utm_campaign=free-tool" target="_blank">VMTurbo Virtual Health Monitor</a>:  Monitor your VM environment in real time</li>
	<li><a href="http://www.zabbix.com/download.php" target="_blank">Zabbix OpenSource</a>: Another popular monitoring tool, thanks for the suggestion Tyron</li>
	<li><a href="http://ghostinspector.com" target="_blank">Ghost Inspector</a>: Web application testing, free for up to 100 tests</li>
</ul>
<strong>Logging</strong>
<ul>
	<li>VMware Syslog Collector and Dump Collector:  Included in vCenter download - at some point there is going to be a problem/error and having your logs centrally collected will be nice.  Seems like busy work now but you will be glad when you need it.</li>
	<li><a href="http://www.splunk.com/" target="_blank">Splunk</a>:  Free version, as most do, has some limitations but as you grown you can remove these with a paid version.</li>
	<li><a href="http://graylog2.org/home" target="_blank">GreyLog</a>:  A new logging tool to the OS world, built on MongoDB.  Installation script from <a href="https://github.com/mrlesmithjr/graylog2" target="_blank">GitHub</a></li>
	<li><a href="http://www.balabit.com/network-security/syslog-ng/opensource-logging-system" target="_blank">Syslog-ng</a>:  Moar syslog</li>
	<li><a href="http://nxlog.org/download" target="_blank">nxlog</a>:  You need a way to get your Windows logs to these cool syslog servers!</li>
	<li><a href="http://logfilemonitoring.com/" target="_blank">Nagios Log Monitor</a>: Check out <a title="Installing Nagios Log Server" href="http://www.virtxpert.com/installing-nagios-log-server/" target="_blank">my write up here</a></li>
	<li><a href="http://www.elasticsearch.org/overview/" target="_blank">ELK</a>: A combination of opensource tools, check out the <a href="http://everythingshouldbevirtual.com/highly-available-elk-elasticsearch-logstash-kibana-setup" target="_blank">write up by Larry Smith Jr</a> and his <a href="http://professionalvmware.com/vbrownbag-devops-series/" target="_blank">#vBrownBag presentation</a></li>
	<li><a href="http://www.sexilog.fr/" target="_blank">SexiLog</a>: An ELK stack developed by members of the virtualization community <img src="http://www.virtxpert.com/wp-content/uploads/2013/09/new2.png" alt="new2" width="39" height="13" /></li>
</ul>
<strong>Automation and DevOps</strong>
<ul>
	<li><a href="http://communities.vmware.com/community/vmtn/automationtools/powercli" target="_blank">VMware PowerCLI</a>:  Free, included, have I mentioned all the stuff thats included with your paid vSphere license?  I think I did</li>
	<li><a href="http://www.vmware.com/products/vcenter-orchestrator/buy.html" target="_blank">vCenter Orchestrator</a>:  Yea you guessed it, included when you buy</li>
	<li>Windows PowerShell:  Installed with Windows - Its there, why click around when you can write a script (I am guilty of not doing this enough)</li>
	<li><a href="http://www.autoitscript.com/site/autoit/" target="_blank">AutoIT</a>:  Another popular scripting platform</li>
	<li><a href="https://puppetlabs.com/solutions/" target="_blank">Puppet</a>:  I will start by saying I don't know enough about it, I need to learn more, the Enterprise version is <a href="https://puppetlabs.com/puppet/enterprise-vs-open-source/">free for up to 10 nodes</a></li>
	<li><a href="http://www.ansibleworks.com" target="_blank">Ansible</a>:  Another configuration management tool, similar to Puppet yet so far easy to use.  Free for up to 10 nodes</li>
	<li><a href="http://www.getchef.com/enterprise-chef/" target="_blank">Chef</a>:  Another automation platform popular with the DevOps movement, a free and paid version like Puppet and Ansible</li>
	<li><a href="http://land.cloudboltsw.com/download-cloudbolt-c2-ve" target="_blank">CloudBlot C2</a>:  Claims to offer an easy to use IaaS tool, taking shots at vCAC.  Free for up to 100 VMs</li>
	<li><a href="https://solutionexchange.vmware.com/store/products/esx-deployment-appliance-eda#.U0cUzfnxrDI" target="_blank">ESX Deployment Appliance</a>:  Free appliance to help automate the installation of ESX/ESXi</li>
	<li><a href="http://www.ultimatedeployment.org/" target="_blank">Ultimate Deployment Appliance</a>:  Helps automate the installation of both deploy Windows and Linux</li>
	<li><a href="https://labs.vmware.com/flings/web-commander" target="_blank">VMware WebCommander</a>:  Part of the Fling labs, publish PowerShell and PowerCLI scripts via a self service portal to your users</li>
	<li><a href="https://labs.vmware.com/flings/vcs-to-vcva-converter" target="_blank">Windows vCenter to VCSA Fling</a>: Fling from VMware to migrate Window vCenter to the vCenter Virtual Appliance</li>
	<li><a href="http://rundeck.org/index.html" target="_blank">RunDeck</a>: Another simple to start with automation platform</li>
	<li><a href="https://stackstorm.com/" target="_blank">StackStorm</a>: An "IFTTT" solution for the enterprise</li>
	<li><a href="https://ghostinspector.com/pricing/" target="_blank">Ghost Inspector</a>: A free web testing automation tool</li>
	<li><a href="https://about.gitlab.com/" target="_blank">GitLab</a>: Free community edition for behind the firewall Git goodness <img src="http://www.virtxpert.com/wp-content/uploads/2013/09/new2.png" alt="new2" width="39" height="13" /></li>
</ul>
<strong>Networking</strong>
<ul>
	<li><a href="http://www.zenloadbalancer.org/web/" target="_blank">Zen Loadbalancer</a>:  Sometimes you need more than one server, web, View Connection etc but real load balancers get pricey</li>
	<li><a href="http://vyos.net/wiki/Main_Page" target="_blank">VyOS</a>: The replacement for Vyatta</li>
	<li><a href="http://www.pfsense.org/" target="_blank">pfSense</a>:  Another OS load balancer</li>
	<li><a href="http://www.untangle.com/store/lite-package.html" target="_blank">Untangle</a>:  Free firewall, really just several other OS projects packaged together but has a nice GUI and means you don't have to package them together.</li>
	<li><a href="http://www.smoothwall.org/" target="_blank">Smoothwall</a>:  Yet another OS firewall</li>
	<li><a href="http://www.endian.com/us/community/overview/" target="_blank">Endian </a>:  Yet another OS firewall</li>
	<li><a href="http://www.packetfence.org/home.html" target="_blank">PacketFence</a>:  OpenSource Network Access Control</li>
	<li><a href="http://www.sophos.com/en-us/products/free-tools/sophos-utm-home-edition.aspx" target="_blank">Sophos UTM Home</a>:  Only really good for a home lab, but many of us have home labs so here's a firewall</li>
	<li><a href="http://www.opendaylight.org/" target="_blank">Open DayLight</a>:  SDN is the future, why not get started here</li>
	<li><a href="http://freeloadbalancer.com/" target="_blank">Kemp Load Balancer</a>: A seemingly free, yet full functional and optimized load balancer</li>
</ul>
<strong>VDI</strong>
<ul>
	<li><span style="line-height: 13px;"><a href="http://www.tucloud.com/DaaS_Engine.html" target="_blank">TuCloud DaaS Engine</a>:  Yet another product I don't have time to research!  </span></li>
	<li><a href="http://labs.vmware.com/flings/vmware-os-optimization-tool" target="_blank">VMware View OS Optimization Fling</a>:  Optimize Windows 7 by the use of VMware best practices such as disabling unnecessary services.</li>
	<li><a href="http://www.nektra.com/services/application-virtualization/" target="_blank">Nektra SpyStudio:</a>  Free for non-commercial use, but fairly inexpensive add on which claims to simplify ThinApp creation.</li>
</ul>
<strong>Patching</strong>
<ul>
	<li><span style="line-height: 13px;"><a href="http://technet.microsoft.com/en-us/windowsserver/bb332157.aspx" target="_blank">Windows WSUS</a>:  Think it goes without saying that Windows needs to be patched</span></li>
	<li><a href="http://spacewalk.redhat.com/" target="_blank">RedHat Spacewalk</a>:  Yes even Linux needs patches, you dont want a vulnerable Apache web server kicking around do you?</li>
	<li><a href="http://www.vmware.com/products/datacenter-virtualization/vsphere/update-manager.html" target="_blank">VMware Update Manager (VUM)</a>:  Something something, included vSphere something.  I wish VMware Go was still around, free and made patching a breeze</li>
</ul>
<strong>Training</strong>
<ul>
	<li><span style="line-height: 13px;"><a href="http://mylearn.vmware.com/portals/www/mL.cfm?menu=topfreecourses" target="_blank">VMware MyLearn</a>:  Lots of great free classes from new features, DR training and even advanced topics such as vCloud Director.</span></li>
	<li><a href="http://vmwarelearning.com/" target="_blank">VMware Learning</a>:  An extension of MyLearn (IMO) with lots of free great videos, versus the slide/formal training style on MyLearn.</li>
	<li><a href="http://www.vmug.com/" target="_blank">Local VMUG</a>:  Great opportunity to meet new people and learn about the latest from the VMware community and vendors.</li>
	<li><a href="http://www.codecademy.com" target="_blank">CodeAcademy</a>:  Coding = automation.</li>
	<li><a href="http://professionalvmware.com/" target="_blank">#vBrownBag</a>:  Community contributed podcasts primarily focused on VMware but also on various technology topics including <a href="http://openstack.prov12n.com/" target="_blank">OpenStack</a>.</li>
	<li><a href="http://labs.hol.vmware.com/" target="_blank">VMware Hands On Labs (HOL)</a>:  Free hands on training on various topics directly from VMware.</li>
	<li><a href="http://www.virtualdesignmaster.com" target="_blank">Virtual Design Maste</a>r:  A free, live competition that puts aspiring architects skills to the test.  Currently in Season 2</li>
	<li><a href="http://www.twistedethics.com/2014/07/13/completely-free-it-training-resources-to-help-diversify-your-it-career/" target="_blank">Phil Wiffen's TwistedEthics.com Free Training List</a>:  Great list of free training resources Phil as curated</li>
</ul>
<strong>Community Contributed Certification Resources </strong>
<ul>
	<li><a href="https://twitter.com/mwpreston" target="_blank">Mike Preson</a>'s VCP5 Study Guide and practice tests:  <a href="http://blog.mwpreston.net/vcp-5/">http://blog.mwpreston.net/vcp-5/</a></li>
	<li><a href="https://twitter.com/SimonLong_" target="_blank">Simon Long</a>'s practice tests:  <a href="http://www.simonlong.co.uk/blog/vcp5-practice-exams/">http://www.simonlong.co.uk/blog/vcp5-practice-exams/</a></li>
	<li><a href="https://twitter.com/GreggRobertson5" target="_blank">Gregg Robertson</a>'s VCAP5-DCD and DCA resources:  <a href="http://thesaffageek.co.uk/vsphere-5-study-resources/vcap5-dca-dcd/">http://thesaffageek.co.uk/vsphere-5-study-resources/vcap5-dca-dcd/</a></li>
	<li><a href="https://twitter.com/PaulPMeehan" target="_blank">Paul Meehan</a>VCAP5-DCD Guide:  <a href="http://paulpmeehan.com/vcap-dcd-study-guide/">http://paulpmeehan.com/vcap-dcd-study-guide/</a></li>
	<li><a href="https://twitter.com/jaslanger" target="_blank">Jason Langer</a> and <a href="https://twitter.com/joshcoen" target="_blank">Josh Coen</a>'s VCAP5-DCA Study Guide:  <a href="http://www.virtuallanger.com/vcap-dca-5/">http://www.virtuallanger.com/vcap-dca-5/</a></li>
	<li><a href="https://twitter.com/coolsport00" target="_blank">Shane Williford</a>'s DCA Study Guide with CLI examples:  <a href="http://professionalvmware.com/2012/11/vcap5-dca-study-outline-pdf/">http://professionalvmware.com/2012/11/vcap5-dca-study-outline-pdf/</a></li>
	<li><a href="https://twitter.com/vBrownBag" target="_blank">#vBrownBag</a> podcasts on all of them! VCP, VCAP, VCDX:  <a href="http://professionalvmware.com/brownbags/">http://professionalvmware.com/brownbags/</a></li>
</ul>
<strong>Misc</strong>
<ul>
	<li><a href="http://blogs.vmware.com/vsphere/2013/03/new-fling-vcenter-5-1-pre-install-check-script.html" target="_blank">VMware vCenter 5.1 Pre-Install Check Script</a>:  A handy PowerShell script from <a href="https://twitter.com/alanrenouf" target="_blank">Alan Renouf </a>that helps in ensuring your environment is setup properly for SSO.</li>
	<li><a href="https://www.vmware.com/products/datacenter-virtualization/vcenter-support-assistant/overview.html" target="_blank">VMware Support Assistant</a>:  An appliance from VMware that assists in submitting support tickets.  Why wouldn't you install this?</li>
	<li><a href="http://www.v-front.de/p/esxi-customizer.html#download" target="_blank">ESXi Customizer</a>:  A pretty way to add VIBs to create custom ISOs</li>
	<li><a href="http://www.hotlink.com" target="_blank">HotLink</a>:  A plugin to allow management of different hypervisors all through vCenter.  Includes support for Hyper-V, XenServer or KVM.  Maximum of 3 hosts in the free version.</li>
	<li><a href="http://communities.vmware.com/community/vmtn/vcenter/multi-hypervisor-manager" target="_blank">vCenter Multi-Hypervisor Management (MHM)</a>:  From VMware to allow management of Hyper-V from vCenter.</li>
	<li><a href="http://communities.quest.com/community/vworkspace/blog/2011/09/08/introducing-the-free-quest-vworkspace-desktop-optimizer" target="_blank">Dell Quest vWorkspace Desktop Optimizer</a>:  For those looking to create a virtual desktop environment, this tool will help configure your desktop settings for maximum performance.</li>
	<li><a href="http://meraki.cisco.com/products/systems-manager" target="_blank">Meraki MDM</a>:  Mobile device management for free!</li>
	<li><a href="http://www.netwrix.com/freeware_products.html" target="_blank">Netwrix free tools</a>:  Various tools that may be useful in your environment.</li>
	<li><a href="http://www.mremoteng.org/" target="_blank">mRemoting</a>:  Open Source RDP client replacement</li>
	<li><a href="http://www.visionapp.com/?id=2504" target="_blank">VisionApp</a>:  Not Open Source RDP client replacement but has a free version.</li>
	<li><a href="http://kitty.9bis.net/" target="_blank">KiTTY</a>:  Forked PuTTY</li>
	<li><a href="http://www.millardsoftware.com/puttycs" target="_blank">PuTTYCS</a>:  A solution to send command to multiple PuTTY sessions at one time.  Doesn't seem to work with KiTTY though.  Not quite as powerful as <a href="http://sourceforge.net/projects/clusterssh/" target="_blank">ClusterSSH</a> (<a href="https://code.google.com/p/csshx/" target="_blank">Mac version</a>) but still handy.</li>
	<li><a href="http://www.loadui.org" target="_blank">LoadUI</a>:  An open source project to generate load test on web applications.</li>
	<li><a href="http://www.5nine.com/products.aspx" target="_blank">5nine</a>:  Several free Hyper-V management tools such as a V2V converter and security scanner.</li>
	<li><a href="https://www.whitehatsec.com/aviator/" target="_blank">Aviator</a>:  Security focus browser based on Chrome</li>
	<li><a href="http://www.veeam.com/vmware-esx-stencils.html" target="_blank">Visio Stencils</a>:  Free Visio stencils from Veeam</li>
	<li><a title="Flat Virtualization Icons for PowerPoint or Documentation" href="http://www.virtxpert.com/flat-virtualization-icons-powerpoint-documentation/">Flat Virtualization Icons</a>:  Icons created by me to use in documentation, presentations, etc...</li>
	<li><a href="http://www.iometer.org/doc/downloads.html" target="_blank">IOMeter</a>:  A tool to do basic testing of storage</li>
	<li><a href="http://blogs.vmware.com/vsphere/2013/04/vcenter-certificate-automation-tool-now-available.html" target="_blank">vCenter Certificate Management</a>:  Improve management of certificates</li>
	<li><a href="http://www.derekseaman.com/2014/01/vsphere-5-5-toolkit-v1-55-released.html" target="_blank">vSphere 5.5 Toolkit by Derek Seaman</a>:  A great tool for managing SSL certificates</li>
	<li><a href="https://labs.vmware.com/flings/vmware-os-optimization-tool" target="_blank">VMware OS Optimization Tool</a>:  Currently part of the Fling labs, updated July 30th 2014</li>
	<li><a href="http://vmturbo.com/resources/datacenter-stencils/?utm_source=virtxpert&amp;utm_medium=cpmdisplay&amp;utm_campaign=stencils" target="_blank">VMTurbo Virtualization Stencils</a>:  A set of stencils for Visio or OmniGiraffe</li>
</ul>
<strong>Collaboration</strong>
<ul>
	<li><a href="https://slack.com" target="_blank">Slack</a>:  A group messaging tool with search, paid versions retain more history</li>
	<li><a href="http://www.asana.com" target="_blank">Asana</a>:  A great task manager, share tasks with others as well</li>
	<li><a href="http://www.trello.com" target="_blank">Trello</a>:  A slightly different take on managing your todo items</li>
	<li><a href="http://www.mangoapps.com/pricing" target="_blank">MangoApps</a>:  As with any free SaaS service, some limitations</li>
	<li><a href="http://www.socialcast.com/pricing" target="_blank">SocialCast</a>:  Free for up to 50 users with some limitations</li>
	<li><span style="line-height: 13px;"><a href="http://developer.sharetronix.com/download" target="_blank">Sharetronix</a>:  Hey your still reading?  Cool, probably wondering what this is all about.  My other passion is collaboration, I hate email and "social" seems like a much better way to communicate</span></li>
	<li><a href="http://www.zimbra.com/products/zimbra-open-source.html" target="_blank">Zimbra OpenSource</a>:  Open source edition of Zimbra for email and calendaring</li>
</ul>
Got one (or more) that you want added?  Send me a message.

Contributors to the list:  <a href="https://twitter.com/NerdBlurt" target="_blank">Luigi Danakos</a>, <a href="https://twitter.com/mmars" target="_blank">Michael Mars</a>, <a href="https://twitter.com/GreggRobertson5">Gregg Robertson</a>, <a href="http://twitter.com/mjbrender" target="_blank">Matthew Brender</a>

&nbsp;