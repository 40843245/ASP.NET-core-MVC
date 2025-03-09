# Adding Scaffold item via model
Step 1:

Create a class in `..\Models` folder.

Step 2:

Install `Microsoft.EntityFrameworkCore.Design` package.

> [!TIP]
> You can install package through
>
> Install-Package command in PMC.
>
> For example, you can Install `Microsoft.EntityFrameworkCore.Design` package in PMC.
> 
> ```
> Install-Package Microsoft.EntityFrameworkCore.Design
> ```

Step 3:

Right click `..\Controllers` folder.

Step 4:

Select Add -> Add Scaffold item.

<img width="512" alt="image" src="https://github.com/user-attachments/assets/e2dfa05a-0465-4f4f-afdd-dea2da89272f" />

Step 5: 

Choose `MVC 5 Controller containing View, using EntityFrameWork`.

Then click `Add`.

<img width="714" alt="image" src="https://github.com/user-attachments/assets/f8c8f9e2-3cdd-493b-8678-cd8ebf872882" />

Step 6:

In `Adding Controller` dialog,

Fill in basic info. Including

+ Model Class named as `<ModelClass>`: The class that will be used as model.
+ Data Context Class named as `<DataContextClass>`: The generated class that will be used as Database Context.

> [!NOTE]
> (You can add it by clicking `Add` icon.)

+ Controller name named as `<ControllerName>`: The generated class that will be used as a controller.

Then click `Add` button.

<img width="455" alt="image" src="https://github.com/user-attachments/assets/77230fb4-c647-458b-8063-cbb6457594a1" />

Step 7:

Wait for a moment.

Step 8:

If there are no errors, congrulations. 

You will see these files generated.

+ `<ControllerName>.cs` in `..\Controllers` folder with class `<ControllerName>` which inherits Controller class.
+ `<DataContextClass>.cs` in `..\Data` folder (by default) with `<DataContextClass>` which inherits `DbContext`.

## examples
### example 1
I follow these instruction in MSDS [Part 4, add a model to an ASP.NET Core MVC app](https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-5.0&tabs=visual-studio)

Step 1:

Create a `Move.cs` in `..\Models` folder and update it 

```
using System;
using System.ComponentModel.DataAnnotations;

namespace MvcMovie.Models
{
    public class Movie
    {
        public int Id { get; set; }
        public string Title { get; set; }

        [DataType(DataType.Date)]
        public DateTime ReleaseDate { get; set; }
        public string Genre { get; set; }
        public decimal Price { get; set; }
    }
}
```

Step 2:

Install `Microsoft.EntityFrameworkCore.Design` package.

> [!TIP]
> You can install package through
>
> Install-Package command in PMC.
>
> For example, you can Install `Microsoft.EntityFrameworkCore.Design` package in PMC.
> 
> ```
> Install-Package Microsoft.EntityFrameworkCore.Design
> ```

Step 3:

Right click `..\Controllers` folder.

Step 4:

Select Add -> Add Scaffold item.

<img width="512" alt="image" src="https://github.com/user-attachments/assets/e2dfa05a-0465-4f4f-afdd-dea2da89272f" />

Step 5: 

Choose `MVC 5 Controller containing View, using EntityFrameWork`.

Then click `Add`.

<img width="714" alt="image" src="https://github.com/user-attachments/assets/f8c8f9e2-3cdd-493b-8678-cd8ebf872882" />

Step 6:

In `Adding Controller` dialog,

Fill in basic info. Including

+ Model Class named as `Movie`: The class that will be used as model.
+ Data Context Class named as `MvcMovieContext`: The generated class that will be used as Database Context.

> [!NOTE]
> (You can add it by clicking `Add` icon.)

+ Controller name named as `MoviesController`: The generated class that will be used as a controller.

Then click `Add` button.

<img width="455" alt="image" src="https://github.com/user-attachments/assets/77230fb4-c647-458b-8063-cbb6457594a1" />

Step 7:

Wait for a moment.

Step 8:

If there are no errors, congrulations. 

You will see these files generated.

+ `MoviesController.cs` in `..\Controllers` folder with class `MoviesController` which inherits Controller class.
+ `MvcMovieContext.cs` in `..\Data` folder (by default) with `MvcMovieContext` which inherits `DbContext`.
