---
layout: post
title: Monitoring Azure Functions with Application Insights (Early 2017
date: 2017-04-22
tags: ["Productivity","Tools"]
excerpt_separator: <!--more-->
---

# Monitoring Azure Functions with Application Insights (Early 2017)

Application Insights is becoming a fantastic tool for administrators of Azure Functions. Some new preview features relating to Azure Function and Application Insights Integration have been released, but we still actually have other great ways to get our Azure Functions logging into Application Insights. Another Great benefit of these methods is that Application Insights can be easily integrated with OMS giving you massive visilibilty into your Azure Function Operations.
 
 
 In this post, we will review the capabilties of the Official Preivew for Azure Function Monitoring with Application Insights, AND building our OWN method to utilize Application Inisghts in our Azure Functions (and even our own PowerShell Scripts).
 
 
<!--more-->



Documentation has been a bit limited so I wanted to put together a post to function as a quickstart / getting started guide to using Azure Functions with Application Insights and OMS








### Application Inisghts for Azure Functrions

* EAsy to Setup
* Good Option for monitoring running status of your functions
* Limited as to what you can Log


### Using Applicationg Insights Dlls with our Azure Functrions

* More Difficult to Setup
* Requires you write your onw logging into Azure Functions
* Ability to log... pretty much ANYTHING into Appliction Insights




### Resources:
[Repo for Example](https://github.com/christopheranderson/azure-functions-app-insights-sample)

[Application Insights API Reference](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics)

[Application Insights Connector in OMS](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)



