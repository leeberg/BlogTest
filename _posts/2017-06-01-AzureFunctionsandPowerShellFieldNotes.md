---
layout: post
title: Top 3 Considerations for Azure Functions with Azure PowerShell
date: 2017-05-30
tags: ["Azure Functions","PowerShell", "Azure PowerShell"]
excerpt_separator: <!--more-->
---

A few weeks ago, I deployed a number of PowerShell Azure Functions for a very large customer. These functions were very simple and basically filled a gap to manage and schedule Azure Automation Runbooks. Azure Automation is a good product but it has some large gaps in its management and scheduling capabilities that we've filled with Azure PowerShell Functions.

Specifically, we have PowerShell Azure Functions to use the AzureRM PowerShell CMDlets to login to the Azure Subscription for the purpose of executing to evaluating Azure Automation jobs on Hybrid Runbook Workers.

After a few weeks, we had some major issues come up, that I believe are now resolved. Based on my experiences I have compiled **MY** Top 3 considerations for using Azure PowerShell in Azure Functions.

*Please note that this post applies to LATE MAY 2017 - Everything is subject to change with new releases of the product!*

<!--more-->

First off, before we get started, I want to thank <a href="https://twitter.com/ling_toh" target="_blank">Ling Toh</a> - a Software Engineer working on Azure Web Apps and Azure Functions at Microsoft for all the great feedback all over GitHub, pretty much all these tips came directly from Ling's feedback!


### 1. **DO NOT rely on the "Default" Azure PowerShell Modules - ensure you import your needed Modules in your functions** ###
- One of my favorite features of Azure Functions PowerShell was the fact that we have many modules available to us by default. You can even easily see what <a href="https://blogs.msdn.microsoft.com/powershell/2017/02/24/using-powershell-modules-in-azure-functions/" target="_blank">modules are available to you in the PowerShell profile by default</a>, *However*, even if you see the modules are available, I would strongly advise you **NOT** to rely on this information. I have had occurrences where the modules that are available/loaded DIFFER based on the VM that is being used be your Azure Functions. In my case, after turning off my Function App for an hour during a maintenance window, the Azure Modules **WERE NOT** loaded at the time of Execution when I turned by Function App Back on. I have also had this happen for hours at a time seemingly at random. This has caused many headaches in our production environments. Luckily, there is a viable work-around - If you plan on using the Azure PowerShell modules, ensure you explicitly import them in your function, in my example:
     
    ```powershell
    Import-Module "D:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ResourceManager\AzureResourceManager\AzureRM.Profile\AzureRM.Profile.psd1" -Global;

    Import-Module "D:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ResourceManager\AzureResourceManager\AzureRM.Automation\AzureRM.Automation.psd1" -Global;
    ```
    
     
### 2. **Take the time to verify resources installed on the instance and check which / what version of Modules are Installed** ###
- This is another great use of the Kudu console, normally you would use the debug console to browse the files/folders specific to your Function App, but it can also be used to see what resources are available on the Azure Function VM - in this case, the Azure SDK.
    
1. Navigate to the Azure Functions Portal UI.
2. Click on your Function App and select Platform features -> Advanced tools(Kudu)
3. You should see a new browser tab that shows you the Kudu Diagnostic Console. Click on Debug console->PowerShell
4. On the command prompt, type, cd "D:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell" . You should now be able to navigate through all the child folders for the Azure PowerShell module.
    
    ![]({{ site.baseurl }}/assets/img/PowerShell_Kudu_BrowseToAzureSDK.png)

    -Note: If you need a specific version of the Azure PowerShell Modules, you can certainly upload them yourself to your Function Apps WWWROOT folder.
        

### 3. **Ensure you do not require PowerShell Version 5 features!** ###
   - On the Azure Functions issue list, it is claimed that the PowerShell version 5 Installation does not work on the Azure Function VMs, therefore PowerShell Azure Functions for the time being, will only be PowerShell V4, ensure your scripts do not need/utilize version 5 Specific features!



PowerShell Azure Functions certainly have their uses, and for IT Pros out there with "Code-Phobia" it is a tremendous tool to have in the toolbox, however, please consider the above when making your choice of language for your Azure Functions.


Extra Content for Google Search: Some of the direct error messages in my PowerShell Azure Functions were the following. The steps above resolved errors like these, that can occur seemingly at random, and would normally require a restart of the Function App to resolve:

`
Login-AzureRmAccount : The 'Login-AzureRmAccount' command was found in the module 'AzureRM.Profile', but the module could not be loaded. For more information, run 'Import-Module AzureRM.Profile'.
`

`
System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.ArgumentOutOfRangeException: Index was out of range. Must be non-negative and less than the size of the collection.
`


