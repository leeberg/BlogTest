---
layout: post
title: Pulling the plug on your hybrid runbook worker (Force Remove-HybridRunbookWorker)
date: 2016-01-23
tags: ["Automation","Azure Automation","Hybrid Runbook Worker","SCORCH"]
excerpt_separator: <!--more-->
---


I had a situation where I was having trouble REMOVING my Azure Automation Hybrid Runbook Worker. I could not successfully run the "Remove-HybridRunbookWorker" command as I did not the URL / Key Information.

I my instance, I had deleted my Azure Automation Account in Azure, even though I had a Hybrid Runbook Worker registered to that Account, I did not remove the hybrid runbook worker before I removed the Automation account.

This post will provide an option for resolving this situation:

<!--more-->

Later, when I wanted to setup the Hybrid Runbook Worker on the SAME server that had previously been configured to that automation subscription I was met with the error: "Machine is already registered"

![machine is registerd]({{ site.baseurl }}/assets/img/machine-is-registerd.png)


Hmm, so my machine is registered to something that no longer exists, OK, I can probably use the "Remove-HybridRunbookWorker" Command right? Not at this time, because you have to specify the URL and Key for the command to work (which in my case no longer exist)

After taking a moment to think about how this command worked, I figured there may be a registry check going on that stored the OLD runbook worker configuration information.

Indeed, you will find the current configuration objects stored in the following registery location:

**<span style="color: #000000;">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\HybridRunbookWorker</span>**:
![registry]({{ site.baseurl }}/assets/img/registry.png)



Since this was my lab and I had no fear of issues, I simply deleted the entire "HybridRunbookWorker" Key folder. This essentially deleted all the local hybrid workers references to my previous azure automation account.

&nbsp;

Afterwards, I was able to run the:

```Add-HybridRunbookWorker -Name <String> -EndPoint <Url> -Token <String> command without any issues and the server is now happily functioning as a hybrid runbook worker registered with a new azure automation subscription again.```


**Before:**

![no groups]({{ site.baseurl }}/assets/img/no-groups.png)

**After:**

![after]({{ site.baseurl }}/assets/img/after.png)

&nbsp;

&nbsp;

&nbsp;