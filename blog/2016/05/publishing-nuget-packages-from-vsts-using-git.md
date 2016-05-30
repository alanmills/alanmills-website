Publishing NuGet packages from Visual Studio Team Services using Git
====================================================================
**Author:** Alan Mills  
**Date:** [27 May 2016 16:16](/blog/history/2016-04.md)  
**Tags:** [Visual Studio Team Services](/blog/categories/visual-studio-team-services.md), [NuGet](/blog/categories/nuget.md),
[Git](/blog/categories/git.md)  
**Status**: Draft

**NOTE: This needs to be split in to multiple parts and brought together in this post**

Overview
--------
In this blog post, I am going to walk through the process of setting up a NuGet package in Visual Studio Team Services, published from one Visual Studio solutions and consumed in two others:
1. ASP.NET MVC Solution: MVC application that displays data from a WebAPI REST API,
2. WebAPI Solution: Provide a REST API for the MVC solution,
3. Data Contracts Solution: Our NuGet package, used to share data type definitions between the MVC and WebAPI solutions.

The steps to build this solution:
1. Pre-requisites
2. Setup Visual Studio Team Services
3. Setup Visual Studio Team Services Project
4. Setup Visual Studio Team Services Project repositories
5. Create Data Contracts Solution
6. Publish Data Contracts NuGet Feed from Visual Studio Team Services
7. Consume Data Contracts NuGet Feed in Visual Studio
8. Setup WebAPI Solution
9. Setup MVC Solution
10. Deploying to Azure


### Pre-requisites
* VSTS subscription
* VSTS Administrator rights

### Setup Visual Studio Team Services
1. Install VSTS GitVersion and Package Management extensions

### Setup Visual Studio Team Services Project

### Setup Visual Studio Team Services Project repositories
1. Setup Git repository

### Create Data Contracts Solution
1. Create initial solution
2. Create Work Item and associated feature branch
3. Tag the Release

#### Tag the Release
``` bash
$ git tag -a v0.1.0 -m "v0.1.0 Feature/1-some-sort-of-feature"
```

### Publish Data Contracts NuGet Feed from Visual Studio Team Services
1. [Create build process](create-build-process)
2. [Create package feed](create-package-feed)
2. [Create release process](create-release-process)

#### Create build process
**Create new build process in VSTS**
1. Navigate to the *BUILD* section of Visual Studio Team Services
2. Create a New Build definition
3. Select the *Visual Studio* template
4. Select *contracts* repository, *master* branch and select *Continuous integration*

**Modify *Build* steps**
1. Add *GitVersion* build step and move to the top of the list
2. Configure *GitVersion*, enable *Update AssemblyInfo files* option
3. Add *NuGet Package* build step and place between the *Build solution* and *Copy Files* build steps
4. Configure *NuGet Package*, set *Package Folder* to *Package*.  Ensure *Automatic package versioning* is *Off*
5. Configure *Copy Files* build step, set *Source Folder* to *Package*

**Modify Triggers**
1. Change Filters *branch* from *master* to *Feature/**

**Save Build defintion**
1. Save the build defintion with a meaningful name *Data Contracts CI Build*

#### Create package feed
1. Navigate to the *PACKAGE* section of Visual Studio Team Services
2. Create a *New feed*
3. NuGetDataContracts

#### Create release process
**Create new release definition in VSTS**
1. Create a new release definition,
2. Select the **Empty** release template,
3. Select from the **Create new release definition** dialogue
  1. Artifacts: **Build**
  2. Project: **NuGet**
  3. Source (Build definition): **Data Contracts CI Build**
  4. Continuous deployment: **True**

  **Modify *Environment* tasks**
  1. Add *NuGet Publisher* release task
  2. Configure *NuGet Publisher*
    1. Feed type: **Internal NuGet Feed**
    2. Internal Feed URL: https://alanmills.visualstudio.com/DefaultCollection/_packaging/NuGetDataContracts/nuget/v3/index.json

### Consume Data Contracts NuGet Feed in Visual Studio
### Setup WebAPI Solution
### Setup MVC Solution
### Deploying to Azure
