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






