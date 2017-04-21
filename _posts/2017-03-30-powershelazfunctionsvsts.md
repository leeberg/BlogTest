---
layout: post
title: Deploying PowerShell Azure Functions with Visual Studio Team Services (Simple)
date: 2017-03-30
tags: ["Azure","DevOps","Azure Functions","VSTS"]
excerpt_separator: <!--more-->
---

Many of my customers have started utilizing the new capabilities of Visual Studio Online: Visual Studio Team Services  to manage and deploy production Azure Resources. I have been recently tasked with deploying and maintaining PowerShell based Azure Functions in such an environment.

After some research on integrating Azure Functions with VSTS (and having a limited background in Azure App Services)  I wasn't finding any short and simple explanations/documentation on this process. I needed a mature automated (One Click) way to deploy Azure Functions from a VSTS Code Repository. I did not / could not, by project requirement resort to using "custom code" / manual processes, so I wanted to outline the steps I took to help speed up the process for those new to Azure Functions.

This example will take a handful of files and folders in a VSO:VSTS Git Repository and deploy an entire App Service / Function App with 3 Azure Functions using Out Of Box Build Activities. I am NOT focusing on the STEP BY STEP process, this merely functions as a solution example. There are MANY ways to accomplish this task, but I consider this a fully-functional and easy implementation of this process.

<!--more-->

# **Source Files**

The source files for my project are very simple, and was created mostly by hand in Visual Studio Code.

![]({{ site.baseurl }}/assets/img/01.png)


<span style="text-decoration: underline;">**Contents:**</span>
**Folder: Build** - Contains the ARM Template and ARM Template Parameter JSON files. This is what we we use in our Build & Release process to deploy the Function App Itself.

**Folder: wwwroot** - This folder will contain the physical folders and files that will be transferred to the file store of the Function App (Modules, Keys, Resources, Function Folders).

One of my Favorite features of Azure Functions is the fact the Function App will "create" a function automatically, simply by placing a a folder containing a function.json and run.ps1 (in the case of powershell) file.  In this example by the folders (Function1, Function2, and Function3) will become our actual Azure Functions.

WWWROOT Folder Contents:

![]({{ site.baseurl }}/assets/img/02-1.png)


All of the source files outline aboved are currently maintained in a Visual Studio Online VSTS GIT repository:

![]({{ site.baseurl }}/assets/img/03.png)

In this example, I will setup a New Build that, for the sake of example will complete all Build and Release Activities.

This will take our handful of files and folders and deploy an entire the App Service / Function App with 3 Azure Functions.

&nbsp;

# **Let's Build!**

Here I have created a new Build Definition in my repository containing the Function App ARM Template and Azure Function source.


![]({{ site.baseurl }}/assets/img/3.png)


This represents a **SIMPLE / QUICK / EASY solution** your mileage may vary and of course there are MANY ways to complete this tasks. If you search online you will find a wide variety of solutions, but this one relies on "out of box" VSTS capabilities and minimal custom code.

The general process of deploying our Function App and Azure Functions is as follows:

**STEP 1 - Azure Deployment
****STEP 2 - Archive the wwwroot Directory into a Zip File
STEP 3 - Use the "Azure App Service Deployment" Activity to deploy the ZIP file**

**
**

![]({{ site.baseurl }}/assets/img/4.png)




**STEP 1 Azure Deployment Details:**

![]({{ site.baseurl }}/assets/img/5.png)



Our first step utilizes the "Azure Deployment" VSTS build step activity. This activity will allow you to specify a Subscription and an Azure Resource Manager Template to Create OR Update your Azure App Service / Function App.

In my example, I have slightly modified one of the existing "Azure Quickstart Templates" setup to deploy a dynamic consumption hosted Azure App Service / Function App.

**You can find this template here:**
[https://azure.microsoft.com/en-us/resources/templates/101-function-app-create-dynamic/](https://azure.microsoft.com/en-us/resources/templates/101-function-app-create-dynamic/)

As with all ARM Deployments, I suggest you variablize as much as you can as this will allow you to easily re-use the ARM template in the future as well as enable a number of dynamic deployment scenarios for Azure Functions.


![]({{ site.baseurl }}/assets/img/6.png)

**STEP 2 - Archive the wwwroot Directory into a Zip File**

Step 2 is the most simple as we are simple creating a compressed ZIP archived file from entire contents (files and folders) of our wwwroot folder. The reason we are doing this is because the Step 3 build activity is designed to "ingest" a zip file.


![]({{ site.baseurl }}/assets/img/7.png)

**STEP3 - Use the "Azure App Service Deployment" Activity to deploy the ZIP file
**This is the final step where the magic really happens. In this step we are using the "Azure App Service Deployment" Activity. This is an activity specifically designed to send files / configurations to an Azure App Service. Since our Azure Functions / Function App ARE/ARE built on top of the Azure App Service this will work perfectly for us.


![]({{ site.baseurl }}/assets/img/8.png)


We specify the Azure Subscription, and App Service Name, and the location of the ZIP archive we created in step 2.

&nbsp;

# **Let's Release!**

Now that we have built out our Build/Release let's run this thing!

![]({{ site.baseurl }}/assets/img/9.png)

Once your first build out your solution, you may have to go back and tweak your variables and syntax before you have a perfect build, but in the steps outlined above, you can go from having NO App Services / Functions Apps / Azure Functions, to having a fully functional Function App with Azure Functions.

If you specify the "disabled" property to be false in your function.json file, your Azure Function will be Live Immediately.


![]({{ site.baseurl }}/assets/img/10.png)

Here we see the "Deployed" Azure Functions:


![]({{ site.baseurl }}/assets/img/11.png)


Here we see our familiar files from our VSTS repository, except now they live in our Azure App Service / Function app wwwroot directory:


![]({{ site.baseurl }}/assets/img/12.png)


Any Future updates to your functions can then be maintained in your code repo, and future updates will redeploy your functions.

&nbsp;

&nbsp;