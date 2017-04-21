---
layout: post
title: Remove SMA Sample Runbooks
date: 2015-02-08
tags: ["SMA"]
excerpt_separator: <!--more-->
---

Just a quick post for those of you who would like to remove the Sample SMA Runbooks.

When you stand up SMA you may have about 25+ Sample SMA runbooks present and they tend to clutter up the Windows Azure Pack Automation website. These samples are great, especially when you are getting started! But you may want to remove them just to clean up the interface once you have done some learning and testing.

<!--more-->

When I am working in my lab I like to remove the samples to just focus on a particular project I am working on.


You will need the SMA PowerShell Module installed but this quick simple powershell script will remove all runbooks starting with "Sample"

```
#Get RunBooks starting with Name that start with "Sample"

$SampleRunbooks = Get-SMARunbook -WebServiceEndpoint 'HTTPS://SMAServerHere' ' Where-Object {$_.RunbookName -like 'Sample*'}

#For Each Runbook Found - Remove it!
Foreach ($SampleRunbook in $SampleRunbooks ) {Remove-SMARunbook -WebServiceEndpoint 'HTTPS://SMASERVERHERE' -RunBookName $SampleRunbook.RunBookName}
```

***Replace "SMASERVERHERE" with your HostName**

**You will have specify "-Port" for get hte Get-SMARunbook and Remove-SMARunbook command if you do not use the default port.**

Read more about how to use the SMA PowerShell Module here:
[http://blogs.technet.com/b/orchestrator/archive/2014/03/11/sma-capabilities-in-depth-the-sma-powershell-module.aspx](http://blogs.technet.com/b/orchestrator/archive/2014/03/11/sma-capabilities-in-depth-the-sma-powershell-module.aspx)