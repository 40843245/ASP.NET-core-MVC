# Url.Action
## intro
Generates an url with zero or more parameters.

## details
Given 

+ action (by `actionName` parameter)
+ controller (by `controllerName` parameter)
+ and url parameters (by `routeValues` parameters),

generates an url with zero or more parameters.

> [!NOTE]
> About `routeValues` parameter.
> 
> 1. If `routeValues` is NOT specified, the generated url will be parameterless.
>
> Otherwise, the generated url will have one or more parameters.
>
> 2. `routeValues` must be a one of these types: `System.Web.Routing.RouteValueDictionary` or `object`.
>
> The url parameters will be determined by `routeValue`.
> 
> 3. In each key-value pair, the key indicates the url parameters name. While the value indicates the url parameters value.
>
> See the following example for more fully understand.

## syntax
See link in reference section.

## examples
### example 1

```
string url = Url.Action(
"Index",
"AccountInfoes"
);
```

The url will be 

```
/AccountInfoes/Index
```

### example 2

```
string url = Url.Action(
"Index",
"AccountInfoes",
new { sid = "1007"}
);
```

The url will be 

```
/AccountInfoes/Index?sid=1007
```

### example 3

```
string url = Url.Action(
"Index",
"AccountInfoes",
new { sid = "1007" , loginTimes = "2" }
);
```

The url will be 

```
/AccountInfoes/Index?sid=1007&loginTimes=2
```

## reference
### API reference
See [UrlHelper.Action Method](https://learn.microsoft.com/en-us/dotnet/api/system.web.mvc.urlhelper.action?view=aspnet-mvc-5.2)
