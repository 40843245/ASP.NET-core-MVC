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

### part 1.2

In `Application_Start` method call, it will invoke `AreaRegistration.RegisterAllAreas();` to registers all areas in an `ASP.NET MVC` application.

> [!NOTE]
> For more information about API,
>
> + see [`AreaRegistration.RegisterAllAreas method (MSDS)`](https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.arearegistration.registerallareas?view=aspnet-mvc-5.2)

### part 1.3

Then it will invoke `WebApiConfig.Register(GlobalConfiguration.Configuration);` (is defined in `WebApiConfig` class in `..\App_Start\WebApiConfig.cs` file) to configure the web api configurations.

You can see how the route is configured in this statement

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
> To learn more about routing in ASP.NET application, you can see [Routing in ASP.NET Web API (MSDS)](https://learn.microsoft.com/en-us/aspnet/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api)

> [!NOTE]
> To hear the Google Gemini analysis,
>
> see [Answers from Google Gemini about WebConfig.Register](https://github.com/40843245/ASP.NET-core-MVC/blob/main/ASP.NET%20core%20MVC4/Answers/Answers%20from%20AI%20model/Google%20Gemini/WebConfig.Register.md#answers-from-google-gemini-about-webconfigregister)

### part 1.4

After that, it will invoke `FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);` (defined in `GlobalFilter`  class in `..\App_Start\FilterConfig.cs` file) to register the filter globally.

```
    public class FilterConfig
    {
        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new HandleErrorAttribute());
        }
    }
```

Let's breakdown.

+ `HandleErrorAttribute` is an attribute that represents how to handle an exception that is thrown by an action method (such as `public ActionResult Create(){ // ...} )`.

+ `GlobalFilterCollection` is a collection that represents all filters that will be used globally.

Thus, invoking `filters.Add(new HandleErrorAttribute());` will add an filter that handles an exception that is thrown by an action method

> [!NOTE]
> For more information about API,
>
> + [System.Web.Mvc.GlobalFilterCollection class (MSDS)](https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.globalfiltercollection?view=aspnet-mvc-5.2)
>
> + [System.Web.Mvc.HandleErrorAttribute class (MSDS)](https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.handleerrorattribute?view=aspnet-mvc-5.2)

### part 1.5

After that, it will invoke `RouteConfig.RegisterRoutes(RouteTable.Routes);` (defined in `RouteConfig` class in `..\App_Start\RouteConfig.cs` file) to ignore the route which is defined in `.axd` file then register the route.

```
    public class RouteConfig
    {
        public static void RegisterRoutes(RouteCollection routes)
        {
            routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

            routes.MapRoute(
                name: "Default",
                url: "{controller}/{action}/{id}",
                defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }
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
> In the above code snippet, it will invoke `routes.MapRoute` method with `url` argument passed as `"{controller}/{action}/{id}"`
>
> which indicates the url will be look like
>
> ```
> http:<ipAddress>/<fileNameExecutedWithoutSuffixController>/<invokedMethod><queries>
> ```
> 
> You can set how the url will look like here.

> [!NOTE]
> By default, when the application is running, it will redirect to
>
> ```
> http:<ipAddress>/Home/Index
> ```
>
> The reason why can be found in the above code snippet.
>
> In the above code snippet, it will invoke `routes.MapRoute` method with `defaults` argument passed as `new { controller = "Home", action = "Index", id = UrlParameter.Optional }`

> [!NOTE]
> For more information about API,
>
> + [System.Web.Routing.RouteCollection class (MSDS)](https://learn.microsoft.com/en-us/dotnet/api/system.web.routing.routecollection?view=netframework-4.8.1)

> [!NOTE]
> To hear Google Gemini analysis,
>
> see [Answers from Google Gemini about RouteConfig.RegisterRoutes](https://github.com/40843245/ASP.NET-core-MVC/blob/main/ASP.NET%20core%20MVC4/Answers/Answers%20from%20AI%20model/Google%20Gemini/RouteConfig.RegisterRoutes.md)

### part 1.6
After that, it will invoke `BundleConfig.RegisterBundles(BundleTable.Bundles);` (defined in `BundleConfig` class in `..\App_Start\BundleConfig.cs` file) to register some bundles 

so that we can use some JS scripts in this project.

```
    public class BundleConfig
    {
        // 如需 Bundling 的詳細資訊，請造訪 http://go.microsoft.com/fwlink/?LinkId=254725
        public static void RegisterBundles(BundleCollection bundles)
        {
            // clear the bundles ignore list.
            bundles.IgnoreList.Clear();

            // add this file as a script into bundle collection so that we can use this file in this project. 
            bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-2.0.3/jquery-2.0.3.js"));

            // ... other `bundle.Add` method invocation.  
        }
    }
```

In `bundles.IgnoreList.Clear();` statement, the ignore of bundles will be cleared.

In `bundles.Add(new ScriptBundle("~/bundles/jquery").Include("~/Scripts/jquery-2.0.3/jquery-2.0.3.js"));` statement, we add this file as a script into bundle collection so that we can use this file in this project. 

![image](https://github.com/user-attachments/assets/4bdf147c-9cb6-4963-80d1-019400a0a272)

> [!NOTE]
> For more information about API,
>
> + [System.Web.Optimization.BundleCollection class (MSDS)](https://learn.microsoft.com/en-us/previous-versions/aspnet/hh195147%28v%3dvs.110%29)
> + [System.Web.Optimization.BundleCollection.ScriptBundle class (MSDS)](https://learn.microsoft.com/en-us/previous-versions/aspnet/jj646584%28v%3dvs.110%29)

> [!WARNING]
> The MSDS [System.Web.Optimization.BundleCollection class (MSDS)](https://learn.microsoft.com/en-us/previous-versions/aspnet/hh195147%28v%3dvs.110%29) says that:
>> We're no longer updating this content regularly. Check the [Microsoft Product Lifecycle](https://learn.microsoft.com/lifecycle/products) for information about how this product, service, technology, or API is supported.
>> ![image](https://github.com/user-attachments/assets/6cd6f6e5-74b3-4c19-b7e6-185aeff658b9)

> [!WARNING]
> The MSDS [System.Web.Optimization.BundleCollection.ScriptBundle class (MSDS)](https://learn.microsoft.com/en-us/previous-versions/aspnet/jj646584%28v%3dvs.110%29)says that:
>> We're no longer updating this content regularly. Check the [Microsoft Product Lifecycle](https://learn.microsoft.com/lifecycle/products) for information about how this product, service, technology, or API is supported.
>> ![image](https://github.com/user-attachments/assets/eafe1a07-6dcd-4325-aad0-7b53c150dadd)

> [!NOTE]
> To hear Google Gemini analysis,
>
> see [Answers from Google Gemini about BundleConfig.RegisterBundles](https://github.com/40843245/ASP.NET-core-MVC/blob/main/ASP.NET%20core%20MVC4/Answers/Answers%20from%20AI%20model/Google%20Gemini/BundleConfig.RegisterBundles.md)

### part 1.7
After that, it will invoke `AutofacConfig.Initialize();` (defined in `AutofacConfig` class in `..\App_Start\AutofacConfig.cs` to initialize the configuration about `Autofac` module.

`AutofacConfig.cs` file

```
    public class AutofacConfig
    {
        public static IContainer Container { get; set; }

        public static void Initialize()
        {
            ContainerBuilder builder = new ContainerBuilder();
            builder.RegisterControllers(Assembly.GetExecutingAssembly());
            builder.RegisterModule(new AutoMapperModule());
            builder.RegisterModule(new DataAccessModule());
            builder.RegisterModule(new CommonHandleModule());
            builder.RegisterModule(new AttributeModule());

            #region For AutoMapper
            IContainer container = builder.Build();
            var profiles = container.Resolve<IEnumerable<BaseProfile>>();
            Mapper.Initialize(x =>
            {
                x.ConstructServicesUsing(container.Resolve);
                profiles.ToList().ForEach(x.AddProfile);
            });
            Container = container;
            #endregion

            DependencyResolver.SetResolver(new AutofacDependencyResolver(container));
        }
    }
```

where 

+ `AutoMapperModule` class is defined in `..\Modules\AutoMapperModule.cs` file.

```
namespace AKM.Modules
{
    public class AutoMapperModule : Autofac.Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterAssemblyTypes(Assembly.GetExecutingAssembly())
                .Where(t => t.Name.EndsWith("Profile"))
                .As(t => t.BaseType);

            builder.RegisterAssemblyTypes(Assembly.GetExecutingAssembly())
                .Where(t => t.Name.EndsWith("Resolver"));
        }
    }
}
```

+ `DataAccessModule` class is defined in `..\Modules\DataAccessModule.cs` file

```
namespace AKM.Modules
{
    public class DataAccessModule : Autofac.Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<AKMContextFactory>().Keyed<IContextFactory>(ContextEnum.AKMContext);
            builder.Register<Func<ContextEnum, IContextFactory>>(c => s => c.ResolveKeyed<IContextFactory>(s));

            builder.RegisterAssemblyTypes(Assembly.GetExecutingAssembly())
                .Where(t => t.Name.EndsWith("Repository"))
                .AsImplementedInterfaces();

            builder.RegisterAssemblyTypes(Assembly.GetExecutingAssembly())
                .Where(t => t.Name.EndsWith("Service"))
                .AsImplementedInterfaces();

            builder.RegisterAssemblyTypes(Assembly.GetExecutingAssembly())
                .Where(t => t.Name.EndsWith("SqlQuery"))
                .AsImplementedInterfaces();
        }
    }
}
```

+ `CommonHandleModule` class is defined in `..\Modules\CommonHandleModule.cs`

```
namespace AKM.Modules
{
    public class CommonHandleModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType(typeof(Encryption)).As(typeof(IEncryption));
            builder.RegisterType(typeof(ParameterEncryption)).As(typeof(IParameterEncryption));
            builder.RegisterType(typeof(Authorization)).As(typeof(IAuthorization));
            builder.RegisterType(typeof(TreeGenerator)).As(typeof(ITreeGenerator));

            #region ReportType

            builder.RegisterType<LastDocumentReportType>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.LastDocument);

            builder.RegisterType<PlatformDocShareReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.PlatformDocShare);

            builder.RegisterType<ProfessionalFieldReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.ProfessionalField);

            builder.RegisterType<ProfessionalFieldRootReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.ProfessionalFieldRoot);

            builder.RegisterType<EverymounthExpertReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.EverymounthExpert);

            builder.RegisterType<LikeExpertReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.LikeExpert);

            builder.RegisterType<FeaturesExpertPeport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.FeaturesExpert);

            builder.RegisterType<ProfessionalFieldIsCommonReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.ProfessionalFieldIsCommon);

            builder.RegisterType<ExpertExperienceReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.ExpertExperience);

            builder.RegisterType<ExpertExpertiseReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.ExpertExpertise);

            builder.RegisterType<ExpertCalendarReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.ExpertCalendar);

            builder.RegisterType<EverymounthCourseReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.EverymounthCourse);

            builder.RegisterType<FeaturesCourseReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.FeaturesCourse);

            builder.RegisterType<CourseFieldReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.CourseField);

            builder.RegisterType<CommonLinkReport>()
               .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.CommonLink);

            builder.RegisterType<NewsCollectReport>()
               .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.NewsCollect);

            builder.RegisterType<PdLawRegulationReport>()
              .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.PdLawRegulation);

            builder.RegisterType<PdLawOperationFlowReport>()
               .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.PdLawOperationFlow);

            builder.RegisterType<LastRegulationReport>()
               .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.LastPdPdLawRegulation);

            builder.RegisterType<LastOperationFlowReport>()
              .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.LastPdLawOperationFlow);

            builder.RegisterType<PersonnelShareReport>()
             .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.PersonnelShare);

            builder.RegisterType<ProfessionalShowHomeReport>()
            .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.ProfessionalShowHomeReport);

            builder.RegisterType<EbasAnnouncementReport>()
           .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.EbasAnnouncementReport);

            builder.RegisterType<BulletinReport>()
            .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.BulletinReport);
            builder.RegisterType<PersonnelBulletinSystemReport>()
             .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.PersonnelBulletinSystemReport);
            builder.RegisterType<PersonnelBulletinNewReport>()
             .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.PersonnelBulletinNewReport);
            builder.RegisterType<PersonnelTableReport>()
            .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.PersonnelTableReport);
            builder.RegisterType<PersonnelStatisticsReport>()
             .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.PersonnelStatisticsReport);
            builder.RegisterType<PersonnelBusinessReport>()
            .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.PersonnelBusinessReport);
            builder.RegisterType<PersonnelTrainReport>()
           .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.PersonnelTrainReport);

            builder.RegisterType<LastCourseReport>()
            .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.LastCourseReport);
            builder.RegisterType<UnitDepReport>()
            .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.UnitDep);
            builder.RegisterType<UnitDepShareReport>()
             .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.UnitDepShare);

            // 20240206 註冊 e等公務員專區功能
            builder.RegisterType<CommonLinkEOpenReport>()
                .As<IReportType>().Keyed<IReportType>(ReportConstants.ReportType.CommonLinkEOpen);

            //Factory
            builder.Register<Func<ReportConstants.ReportType, IReportType>>(c =>
            {
                var componentContext = c.Resolve<IComponentContext>();
                return (reportType) =>
                {
                    var ReportType = componentContext.ResolveKeyed<IReportType>(reportType);
                    return ReportType;
                };
            });

            #endregion
        }
    }
}
```

> [!NOTE]
> For more informations about API,
>
> + [Autofac.ContainerBuilder class (Autofac .NET docs)](https://autofac.org/apidoc/html/717248.htm)
> 

> [!NOTE]
> To hear Google Gemini analysis,
>
> + see [Answers from Google Gemini about AutofacConfig.Initialize](https://github.com/40843245/ASP.NET-core-MVC/blob/main/ASP.NET%20core%20MVC4/Answers/Answers%20from%20AI%20model/Google%20Gemini/AutofacConfig.Initialize.md)
> + see [Answer from Google Gemini about defining AutoMapperModule that inherits Autofac.Module class](https://github.com/40843245/ASP.NET-core-MVC/blob/main/ASP.NET%20core%20MVC4/Answers/Answers%20from%20AI%20model/Google%20Gemini/AutoMapperModule%C2%A0%3A%C2%A0Autofac.Module%20class.md)
