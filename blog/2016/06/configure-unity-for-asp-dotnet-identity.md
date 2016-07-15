Configure the Unity Container with a Visual Studio project template that uses ASP.NET identity
==============================================================================================
**Author**: Alan Mills  
**Date**: [2 June 2016 18:11](/blog/history/2016-06.md)
**Tags**: [Visual Studio 2015](blog/categories/visual-studio-2015.md), [ASP.NET MVC 5](blog/categories/asp-net-mvc-5.md), [Unity Container](blog/categories/unity-container.md)
**Status**: Draft

When converting a Visual Studio project created with a template that uses the ASP.NET Identity to use the Unity Container.  You will need to configure Unity otherwise things will not work at runtime.

Examples of projects that have this issue are the *ASP.NET Web Application (.NET Framework)* Project *ASP.NET 4.6.1 Templates*:
* Web Forms
* MVC
* Web API
* Single Page Application

**NOTE:  Need to validate that all of these templates work**

You will need to install the ASP.NET MVC Unity Container NuGet package `Install-Package Unity.Mvc`. You need to register a number of types with Unity otherwise when you attempt to use any of the ASP.NET identity functionality you will be presented the ASP.NET yellow page of death with one of the error messages:

> The current type, Microsoft.AspNet.Identity.IUserStore 1 ` [WebApplication1.ApplicationUser], is an interface and cannot be constructed. Are you missing a type mapping?

``` CSharp
container.RegisterType<IUserStore<ApplicationUser>, UserStore<ApplicationUser>>();
```

> The current type, System.Data.Common.DbConnection, is an abstract class and cannot be constructed.  Are you missing a type mapping?

``` CSharp
container.RegisterType<DbContext, ApplicationDbContext>(new HierarchicalLifetimeManager());
```

> The current type, Microsoft.Owin.Security.IAuthenticationManager, is an interface and cannot be constructed. Are you missing a type mapping?

``` CSharp
container.RegisterType<IAuthenticationManager>(new InjectionFactory(o => System.Web.HttpContext.Current.GetOwinContext().Authentication));
```

Putting it all together
-----------------------
The easiest way to add these to the `UnityConfig` class in the the `App_Start\UnityConfig.cs` file.

``` CSharp
using System;
using Microsoft.Practices.Unity;

namespace WebApplication1.App_Start
{
  public class UnityConfig
  {
    #region Unity container

    public static void RegisterTypes(IUnityContainer container)
    {
      container.RegisterType<IUserStore<ApplicationUser>, UserStore<ApplicationUser>>();
      container.RegisterType<DbContext, ApplicationDbContext>(new HierarchicalLifetimeManager());
      container.RegisterType<IAuthenticationManager>(new InjectionFactory(o => System.Web.HttpContext.Current.GetOwinContext().Authentication));      
    }
  }
}
```


**NOTE need to validate these too**
``` CSharp
container.RegisterType<UserManager<ApplicationUser>>(new HierarchicalLifetimeManager());

container.RegisterType<AccountController>(new InjectionConstructor());
```
