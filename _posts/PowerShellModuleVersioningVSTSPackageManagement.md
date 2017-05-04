---
layout: post
title: PowerShell Versioning with VSTS Package Management
date: 2017-04-22
tags: ["Productivity","Tools"]
excerpt_separator: <!--more-->
---

Using the new "Package Management" feature of VSTS we can build a robust, and easy to use *PowerShell Module* Build & Release workflow that has many great features such as Version Control, NuGet Integration and much more!

There are some great functionality right out of the box - so let's look at a scenario.



### Getting STarted


1.  Active the Package Management Features on VSTS
  - [Micosoft VSTS Package Management Docs](https://www.visualstudio.com/en-us/docs/package/overview)
  - IMG1
  - IMG2
  - IMG3
2.  Create a New Nuget Feed
- [Package Management "Create a Feed" Docs](https://www.visualstudio.com/en-us/docs/package/feeds/create-feed)
  - IMG1
  - IMG2
  - IMG3
3.  Create a Build Defintion with NuGet Activites
- [Micosoft VSTS Package Management Docs](https://www.visualstudio.com/en-us/docs/package/overview)
  - IMG1
  - IMG2
  - IMG3




### General Workflow:



The General Workflow goes like this:


PowerShell Versioning with VSTS Package Management




### Process:

1. Developer Creates Standard Module Structure
	MyModule (REPO NAME - MUST match module name)

2. Developer Authors Module
	.psm1
	.psd1

3. Developer Authors BASIC NuGet .nuspec file
	build.nuspec

4. Developer Checks IN  Module Code to Master Branch

5. Developer Creates BUILD Defintion for Module - Standard VSTS PowerShell Build  
* NOTE this process is "Self Reference / Variablized" SO THE ONLY Adjustements to the VSTS BUILD is to update the "Get Sources" to the appropriate Repo.


6. Developer Runs Build
	This creates a build artifcat and NuGet Packages

7. Developer Creates Release DEF for PROJECT
	Developer will select NuGet Modules and appropriate Module Versions

8. Build Manager (Or whever) RUNS the Release





