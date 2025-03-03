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
We will dive into this part first -- `process of running the application after the server successfully connected and ready to render first web page`.  

I'm doing so because I think this is a fundamental part and is the simplest if you familiar with these [prequisites](https://github.com/40843245/ASP.NET-core-MVC/blob/main/quickstart%20guide.md#prequisite). 

But don't be worry, we will dive it into these fundamental parts that we need to know for 

developing a project with ASP.NET core MVC architecture in following section respectively.

  + `process of running the application before the server successfully connected`.

  + `process of running the application after the server successfully connected immediately`.

Then summarize it.

### process of running the application after the server successfully connected and ready to render first web page.
Let's dive it into this part.

You can understand the `process of running the application after the server successfully connected and ready to render first web page` with debugger and breakpoint.

The [Demo project of Autofac demo1 in ASP.NET core MVC architecture](https://youtu.be/-q6rQ4UnKUY) illustrate the process of running the application with debugger and breakpoint.

In above link, we can know that

After server and database (if it needs) are BOTH connected successfully, it will set something about Views, Layouts etc before redirecting the url -- `https:\\<ipAddress>\Home\Index`. 

1. it will read `..\Views\Shared\_ViewStart.cshtml`.

`..\Views\Shared\_ViewStart.cshtml`
 
```
@{
    Layout = "~/Views/Shared/_Layout.cshtml";
}
```

> [!TIP]
> In Razor syntax, `~` in path means root directory.
>
> Thus, `"~/Views/Shared/_Layout.cshtml"` is a relative path that is represented as `"~/Views/Shared/_Layout.cshtml"` in Windows 11.

With above code snippets, it will set default layout through rendering `..\Views\Shared\_Layout.cshtml`.

After setting something about Views, Layouts etc,

1. it will redirect the url -- `https:\\<ipAddress>\Home\Index`.

2. According naming convention in ASP.NET core MVC architecture [^1], it will execute `Index` method in `HomeController` class in `..\Controllers\HomeController.cs` file.
   
3. With the following code snippets,

we can know that it will return `View()` and it will display the web page (according to `..\Views\Home\Index.cshtml` file).

part of code snippets in `..\Controllers\HomeController.cs`

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

4. Then `..\Views\Home\Index.cshtml` file will be rendered

`..\Views\Home\Index.cshtml`

```
@{
    ViewBag.Title = "Home Page";
}

<!-- ... -->
<!-- other part of code that will be rendered, displaying result on `https:\\<ipAddress>\Home\Index` web page. -->
```

In above code snippets, we can know the Title getter-setter property of ViewBag class is set to `Home Page` 

(which indicates that the title of the web page is `Home Page`)

> [!TIP]
> Putting `<title>Home Page</title>` in `.html` file gets same result of placing `ViewBag.Title = "Home Page";` under Razor block in `.cshtml` file.
>
> To prove it, you can open webbrowser development tool and select element tab to see the contents of final generated `.html` file.

Additionally, since we don't set specific layout in this file, it will use default layout.

> [!TIP]
> We can specify layout through adding `ViewBag.Layout = "...";` in first Razor block.
>
> For example,
> 
> ```
> @{
>   ViewBag.Title = "Home Page";
>   ViewBag.Layout = "~/Views/Shared/_DefaultLayout`; // add this statement and
> // check `..\Views\Shared\_DefaultLayout.cshtml` file exists.
> }
> ```

### process of running the application before the server successfully connected
Let's dive it into this part.

After we press the `running` button in VS IDE, the following processes will be executed respectively.

1. it will configure the setting of the application under ASP.NET core MVC architecture through reading `..\Web.config` file.

<img width="268" alt="image" src="https://github.com/user-attachments/assets/a92dd73e-af19-4352-b816-57e5b14000f0" />

For more detailed information about how to configure the setting of the application under ASP.NET core MVC architecture, see [configure the setting of the application under ASP.NET core MVC architecture (obsolete)](https://learn.microsoft.com/en-us/previous-versions/aspnet/4w3ex9c2(v=vs.100)?redirectedfrom=MSDN)

2. it will configure the setting of the application under ASP.NET core MVC architecture through reading `..\Views\Shared\Web.config` file.

<img width="275" alt="image" src="https://github.com/user-attachments/assets/eb3488f0-fb53-4228-8c16-ae77c30cbf80" />

3. Then it will try to connect the server and database (if it needs to connect database) through reading `..\Views\Shared\Web.config` file.

### process of running the application after the server successfully connected immediately
Let's dive it into this part.

After the server connected successfully, it will execute these scripts under `..\App_Start` folder.

<img width="268" alt="image" src="https://github.com/user-attachments/assets/dccdb47e-fe07-49c6-a82c-89758bdbb0c8" />

### summary
After we press the `running` button in VS IDE, try to running the application under ASP.NET core MVC architecture, it will 
compiles first. If there are no compiler error at compiler time, it will build the project. After building it will configure the setting of the application (through reading `..\Web.config` file then `..\Views\Shared\Web.config` file),
After configurating the setting of the application, it will try to connect the server and database (if it needs to connect database) (through reading `..\Views\Shared\Web.config` file). If the connections are BOTH successful, it will configure some neccessary bundle or something else (through execute these scripts under `..\App_Start` folder). After that, it will try to render the first web page -- `https:\\<ipAddress>\Home\Index`. By naming convention under this architecture[^1], the `Index` method in `HomeController` class under `..\Controllers\HomeController.cs` file will be executed. According to  code snippets of `Index` method in `HomeController` class under `..\Controllers\HomeController.cs` file. 

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

It will only return `View()`, rendering the `..\Views\Home\Index.cshtml`, display result on web page `https:\\<ipAddress>\Home\Index`.

However, before rendering the `..\Views\Home\Index.cshtml`, it will render `..\Views\Shard\_ViewStart.cshtml` file `and set its default layout in `..\Views\Shared\_Layout.cshtml` file. we first configure the title of the web page as `HomePage` according `ViewBag.Title = "Home Page";` in first Razor block in this file. Since it will set specific layout (through `ViewBag.Layout = "...";`) in first Razor block in this file, it will use default layout.

After that, it will continue to execute the following in this file. 

After execution, it finishes rendering `..\Views\Home\Index.cshtml` file, and displaying the result on web page `https:\\<ipAddress>\Home\Index`.

## demo project
See [Autofac_demo1.7z](https://gitlab.com/jayw711kb/asp.net-core-mvc-demo-project/-/blob/main/Autofac/Autofac_demo1.7z)

See [Demo project of Autofac demo1 in ASP.NET core MVC architecture](https://youtu.be/-q6rQ4UnKUY)

[^1] [naming convention in ASP.NET core MVC](https://github.com/40843245/ASP.NET-core-MVC/blob/main/naming%20convention/MS%20MVC%20naming%20convention.md)
