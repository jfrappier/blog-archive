---
layout: post
author: Jonathan Frappier
title: Error creating tenants in vCAC with vSphere SSO
permalink: /:title/
modified: 2014-07-14
category: articles
tags: [vmware, vsphere, vra, vcac, sso]
share: true
---
Just a heads up to anyone looking to deploy vCAC using vSphere SSO instead of the vCAC SSO appliance, in 5.5.0 U1, U1a, and U1b you will have problems adding tenants or new users.  The fix is to replace a JAR file which can be found at the link below along with a more detailed description of the error and solution.  I ran up against this recently, and if it weren't for another error I'm troubleshooting to bring my vCAC SQL server online I'd verify if it works.

<a href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=2081730" target="_blank">http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=2081730</a>
<ul>
	<li>Creating tenants fails and the System Exception error is displayed.</li>
	<li>Adding or deleting users from tenants fails and the System Exception error is displayed.</li>
	<li>In the vCloud Automation Center /var/log/vmware/vcac/catalina.out log file, you see errors similar to:</li>
</ul>

<pre>[authentication] ERROR com.vmware.vcac.platform.service.rest.resolver.ApplicationExceptionHandler.handleUnexpectedException:860 - Error registering relying party for tenant:tenant-name com.vmware.vcac.platform.service.SSOException: Error registering relying party for tenant -tenant-name
at com.vmware.vcac.authentication.service.sso.impl.TenantManagementImpl.ensureTenantConfigured(TenantManagementImpl.java:125)</pre>
<ul>
	<li>In the SSO support bundle file located at ssosupport-timestamp.zip/ssosupport-timestamp/Single Sign-On Service/runtime/VMwareSTS/logs/ssoAdminServer.log, you see errors similar to:</li>
</ul>
<pre>ERROR com.vmware.identity.admin.vlsi.ConfigurationManagementServiceImpl] org.xml.sax.SAXParseException; lineNumber: 1; columnNumber: 1; JAXP00010001: The parser has encountered more than "100" entity expansions in this document; this is the limit imposed by the JDK. java.lang.AssertionError: org.xml.sax.SAXParseException; lineNumber: 1; columnNumber: 1; JAXP00010001: The parser has encountered more than "100" entity expansions in this document; this is the limit imposed by the JDK. at com.vmware.identity.idm.client.CasIdmClient.getSamlSchema(CasIdmClient.java:2871)</pre>