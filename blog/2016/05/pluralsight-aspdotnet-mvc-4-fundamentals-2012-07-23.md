Review of Pluralsight course: ASP.NET MVC 4 Fundamentals (2012/07/23)
=====================================================================
**Author:** Alan Mills  
**Date:** [19 May 2016 10:42](/blog/history/2016-05.md)  
**Tags:** [Pluralsight](/blog/categories/pluralsight.md), [ASP.NET MVC 4](blog/categories/asp-net-mvc-4.md), [ASP.NET Web API](blog/categories/asp-net-web-api.md)
**Status**: Publish

The Pluralsight course [ASP.NET MVC 4 Fundamentals](https://app.pluralsight.com/library/courses/mvc4/table-of-contents), published on 23 July 2012 is given by [Scott Allen](http://www.odetocode.com).

Overview
--------
Scott Allen gives a good overview of the subjects:
* ASP.NET MVC 4
* Entity Framework Migrations
* Deploying to Azure
* Bundles
* ASP.NET Web API
* Async Await in MVC 4
* MVC 4 and Mobile Development

Given that I have completed this course during May 2016 using Visual Studio 2015 Update 2, some of this material is out-of-date and no longer relevant however, the majority of the course is relevant if you are trying to getting an overview of ASP.NET MVC 4.

Areas of difficulty
-------------------
* [Deploying to Azure](deploying-to-azure)
* [Bundles - CoffeeScript example](bundles---coffeescript-example)
* [Async Await in MVC 4 - Example code does not compile and run](async-await-in-mvc-4---example-code-does-not-compile-and-run)
* [MVC 4 and Mobile Development - Mobile Application project template no longer exists](mvc-4-and-mobile-development---mobile-application-project-template-no-longer-exists)

### Deploying to Azure
Since 2012 there have been a lot of changes to Azure and while the steps outlined in this course are approximately remain the same, if you do not have experience working with Azure then this part of the course will be confusing.  Follow the Azure tutorial [Deploy an ASP.NET web app to Azure App Service, using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).  Once you have completed the tutorial you may find it beneficial to try to deploy the eManager solution to re-inforce the the Azure tutorial however, you do not need to do so to complete the rest of this Pluralsight course.

### Bundles - CoffeeScript example
The NuGet package CoffeeBundler is no longer available and I did not spend a long time trying to resolve this issue as CoffeeScript transcompilation to JavaScript is not my primary objective for this part of the course.

Not completing this part of the course has no negative impact on the rest of this Pluralsight course.

### Async Await in MVC 4 - Example code does not compile and run
The sample code AsyncAwait would not compile for me, on closer inspection this is a simple WCF service that has a fixed delay of 2 seconds and returns a static response.  Therefore, I recommended that you recreate the WCF service using the source code as a reference.

### MVC 4 and Mobile Development - Mobile Application project template no longer exists
In the course the Mobile Application project template is used in the course, this is no longer available in Visual Studio 2015 (Update 2), this is probably due to the change in the MVC template to use Bootstrap.

Bootstrap supports responsive design and this part of the course makes reference to the previous approach of having different views based on the requesting device.  This part of the course is no longer an ideal way to deal with different devices.  To this end I have added the Pluralsight course [Responsive Websites With Bootstrap 3](https://app.pluralsight.com/library/courses/responsive-websites-bootstrap3/table-of-contents) by [Mark Zamoyta](https://www.linkedin.com/in/mark-zamoyta-54b7b338) to my learning backlog.

Recommended courses
-------------------
* [Pluralsight: Getting Started with Entity Framework 6 (2015/11/16)](https://app.pluralsight.com/library/courses/entity-framework-6-getting-started/table-of-contents), [Review](blog/2016/05/pluralsight-getting-started-with-entity-framework-6-2015-11-16.md)
* [Pluralsight: Building and Securing a RESTful API for Multiple Clients in ASP.NET (2015/03/05)](https://app.pluralsight.com/library/courses/building-securing-restful-api-aspdotnet/table-of-contents), [Review](blog/2016/05/pluralsight-building-and-securing-a-restful-api-for-multiple-clients-in-aspdotnet-2015-03-05.md)
