# Adding Scaffold item via model
Step 1:

Create a data model class `<ModelClass>` in `..\Models` folder.

> [!WARNING]
> The data model class must have a key.
>
> By default, Entity Framework CodeFirst recognize the key by name Valid names are Id or `<ModelClass>Id`.
>
> For more information, see [Creating Primary Key field on MVC class](https://stackoverflow.com/questions/10236819/creating-primary-key-field-on-mvc-class)

Step 2:

Install `Microsoft.EntityFrameworkCore.Design` package.

> [!WARNING]
> Please make sure that requirements of `Microsoft.EntityFrameworkCore.Design`.
>
> The requirements of `Microsoft.EntityFrameworkCore.Design` package depends on `.NET Framework`.

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
+ `Create.cshtml` in `..\Views\<ModelClass>` folder.
+ `Delete.cshtml` in `..\Views\<ModelClass>` folder.
+ `Details.cshtml` in `..\Views\<ModelClass>` folder. 
+ `Edit.cshtml` in `..\Views\<ModelClass>` folder.
+ `Index.cshtml` in `..\Views\<ModelClass>` folder.
  
## examples
### example 1
I follow these instruction in MSDS [Part 4, add a model to an ASP.NET Core MVC app](https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-5.0&tabs=visual-studio)

Step 1:

Create a `Movie.cs` in `..\Models` folder and update it 

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

```
using System.Data.Entity;
using System.Linq;
using System.Net;
using System.Web.Mvc;
using MvcMovie.Data;
using MvcMovie.Models;

namespace MvcMovie.Controllers
{
    public class MoviesController : Controller
    {
        private MvcMovieContext db = new MvcMovieContext();

        // GET: Movies
        public ActionResult Index()
        {
            return View(db.Movies.ToList());
        }

        // GET: Movies/Details/5
        public ActionResult Details(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Movie movie = db.Movies.Find(id);
            if (movie == null)
            {
                return HttpNotFound();
            }
            return View(movie);
        }

        // GET: Movies/Create
        public ActionResult Create()
        {
            return View();
        }

        // POST: Movies/Create
        // 若要免於大量指派 (overposting) 攻擊，請啟用您要繫結的特定屬性，
        // 如需詳細資料，請參閱 https://go.microsoft.com/fwlink/?LinkId=317598。
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "Id,Title,ReleaseDate,Genre,Price")] Movie movie)
        {
            if (ModelState.IsValid)
            {
                db.Movies.Add(movie);
                db.SaveChanges();
                return RedirectToAction("Index");
            }

            return View(movie);
        }

        // GET: Movies/Edit/5
        public ActionResult Edit(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Movie movie = db.Movies.Find(id);
            if (movie == null)
            {
                return HttpNotFound();
            }
            return View(movie);
        }

        // POST: Movies/Edit/5
        // 若要免於大量指派 (overposting) 攻擊，請啟用您要繫結的特定屬性，
        // 如需詳細資料，請參閱 https://go.microsoft.com/fwlink/?LinkId=317598。
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "Id,Title,ReleaseDate,Genre,Price")] Movie movie)
        {
            if (ModelState.IsValid)
            {
                db.Entry(movie).State = EntityState.Modified;
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(movie);
        }

        // GET: Movies/Delete/5
        public ActionResult Delete(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Movie movie = db.Movies.Find(id);
            if (movie == null)
            {
                return HttpNotFound();
            }
            return View(movie);
        }

        // POST: Movies/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Movie movie = db.Movies.Find(id);
            db.Movies.Remove(movie);
            db.SaveChanges();
            return RedirectToAction("Index");
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
}
```

+ `MvcMovieContext.cs` in `..\Data` folder (by default) with `MvcMovieContext` which inherits `DbContext`.

```
using System.Data.Entity;

namespace MvcMovie.Data
{
    public class MvcMovieContext : DbContext
    {
        // You can add custom code to this file. Changes will not be overwritten.
        // 
        // If you want Entity Framework to drop and regenerate your database
        // automatically whenever you change your model schema, please use data migrations.
        // For more information refer to the documentation:
        // http://msdn.microsoft.com/en-us/data/jj591621.aspx
    
        public MvcMovieContext() : base("name=MvcMovieContext")
        {
        }

        public System.Data.Entity.DbSet<MvcMovie.Models.Movie> Movies { get; set; }
    }
}
```

+ `Create.cshtml` in `..\Views\Movies` folder.
+ `Delete.cshtml` in `..\Views\Movies` folder.
+ `Details.cshtml` in `..\Views\Movies` folder. 
+ `Edit.cshtml` in `..\Views\Movies` folder.
+ `Index.cshtml` in `..\Views\Movies` folder.

The hierarchy is as follows.

<img width="188" alt="image" src="https://github.com/user-attachments/assets/bb89acc2-9502-4804-a9d6-90c87994d04b" />
