# ASP.NET core MVC
## prequisite 
Before building a project with ASP.NET core MVC architecture, you have to install.

1. VS IDE
2. .NET Framework
3. ASP.NET core MVC

Before developing a project with ASP.NET core MVC architecture, you have to build a project with ASP.NET core MVC architecture in VS IDE. And be familiar with

+ core design pattern of programming language

  - OOP
    
+ front-end

  - html5 (a markup syntax)
  - js (programming lanuage with weak type)
  - jQuery (a kind of JS library) (based on JS and html)
  - [AJAX](https://zh.wikipedia.org/zh-tw/AJAX) (stands for Asynchronic JavaScript And XML) (based on JS and html)
  - [Razor syntax](https://zh.wikipedia.org/zh-tw/ASP.NET_Razor) (a markup syntax based on `html5`) (a `.cshtml` file supports Razor syntax and `.html` file supports) 
+ back-end
  - C# programming language.

## quickstart guide
### building a project with ASP.NET core MVC architecture in VS IDE
Similar step as building a project with Console in VS IDE.

Step 1:

Click `building a new project`.

<img width="750" alt="image" src="https://github.com/user-attachments/assets/ef850ab8-717a-4ce4-987c-48308ccce0bb" />

Step 2:

In popup window, select C# as programming language and Windows as platform.

Then select ASP.NET Web Application.

<img width="749" alt="image" src="https://github.com/user-attachments/assets/497bac01-2854-46b7-bbe8-5c83e8e820b5" />

Then click `Next` Button.

Step 3:

Fill in basic info. Including 

+ Application name: name of project.
  
+ Location name: the project will be created at

+ Solution name: name of solution. (`.sln` file extension)

And untick the option `Place Solution and project in same directory`.

<img width="759" alt="image" src="https://github.com/user-attachments/assets/1ac505c3-0489-4c88-97cb-788db2b4f379" />

Then click `Next` button.

Step 5:

Select `MVC` option.

Then click `Build` button.

<img width="755" alt="image" src="https://github.com/user-attachments/assets/da96075a-39b8-48d8-8ca0-f13d85671cea" />

Step 6:

That's done. Just wait for a minute.

## understand the process of running the application
You can understand the process of running the application with debugger and breakpoint.

The [Demo project of Autofac demo1 in ASP.NET core MVC architecture](https://youtu.be/-q6rQ4UnKUY) illustrate the process of running the application with debugger and breakpoint.

In above link, we can know that

When running the application, 

1. it will redirect the url as `https:\\<ipAddress>\Home\Index`.

2. Through naming convention in ASP.NET core MVC architecture [^1], it will execute `Index` method in `HomeController` class in `..\Controllers\HomeController.cs` file.
   
3. With the following code snippets,

we can know that it will return `View()` and it will display the web page (according to `..\Views\Home\Index.cshtml` file).

```
    public class HomeController : Controller
    {
        public ActionResult Index()
        {
            return View();
        }

        // ...
    }
```
## demo project
See [Autofac_demo1.7z](https://gitlab.com/jayw711kb/asp.net-core-mvc-demo-project/-/blob/main/Autofac/Autofac_demo1.7z)

See [Demo project of Autofac demo1 in ASP.NET core MVC architecture](https://youtu.be/-q6rQ4UnKUY)

[^1] [naming convention in ASP.NET core MVC](https://github.com/40843245/ASP.NET-core-MVC/blob/main/naming%20convention/MS%20MVC%20naming%20convention.md)
