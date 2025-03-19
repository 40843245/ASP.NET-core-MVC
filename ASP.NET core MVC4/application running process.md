# application running process.
## part 1

When the ASP.NET core MVC4 application runs, it will run the entry point (i.e. invoking `Application_Start` method in `MvcApplication` class (in `Global.asax.cs` file))

### part 1.1
In `Global.asax.cs` file, the basic format of MvcApplication will be look like this:

```
// ... some import statements.

namespace AKM
{
    // 注意: 如需啟用 IIS6 或 IIS7 傳統模式的說明，
    // 請造訪 http://go.microsoft.com/?LinkId=9394801
    public class MvcApplication : System.Web.HttpApplication
    {
        private static Logger _logger = LogManager.GetCurrentClassLogger();

        protected void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();

            WebApiConfig.Register(GlobalConfiguration.Configuration);
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);

            BundleConfig.RegisterBundles(BundleTable.Bundles);
            AutofacConfig.Initialize();
            ViewEngines.Engines.Add(new ViewConfig());
        }

        protected void Application_Error(object sender, EventArgs e)
        {
            string message = "";
            Exception ex = Server.GetLastError();
            message = $"發生錯誤的網頁:{Request.Path + Environment.NewLine}錯誤訊息:{ex.GetBaseException().Message + Environment.NewLine}堆疊內容:{Environment.NewLine + ex.StackTrace}";

            _logger.Error(message);
        }
    }
}
```

![image](https://github.com/user-attachments/assets/19e8b2eb-6a39-4d54-8fb1-7b7e8c321c76)

> [!TIP]
> The `Application_Start` method of `MvcApplication` will be executed at startup of application running.
>
> And `Application_Error` method of `MvcApplication` will be executed iff the it throws an exception when running on server. 

## part 1.2

In `Application_Start` method call, it will invoke `AreaRegistration.RegisterAllAreas();` to registers all areas in an `ASP.NET MVC` application.

> [!NOTE]
> More information about `AreaRegistration.RegisterAllAreas` static method call, see [`AreaRegistration.RegisterAllAreas method (MSDS)`](https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.arearegistration.registerallareas?view=aspnet-mvc-5.2)

## part 1.3

Then it will invoke `WebApiConfig.Register(GlobalConfiguration.Configuration);` (is defined in `WebApiConfig` static class in `..\App_Start\WebApiConfig.cs` file) to configure the route and register it.

You can see how the route is configured in this statement.

```
config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
```

in `WebApiConfig` class.

```
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
        }
    }
```

> [!NOTE]
> By default, the naming convention of url explained in [this article](https://github.com/40843245/ASP.NET-core-MVC/blob/main/naming%20convention/MS%20MVC%20naming%20convention.md#url) is
>
> ```
> http:<ipAddress>/<fileNameExecutedWithoutSuffixController>/<invokedMethod><queries>
> ```
>
> The reason why can be found in the above code snippet.
>
> In the above code snippet, it will invoke `config.Routes.MapHttpRoute` method with `routeTemplate` argument passed as `"api/{controller}/{id}"`
>
> which indicates the url will be look like
>
> ```
> http:<ipAddress>/<fileNameExecutedWithoutSuffixController>/<invokedMethod><queries>
> ```
> 
> You can set how the url will look like here.

> [!NOTE]
> To learn more about routing in ASP.NET application, you can see [Routing in ASP.NET Web API (MSDS)](https://learn.microsoft.com/en-us/aspnet/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api)

### part 1.3

After that, it will executed `FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);` (defined in `GlobalFilter` static class in `..\App_Start\FilterConfig.cs` file) to register the filter globally.

```
    public class FilterConfig
    {
        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new HandleErrorAttribute());
        }
    }
```

> [!NOTE]
> For more information about API,
>
> + [System.Web.Mvc.GlobalFilterCollection class (MSDS)](https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.globalfiltercollection?view=aspnet-mvc-5.2)
>
> + [System.Web.Mvc.HandleErrorAttribute class (MSDS)](https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.handleerrorattribute?view=aspnet-mvc-5.2)
