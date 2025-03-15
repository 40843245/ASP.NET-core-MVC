# name binding
## intro
When a request from front-end platform sent to back-end platform, the back-end archiecture uses name binding.

## notice
> [!CAUTION]
> The ASP.NET core MVC uses name binding by `name` attribute in html (`.html`) or Razor page (`.cshtml`)
> 
> When there are duplicated elements by `name` attribute in form,
>
> it may choose no elements, it may choose the first element (from begin). I don't how it handles the issue.
>
> We can know that
>
> + when there are duplicated elements by `name` attribute, it may choose no elements and thus, the request sent from `the frot-end server` does not
 have any elements. In this case, the back-end` client will recieve null.
>
> I commended that NEVER use HTML and pzor page.

### exanples
#### example 1
##### wrong example
```
<form action = "/WTF">
  <inpu type="file" name = "JobResume"> // may not be chooesen 
  <inpu type="file" name = "JobResume"> // may not be chooesen 
</form>
```

#### example 2
##### wrong example
In `UserInfo.cs` as view model

```
//...
public class UserInfo
{
 public int Id {get;set;}
 public string UserName {get;set;}
}
//...
```

In `..\Controllers\UserInfoController.cs`

```
//...
public class UserInfoController : Controller
{
   public ActionResult Index()
   {
     reutrn View();
   }

   public ActionResult Index(UserInfo vm)
   {
     reutrn View(vm);
   }
   //...
}
// ...
```

In `..\Views\UserInfo\Index.cshtml`

```
@model YourProjectName.Models.UserInfo;

//...

@using(Html.Begin("Index.""UserInfo")){
  @Html.HiddenFor(model => model.UserName) // may choose this element or may not.
  <h2>Index pgae<h2>
  <input name="@Html.NameFor(model => model.UserName)"> // may choose this element or may not.
}
```

The compiler will parse `.cshtml` into `.html`,

And generate a html elenment

 `<input type="file" name = "JobResume">` 


`@Html.HiddenFor(model => model.UserName)` will be parsed to `<input type="file" name = "JobResume">` 

which

there are duplicated tag by nam attribute.


