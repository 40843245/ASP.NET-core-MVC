# How to look version of ASP.NET core mvc?
## way 1
By code,

```
string mvcVersion = typeof (Controller).Assembly.GetName().Version.ToString();
```
## way 2
In packages.config,

```
<package id="Microsoft.AspNet.Mvc" version="5.2.7" targetFramework="net481" />
```

<img width="498" alt="image" src="https://github.com/user-attachments/assets/70a5b447-4eab-4377-b06a-14f30efae2b1" />

## way 3 
Step 1:

Go to your reference folder

Step 2:

look for system.web.mvc

Step 3:

Right Click on it

Step 4:

Click Properties Look at the Version property

## reference

[How to determine the current version of ASP.NET MVC?](https://stackoverflow.com/questions/3008704/how-to-determine-the-current-version-of-asp-net-mvc)
