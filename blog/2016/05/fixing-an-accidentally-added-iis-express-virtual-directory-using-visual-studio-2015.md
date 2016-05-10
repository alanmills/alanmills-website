Fixing an accidentally added IIS Express virtual directory using Visual Studio 2015
===================================================================================
**Author**: Alan Mills  
**Date**: [10 May 2016 09:38](/blog/history/2016-05.md)
**Tags**: [Visual Studio 2015](blog/categories/visual-studio-2015.md), [IIS Express](blog/categories/iis-express.md), [Windows 7](blog/categories/windows-7.md), [ASP.NET MVC 5](blog/categories/asp-net-mvc-5.md), [ASP.NET Web API 2](blog/categories/asp-net-web-api-2.md)
**Status**: Publish

While editing the Project Properties of an [ASP.NET Web API 2](blog/categories/asp-net-web-api-2.md) application, in a moment of madness I changed the Project Url to include the path to my controller and instructed [Visual Studio 2015](blog/categories/visual-studio-2015.md) to create a new Virtual Directory.  Of course after this the controller was no longer getting the REST calls and I had no idea how to fix this...

After inspecting my ```git diff``` history, I discovered that Visual Studio had added the following configuration snippet to The IIS Express solution settings file ```<<solution path>>\.vs\config\applicationhost.config```.  In the example below *<<solution path>>* is *C:\mycode\mysolution\*

``` xml
<sites>
  <site name="MyWebApiProject" id="2">
    <application path="/" applicationPool="Clr4IntegratedAppPool">
      <virtualDirectory path="/" physicalPath="C:\mycode\mysolution\myproject" />
      <virtualDirectory path="/api" physicalPath="C:\mycode\mysolution\myproject" />
    </application>
    <application path="/api/controller" applicationPool="Clr4IntegratedAppPool">
      <virtualDirectory path="/" physicalPath="C:\mycode\mysolution\myproject" />
    </application>
    <bindings>...</bindings>
</sites>
```
To clean this up, remove the additional items for the *api* and *controller* leaving the file in the state

``` xml
<sites>
  <site name="MyWebApiProject" id="2">
    <application path="/" applicationPool="Clr4IntegratedAppPool">
      <virtualDirectory path="/" physicalPath="C:\mycode\mysolution\myproject" />
    </application>
    <bindings>...</bindings>
</sites>
```
