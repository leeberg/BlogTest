---
layout: post
title: 2020 Side Projects for Fun
date: 2020-10-20
tags: ["Career","Fun","Projects"]
excerpt_separator: <!--more-->
---

Recently, my day job has kept me extremely busy writing blogs, producing videos, and fighting fires on nights and weekends, so my social media posting and blogging frequency basically dropped down to nothing. However, I have been working on plenty of side projects as well as continuing to do presentations and working with user groups. I figured I would quickly write up a simple highlight real for some of the things I have been cooking up in 2020.

![]({{ site.baseurl }}/assets/img/side_projects.png)

Here's a small selection of some of the projects I have worked on this year.

* [BlueStepper](https://github.com/leeberg/BlueStepper)
* [Chicago PowerShell Conference](https://www.youtube.com/channel/UC68iGEhLlXSPe_0NxfxgvxQ/videos)
* [CashCat Ransomware Simulator](https://github.com/leeberg/CashCatRansomwareSimulator)
* [Universal Dashboard / PowerShell Universal](https://ironmansoftware.com/powershell-universal-dashboard)
* [UDMSFS](https://github.com/leeberg/UDMSFS)
* [BlueHive]()

[View Details]({{ site.baseurl }}/2020/10/RecentProjects)


<!--more-->


# BlueStepper

Recently, I have begun to connect more with my interests in producing music. Given how this is a costly hobby AND not wanting to invest a lot of money on hardware, I decided to write my own 16 Step MIDI Sequencer in PowerShell.

The result was "**BlueStepper**"
[![](https://raw.githubusercontent.com/leeberg/BlueStepper/master/img/bluestepper.png)](https://github.com/leeberg/BlueStepper)

**Video Demo:**

[![](http://img.youtube.com/vi/hf5Uzw1xWl0/0.jpg)](https://www.youtube.com/watch?v=hf5Uzw1xWl0)

It worked pretty well, but sadly I discovered that PowerShell and Windows is not exactly all the accurate when 1ms matters, and using the default sleeps/waits available... I ran into a lot of problems with timings going haywire which lead to terrible sounding music.

I barely touched it at all for months. Still, within the last week, I thought of a new way to approach the problem and discovered [https://docs.microsoft.com/en-us/windows/win32/multimedia/multimedia-timers](MultiMedia Timers). Now, after a rewrite, it works GREAT.

I'll be demonstrating some of this module's new capabilities soon - very excited to build some more features into this module and start producing some actual music.

# Chicago PowerShell Conference

In late July, I hosted the Twitch Stream for the Chicago PowerShell Conference. This was a great one day PowerShell event with some fantastic  PowerShell subject matter experts. The stream peaked at over 800 live viewers with basically NO technical difficulties throughout all 10 hours.

![]({{ site.baseurl }}/assets/img/CPUGAdvert.png)

The Chicago PowerShell User Group team did a great job planning and organizing this conference, and I was happy to get the opportunity to run the Twitch Stream. I also built the art assets, scenes, and transitions using a variety of tools. I think this really helped give the conference a great professional look! Through this process, I discovered [Canva](https://www.canva.com/), which I highly recommend to anyone looking at adding art/design to their video or presentation content.

In addition to running the Twitch stream, I also presented a session on: [Universal Dashboard](https://docs.universaldashboard.io/).

I have meant to write up a short guide or an e-book on how to technically run a live stream production from the Audio/Streaming side of things. Unfortunately, I have not yet got around to it; hopefully soon as I would like to help others looking to run remote events in the times of social distancing.

One of my favorite moments was chatting with Jeffery Snover live on the stream. What a rush!

[Check out the recordings from the conference here](https://www.youtube.com/channel/UC68iGEhLlXSPe_0NxfxgvxQ/videos)

# CashCat Ransomware Simulator

In my day job, I needed a simple demo application that replicated some of the "cryptolocking" behavior of ransomware applications. Previously we have to use PowerShell and batch scripts to facilitate rename/encryption operation of files. I wanted to put my own flair on this, and thus [Cashcat Ransomware Simulator](https://github.com/leeberg/CashCatRansomwareSimulator) was born

![demo of cashcat](https://github.com/leeberg/CashCatRansomwareSimulator/raw/master/img/cashcatdemo132.gif)

It's a silly little app, but if you happen to be testing a real time threat detection product based on file system activity or an endpoint solution, it might come in handy!

It has been rewarding to see some of my colleagues and customers utilize this utility.

# Universal Dashboard Projects / PowerShell Universal Projects

One of my new favorites things on the internet! - You can find me producing some video content and submitting open-source code contributions for Universal Dashboard and PowerShell Universal in my free time. Taking a few lines of PowerShell and building interactive tooling and experiences is something I really have a lot of fun with.

Here's a few notable projects I have been working on recently utilizing these products.

### UDMSFS

I'm a big fan of the recent release of [Microsoft Flight Simulator](https://www.xbox.com/en-US/games/microsoft-flight-simulator) and wanted to build a dashboard. Sometimes, especially on longer hauls, flight simulators can get a bit boring... so, what better way to spend the time than to build some dashboards in-flight. 😁

![]({{ site.baseurl }}/assets/img/udmsfs.png)

The dashboard is capable of displaying a map of where the aircraft is located, various statistics such as airspeed and direction, and even has controls for enabling autopilot.

I even made a video demo of the project here:

[![](http://img.youtube.com/vi/BbFmyanPcJ4/0.jpg)](https://www.youtube.com/watch?v=BbFmyanPcJ4)

This project was gratifying as I brushed up on some basic Python and brought a technical development aspect to a video game series I enjoy. 


### BlueHive - In dire need of an update

Yet another proof of concept dashboard! In my home lab, I had the need to do some research on the concept of "Honey Tokens" and "Honey Pot User Accounts" - to facilitate this, I decided to write a tool to be able to deploy and manage Honey Token users, the result was "BlueHive."

![bluehive logo](https://github.com/leeberg/BlueHive/raw/master/img/bluehive.png)

**Homepage**

![bluehive homepage](https://github.com/leeberg/BlueHive/raw/master/img/screenshot1.png)

**User Management**

![bluehive management](https://github.com/leeberg/BlueHive/raw/master/img/screenshot3.png)

**Creating New Honey Token Users**

![demo of cashcat](https://github.com/leeberg/BlueHive/raw/master/img/hivedeployment.gif)

BlueHive is in dire need of some updates - I am really letting my 124 Github stars down!