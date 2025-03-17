# How to bind model in Razor page?
use `@model` keyword;

## examples
### example 1
Suppose I have a model `account_info`

`account_info.cs` file

```
namespace MediaManagerSystem.Models.FormBean
{
    public partial class account_info
    {
        public int Id { get; set; }
        public string ACCOUNT_NAME { get; set; }
        public string PASSWORD { get; set; }
        public string PRIMARY_EMAIL { get; set; }
    }
}
```

Then in a Razor page, I can use `account_info` model using `@model` like this

```
@model IEnumerable<MediaManagerSystem.Models.FormBean.account_info>
```

> [!NOTE]
> DON'T place `;` at the end of @model statements. Otherwise, you will get an compiler error.

## reference
### MSDS link
see this example

<img width="464" alt="image" src="https://github.com/user-attachments/assets/e45446ff-e633-477d-8903-1662ea9be9a8" />

from MSDS [`Strongly typed models and the @model directive` section in this article -- `Part 4, add a model to an ASP.NET Core MVC app` (ASP.NET core in .NET 5.0)](https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-5.0&tabs=visual-studio)

#### example code
```
@model IEnumerable<MvcMovie.Models.Movie>

@{
    ViewData["Title"] = "Index";
}

<h1>Index</h1>

<p>
    <a asp-action="Create">Create New</a>
</p>
<table class="table">
    <thead>
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.Title)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.ReleaseDate)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Genre)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Price)
            </th>
            <th></th>
        </tr>
    </thead>
    <tbody>
@foreach (var item in Model) {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Title)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.ReleaseDate)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Genre)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Price)
            </td>
            <td>
                <a asp-action="Edit" asp-route-id="@item.Id">Edit</a> |
                <a asp-action="Details" asp-route-id="@item.Id">Details</a> |
                <a asp-action="Delete" asp-route-id="@item.Id">Delete</a>
            </td>
        </tr>
}
    </tbody>
</table>
```
