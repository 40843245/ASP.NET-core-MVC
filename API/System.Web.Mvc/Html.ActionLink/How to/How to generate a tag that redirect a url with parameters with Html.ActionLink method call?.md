# How to generate `<a>` tag that redirect a url with parameters with `Html.ActionLink` method call?
## Answer
> [!TIP]
> `<a>` tag in html5 represents a link and its `href` indicates the url the web browser will try to redirect to when the end-user clicks the link.
>
> For example
>
> When an end-user clicks a link
> 
> ```
> <a href="https://www.google.com.tw/">
> ```
>
> The web browser will try to redirect the url `https://www.google.com.tw/`.

> [!TIP]
> `Html.ActionLink` method call will generate an `<a>` tag with `href` attribute.

With [Html.ActionLink syntax](https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.html.linkextensions.actionlink?view=aspnet-mvc-5.2), in `Html.ActionLink method` call, to do that, we can, override the attr `href` value.

See following example.

## examples
### example 1

```
@{
 string urlWithParameters = "/AccountInfoes/Index/sid=2017";
 @Html.ActionLink(
   "使用者資訊",
   null,
   null,
   new { href = urlWithParameters }
 );
}
```

will be compiled as 

```
<a href="/AccountInfoes/Index/sid=2017">使用者資訊</a>
```

Don't write the following code snippets. It will not be compiled as above tag. 

(P.S. I have taken this mistaken and thus, got stuck on it about an hour.)

#### wrong example 1.1
```
@{
 string urlWithParameters = "/AccountInfoes/Index/sid=2017";
 @Html.ActionLink(
   "使用者資訊",
   null,
   null,
   new { @href = urlWithParameters }
 );
}
```

will may be compiled as 

```
<a href="/AccountInfoes/Index/href='/AccountInfoes/Index/sid=2017'">使用者資訊</a>
```

#### wrong example 1.2

```
@{
 string sid = "2017";
 @Html.ActionLink(
   "使用者資訊",
   "Index",
   "AccountInfoes",
   new { sid = sid },
 );
}
```

will may be compiled as 

```
<a href="/AccountInfoes/Index" sid="2017">使用者資訊</a>
```

The reason why it occurs explained in [Html.ActionLink syntax]([https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.html.linkextensions.actionlink?view=aspnet-mvc-5.2](https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.html.linkextensions.actionlink?view=aspnet-mvc-5.2#system-web-mvc-html-linkextensions-actionlink(system-web-mvc-htmlhelper-system-string-system-string-system-object-system-object))),

the above method call corresponds the [ActionLink(HtmlHelper, String, String, Object, Object)](https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.html.linkextensions.actionlink?view=aspnet-mvc-5.2#system-web-mvc-html-linkextensions-actionlink(system-web-mvc-htmlhelper-system-string-system-string-system-object-system-object)) method call.

The 3th argument (`new { sid = sid }`) (zero-based, counting from zero), it corresponds to the parameter with type `I`. 

In the above MSDS, we can know that 

+ 3th argument must be passed as a dictionary (an instance with `IDictionary<string,object>` type).
  
+ it defines one or more attributes for html tag.

  In each key-value pair, it will define a new attribute name by key and assign the value as attribute's value (if the key does not exist in the attributes),

  or it will override the attribute with value (if the key does not exist in the attributes).

> [!TIP]
> The regularity can also be applied to parameter type `System.Web.Routing.RouteValueDictionary` in other methods under `System.Web.Mvc` namespace.
>
> In fact, with my observement, there are these common features in other methods under `System.Web.Mvc` namespace.
>
> (P.S. I don't know it until today, so let's share my observement)
>
> [1. About parameters' meaning and definition in all methods under System.Web.Mvc namespace](https://github.com/40843245/ASP.NET-core-MVC/blob/main/API/System.Web.Mvc/regularity/regularity.md#1-about-parameters-meaning-and-definition-in-all-methods-under-systemwebmvc-namespace)

## reference
### API reference
See [Html.ActionLink syntax](https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.html.linkextensions.actionlink?view=aspnet-mvc-5.2)
