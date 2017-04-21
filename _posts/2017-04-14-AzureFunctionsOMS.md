---
layout: post
title: "Azure Functions - OMS: Data Retrieval and Injection"
date: 2017-02-05
tags: ["OMS","Azure","Azure Functions"]
excerpt_separator: <!--more-->
---

Recently I have been developing a number of "Hobby" IoT Solutions, and for me a quick start to this development was to use OMS as a Temporary Storage and Alerting Mechanism. Through this process (and suggestion by the great Tao Yang) I have taken the time to experiment with Azure functions to Retrieve Data from OMS, and to Inject Data into OMS.

For Readers, please know that following blog posts on Azure Functions from [THE GREATS ](https://memecrunch.com/meme/BEAOT/you-da-real-mvp/image.jpg?w=992&c=1)are required reading for this topic, and will get you up to speed with Azure Functions (Especially PowerShell + Azure Functions)

**[David O'Brien](https://david-obrien.net)**

[**Stefan Stranger**](https://blogs.technet.microsoft.com/stefan_stranger/2017/01/29/powershell-azure-functions-lessons-learned/)

[**Tao Yang**](http://blog.tyang.org/?s=azure+function)


This blog post here skips some of the details and instead focuses on implementation and some of the results, this post mainly serves as an example of how awesome Azure Functions are and to highlight some of their capabilities.

[Github Repo for Example Script](https://github.com/leeberg/AzureFunctionsOMS)

<!--more-->

## **Data Injection**
⋅⋅* Unordered sub-list.
⋅⋅* Unordered sub-list.
⋅⋅* Unordered sub-list.
This process is actually the most straight forward, as the process to Inject data into OMS is fairly well documented. In this case I decided opt NOT to use PowerShell and instead use C#.

I put this together before I even realized that Azure Functions could run natively as  PowerShell - so if you are not comfortable with C#, Powershell is a viable option..

![]({{ site.baseurl }}/assets/img/overview.png)

The process of injecting data into OMS relies on the "OMS Data Collector API" which is well documented and has many code examples:  [Microsoft Documentation on OMS Data Collector API](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-data-collector-api). You can literally copy / paste some of the examples from the official documentation and have a working model up in minutes

I wanted to be able to send data from a wide array of sources and in a variety of string formats, so in my example, I am using Newtonsoft.JSON to process/parse some of my incoming data before sending it to OMS.

Remember, because our workspace keys are stored in our Azure Function, our client app just needs to call the Azure Function hook url with credentials(or anonymously) to run the function.

<script src="https://gist.github.com/leeberg/fb49d6c2aff88ab43b741711b70427e6.js"></script>


![]({{ site.baseurl }}/assets/img/inject1.png)

![]({{ site.baseurl }}/assets/img/Inject2.png)

In my use case, I have a Raspberry Pi with a number of environmental / Bluetooth LE sensors that is running a simple Python Script to push the data to my Azure Function to be processed into OMS.

![]({{ site.baseurl }}/assets/img/python.png)

&nbsp;

## **Data Retrieval**

For this process I wanted easily have a method to query OMS on the fly using an Azure Function. This is a bit trickier as when try and do research on the OMS Search API are really no clear examples of doing this in C# - examples provided utilize the ARMClient command line tool. I didn't want to have to rely on this so my first idea was to take advantage of some of the great work already done by [Stanislav Zhelyazkov](https://cloudadministrator.wordpress.com/) in his awesome [OMSSearch Module](https://github.com/slavizh/OMSSearch) by uploading the module into an Azure Function.

However, PowerShell Azure Functions have many modules setup natively in the profile, and we just so happen to have access to the "AzureRM" cmdlets, and specifically: "AzureRM.OperationalInsights" module in our function.   This means we can easily handle a connection to our Subscription and Retrieve data from OMS natively in our Azure Function.

In my example I am using **Get-AzureRmOperationalInsightsSearchResults** and **Get-AzureRmOperationalInsights_Saved_SearchResults** to fulfill our goal.

<script src="https://gist.github.com/leeberg/da15410629f1f84b8d163f1ba125312b.js"></script>

&nbsp;

![]({{ site.baseurl }}/assets/img/COde.png)

&nbsp;

**Azure Function Logs:**

![]({{ site.baseurl }}/assets/img/Function-Logs.png)



**The results in OMS:**

![]({{ site.baseurl }}/assets/img/OMS-Search.png)


&nbsp;

Using Azure Functions, you can quickly and easily get just about anything INTO or OUT OF OMS securely and effectively.


&nbsp;

&nbsp;