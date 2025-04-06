# regularity
## 1. About parameters' meaning and definition in all methods under `System.Web.Mvc` namespace

> [!TIP]
> For all methods under `System.Web.Mvc` except than `ActionLink` method, I observe that
> 
>>
>> | parameter index | parameter's type | parameter's name (according to definition in MSDS) | description |
>> | --------------- | ---------------- | ---------------- | ----------- |
>> | 0th parameter | `String` | always named `actionName` in MSDS | It is self-explanatory, it indicates the action's name. |
>> | 1th parameter | `String` | always named `controllerName` in MSDS | It is self-explanatory, it indicates the controller's name. |
>> | 2th parameter | `object` | `routeValues` in MSDS | An object that contains the parameters for a route. The parameters are retrieved through reflection by examining the properties of the object. The object is typically created by using object initializer syntax. |
>> | 2th parameter | `System.Web.Routing.RouteValueDictionary` | Same as above | Same as above |
>> | 3th parameter | `IDictionary<String,Object>` | `htmlAttributes` | An object that contains the HTML attributes to set for the element. In each key-value pair, it will define a new attribute name by key and assign the value as attribute's value (if the key does not exist in the attributes), or it will override the attribute with value (if the key does not exist in the attributes). |
>>
>> And in `ActionLink` method, it has one more parameter. In 0th parameter, it must be a `String` type that indicates the link text. 
>>
>> P.S.
>> 
>> There are many overloads, so I can't list all overloads, I just simply list some common overload and share my observement of the regularity of its parameter.
>>
>> For more details, please see each API of MSDS.
