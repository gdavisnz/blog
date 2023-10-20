---
title: "Centralized Point of Management (CPoM) in Cloud Director 10.4.1"
date: 2023-03-25
draft: false
description: "Centralized Point of Management (CPoM) in Cloud Director 10.4.1"
showDate : true
showDateUpdated : false
toc: true
Tags: ["vmware", "vcd", "CPoM"]
summary: "Cloud Director Centralized Point of Management (CPoM) is a UI Plugin that enables providers to present dedicated vCenter instances to their customers."
Author: Grant Davis
cover:
    image: "cover-vcd.png" # image path/url
    alt: "Centralized Point of Management (CPoM) in Cloud Director 10.4.1" # alt text
    caption: "Centralized Point of Management (CPoM) in Cloud Director 10.4.1" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
---

![](diagram.png)

--------
## Introduction
--------
Cloud Director Centralized Point of Management (CPoM) is a UI Plugin that empowers providers to provide customers dedicated vCenter instances. Cloud Director can also act as a proxy which allows providers to simplify tasks such as setting up VPNs and navigating complex NAT configurations. Cloud Director can also act as an API endpoint for all your vcenters, which could simplify automation.

This blog article will run through the installation and configuration of CPoM 


<ul>
<li>Cloud Director can access the vCenter server via HTTPS</li>
<li>System Administrator role in VCD Provider Portal</li>
<li>Login account to the vCenter resource</li>


This steps we are going to take here are:
<ul>
<li>Enable the CPoM feature</li>
<li>Add dedicated vCenter as cloud resource</li>
<li>Publish resource to Tenant</li>
<li>Configure proxy settings</li>
</ul>


----------
## Enabling the CPoM Feature
----------
CPoM can be enabled in the Provider Portal under More >> Customize Portal, select 'CPOM extension' and click <strong>Enable</strong>


![Figure 1: Enable the feture under customize portal](1-enable-CPOM.png)

The CPoM feature will also need to be publised to tenants, while the CPOM extension is still selected click <b>Publish</b> and choose your tenants

![Figure 1a: Publish CPOM extension to Tenant](1a-publishtenant.png)


----------
## Add Dedicated vCenter as Cloud Resource
----------

Next is to add the vCenter server as a Cloud Resource. Click on Resources >> vCenter Server Instances >> Infrastructure Resources >> ADD.
![Figure 2: Add vCenter as cloud resource](2-addvcenter.png)

----------
### vCenter Server
----------
The Add vCenter Wizard opens and you will be prompted for the following:

<ul>
<li><b>Name:</b>  Unique name to identify vCenter instance</li>
<li><b>Description:</b> Optional description field</li>
<li><b>URL:</b> https:// url of vCenter server</li>
<li><b>Username:</b> Username/Password to authenticate with vCenter</li>
<li><b>Password:</b> Username/Password to authenticate with vCenter</li>
<li><b>Enabled:</b> If enabled you will be ready to publish this vCenter to a Tenant.</li>
</ul>



**The below screen shot uses the <b>administrator@ vsphere.local</b> username to authenticate with the vCenter Server,  I couldnt find any offical documentation on this but I tested an account with a <b>Read Only</b> Global Permission role and that also worked fine.**


![Figure 3: Add vCenter as cloud resource wizard](3-addvc-vcenterserver.png)

Once you filled that out Click <b>Next</b>.  Youll get a prompt to retreive and trust the vCenter certificate

![Figure 4: Accept Certificate](4-certificatetrust.png)

----------
### NSX-V Manager
----------

I am going to disable NSX-V Manager by clicking on the <b>Configure Settings</b> slider.  NSX-V is being replaced by NSX-T. 

![Figure 5: NSX Manager](5-addvc-nsx.png)

Click <b>Next</b>


----------
### Access Configuration
----------

Now this is where you actually define this as a dedicated vCenter rather than a shared vCenter resource for orgVDCs. This is done by selecting <b>Enable Tenant Access</b>.  We are also going to enable <b>Generate Proxies</b> so we can walk through that as well. 


![Figure 6: Access Configuration](6-addvc-access.png)

Click <b>Next</b>

----------
### Ready to Complete
----------

Ready to complete is an overview on what we have done so far,  review your settings and then click <b>Finish</b>

![Figure 7: Ready to Complete](7-addvc-ready.png)

I did get prompted to trust the vCenter certificate again.
![Figure 8: Certificate Trust](8-trustagain.png)


You should see the new vCenter now. In the screen shot below I have 2 vCenters with different usage fields, vcsa01 has a usage class of IaaS this is for my tradational orgVDC backed shared compute environment and the vCenter vcsa02 has a usage class of SDDC (Software Defined Data Center) 

![Figure 9: New SDDC resource created.](9-sddcvcenter.png)


----------
## Publish to Tenant
----------

Now to publish this Dedicated Resource to a Tenant, select the SDDC vCenter and click <b>Manage Tenants</b>

![Figure 10: Manage Tenants.](10-managetenants.png)

Select your Tenant and click <b>Save</b>

![Figure 11: Select Tenants.](11-selecttenants.png)


----------
## Proxy Configuration
----------


When we added the vCenter server, we enabled the Generate Proxies setting which created a proxy for us.  This can be view by clicking on the vCenter server and then Proxies on the sidebar.  Here you can also generate new proxies for NSX Managers, ESXi Hosts UI and SSO is there in case there is an external PSC. 

![Figure 12: View Proxies.](12-Proxies.png)




----------
## Extra Configuration
----------

For Tenants to see this new dedicated vSphere instance you'll need to confirm the following, the first 2 have already been done in this exercise.

<ul>
<li>The CPOM Plugin has been published to the tenant.</li>
<li>The vCenter server has been published to the tenant.</li>
<li>Modify rights bundles and tenant roles to access the SDDC Resource</li>
</ul>

![Figure 13: Modify rights bundles and tenant roles to access the SDDC Resource](13-sddcrole.png) 



----------
## Tenant Portal
----------

Lets log into the Tenant Portal, browse to Data Centers >> Dedicated vSphere Data Centers. 

And you should see the new SDDC ready to go!

![Figure 14: Tenant Portal](14-tenantsddc.png) 


----------
### Tenant Portal Proxy Configuration
----------
You may need to action a few steps to configure the proxy capabilities, either by downloading a chrome extension or updating your Windows OS Internet LAN Settings with a automatic configuration script.  When you click <b>Actions</b> under your SDDC Instance and click <b>Username and Password</b> you will get the Proxy credentials needed.

![Figure 15: Proxy Configuration](15-proxyconfig.png) 


![Figure 16: Proxy Credentials](16-proxycreds.png) 



----------
## Conclusion
----------

And there you have it! Customers can have all their dedicated resources in the one UI with a quick glance of capacity requirements and total number of VMs. 

![Figure 17: Final UI in the Tenant Portal](17-final.png) 


<div class="views">
    <span class="views">
        <img src="https://visitor-badge.glitch.me/badge?page_id=gsphere-260323{{ .Permalink }}" alt=""/>
    </span>
</div>