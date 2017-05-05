---
layout: post
title: Monitoring Azure Functions with Application Insights & OMS
date: 2017-05-04
tags: ["Azure Functions","Application Insights","OMS"]
excerpt_separator: <!--more-->
---
  

[Azure Functions](https://azure.microsoft.com/en-us/services/functions/) is a great solution for building cloud-based "serverless" functions that run small and effiecient pieces of code to fill a wide array of automation / develpoment tasks. Right now, one of the biggest asks from my customers are ways to efficiently maintain, monitor, and manage their Azure Functions... [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is a great tool that fits this purpose. Recently, new preview features relating to Azure Function and Application Insights Integration have been released as a public preview. This solution is extremely promising and useful even in it's preview state! 

We can also take a "build-it-yourself" route to monitor Azure Functions with Application Insights.

I wanted to write a post quickly summarizing some options available to monitor Azure Functions utilizing Application Insights. There are a good deal of Wikis, Github Repos, and Documents coming out on the subject but I wanted to summarize my findings (as of MAY 2017) in an actual blog post to help out others looking for a solution.

Before we get started... you might have noticed I mention Operations Management Suite "OMS" in the title of this blog post, well, not much to say here other than the fact that it is very easy to pull our Application Insights data into OMS, but more on that later...


<!--more-->


# Using the Official Azure Functions Integration with Application Insights - Preview ## 
[Official Announcement](https://blogs.msdn.microsoft.com/appserviceteam/2017/04/06/azure-functions-application-insights/)

In April 2017, Microsoft released a preview of direct integration between Azure Functions and Application Insights. This solution involves having an existing Application Insights Workspace and adding a few settings to your Function App. Once completed, many different sets of data will automatically start transferring from your Azure Functions into your Application Insights Workspace.
  
> Now it takes (nearly) zero effort to add Application Insights to your Azure Functions and immediately unlock a powerful tool for monitoring your applications

*However*, please know that there is a huge caveat here, this solution is currently in PREVIEW and subject to breaking changes!

[From the Microsoft Azure Functions Wiki](https://github.com/Azure/Azure-Functions/wiki/App-Insights-(Preview))**:**
>⚠️ 🚧 This is a preview feature and may have breaking changes before we GA it. Use this in production with caution. We don't expect any API breaking changes (maybe data changes), but we are not yet 100% sure. 🚧 ⚠️

Keep this mind, and be prepared for changes...that being said, once you enable this functionality you will get a TON of useful data into Application Insights.

Here's what you get:

**Live Stream** - Live "Stream" around performance, # of requests, etc. for your functions 

   Example Live Stream: (Only 1 running Azure Function...)
   ![]({{ site.baseurl }}/assets/img/Live_Metrics_Stream_Microsoft_Azure.png)

**Analytics of Event Data**
   - **Event Data Types:**
   - **Execution** - Log for each function execution
   - **Exception** - Log Data for Exception Events - GREAT for Debugging as you can really dig into the Exception Object itself, NOT just a text string noting that "Something Broke"!
   - **Trace** - Trace is really anything written to the context.log - If we are thinking PowerShell you can think of this as a "Write-Host / Write-Output" - This is typicall a string you will use for Debug / Logging Purposes 
   - **Custom Events & Custom Metrics** - There are a number of custom Application Insights objects you can build and deliver to your workspace if you have scenarios not covered in the above options... [Take a look](https://social.technet.microsoft.com/wiki/contents/articles/31192.custom-telemetry-events-with-trackevent-in-microsoft-application-insights.aspx)
   
   Analyzing a single Trace Event:
   ![]({{ site.baseurl }}/assets/img/AppInSightsTraceEvent.png)  
   
     
     
   
 **What can I do with all this data this?** 
 
Put simply, Application Insights is *PACKED* with features, you can really tell it was built from the ground up BY developers, FOR Developers. Besides having a number of dashboards, analytic engines, and other ways to VIEW the data, we can even trigger alerts and events based on the data. This enables some interesting scenarios for maintaining and monitoring your Azure Functions. 

![]({{ site.baseurl }}/assets/img/ApplicaitonInsights_Microsoft_Azure_LOTSOFSTUFF.png)

That about wraps it up for the Official Preview of Azure Functions with Application Insights. Even though it is just a preview there are a ton of great features for just a few seconds of configuration!  




# Build it Ourselves! : Using the Application Insights DLLs to get Application Insights Methods in your Azure Functions! #

One of the great features of Azure Functions is that we can utilize our own packages or NuGet to add the functionality of external libraries to our functions. In the following examples, we are using the [Application Insights Core Telemetry API](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics) to send custom events, metrics, and telemetry to application insights.
 
I would actually suggest this method if you are just getting started, don't need all the bells & whistles of the official solution, OR most importantly, don't want to base your monitoring and management of your Azure Function around a preview feature (May 2017) that could go away / have breaking changes...


#####C# Functions - Using NuGet to "Get" and load the Application Insights API
I want to give full credit to this method to:[Christopher Anderson](https://github.com/christopheranderson) a PM for Azure at Microsoft, he even has a [github repo](https://github.com/christopheranderson/azure-functions-app-insights-sample) with great examples and documentation of this method to get you started!


1. **Create a project.json file in your function file and include the Microsoft.ApplicationInsights Package, your function with go and grab nuget packages on compilation:**
    ```xml
    {
      "frameworks": {
        "net46":{
          "dependencies": {
            "Microsoft.ApplicationInsights": "2.1.0"
          }
        }
       }
    }  
    ```  
2. **Add the following references in your function:**
 ```cs
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;
   
    private static TelemetryClient telemetry = new TelemetryClient();
    private static string key = TelemetryConfiguration.Active.InstrumentationKey = System.Environment.GetEnvironmentVariable("%YOUR_FUNCTION_APP_VARIABLE_CONTAINING_YOUR_APP_INSIGHTS_KEY%", EnvironmentVariableTarget.Process);
   ```
3. **Start using the [Application Insights Core Telemetry API](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics) in your Functions!**      
    
  ![]({{ site.baseurl }}/assets/img/AppInsightsCsharrplogging.png)
  
  
  
  

#####PowerShell Functions:
You wouldn't think I would do a blog post and script PowerShell functions right???

This process is very simliar to the above example, but in this case I am going to UPLOAD the Application Insights to my Function App and "Import" the functionality of the Application Insights DLL into to my function.

1. **Obtain the Application Insights .DLL**
    - Easy to get from Nuget / Visual Studio  

2. **Upload the .DLL to your Function App / Azure Function**
    -  ![]({{ site.baseurl }}/assets/img/Microsoft.ApplicationInsights.dll_wwwroot_App_Service_Editor.png)

3. **Load the Application Insights DLL and setup a telemetry client object:**
    -  ![]({{ site.baseurl }}/assets/img/AppInsightsPOSHlogging.png)    

4: **Use the Application Insights API Reference in your scripts to log directly to Application Insights**

**BONUS** - This method also works great for your traditional on-prem scripts :) 



### One Last (Awesome) Thing... OMS! 
So having all this data in Application Insights is Great! We have lots of great ways to view / parse many different data types from our Azure Functions, and we even setup some alerting functionality, great! 

Well, with literally a few clicks - we can also bring all this Application Insights data into our Operations Management Suite (OMS) using the [Applications Insights Connector](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) 

This will allow you to fire off emails, runbooks, alerts, and all sorts of methods based on your Azure Function Data. 
 

