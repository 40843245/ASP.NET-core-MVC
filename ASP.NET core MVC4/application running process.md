# application running process.
When the ASP.NET core MVC4 application runs, it will run the entry point (i.e. invoking `Application_Start` method in `MvcApplication` class (in `Global.asax.cs` file))

After that, it will run in `App_Start` folder to read and set the configuration of

+ route
+ 
