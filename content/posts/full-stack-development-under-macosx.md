---
title: "Full Stack Development Under MacOS for Two Weeks"
date: 2020-03-05T12:31:29+10:00
draft: false
---

After working on my Lenovo X1 Carbon Gen 6 for nearly two years, I bought myself a new MacBook Pro and start to work on it as my workhorse. To be honest I was a bit nervous in the first a couple of days so I decided to also take my trusty Lenovo with my MacBook to our client office (and I felt that it made my bag weight three times more...), but the overall transition experience was pretty good. Hence I will try to share my experience with you on a series of upcoming blog posts. In this post, I would like to share with your my experience as a full-stack web developer.

To be more specific, the majority part of my daily work is around these frameworks:

- .NET Core: 3.1+
- React: 16+
- Angular: 7+

## Things That Still Work

### Node, React and Angular

Not surprisingly, they work exactly as well as they do under Windows due to their cross-platform nature. I didn't even get one compile error on any of the native node libraries that our project relies on (e.g. [node-sass](https://www.npmjs.com/package/node-sass)).

### Visual Studio Code and Azure Data Studio

A big thumb up to Microsoft for creating such a versatile cross-platform editor. Besides some new keybindings for me to get used to in the first week, everything works exactly the same, including all the extentions I've been using (prettier, tslint, etc). I was worried about the font rendering quality in Visual Studio before the new installation because there was a blurry font rendering issue in an early verison of VSCode I still remembered. But it turns out that VSCode now renders font as sharp as the native apps under Mac.

### Powershell

Again, thanks to the great effort of Microsoft Powershell team on making Powershell 7 a cross-platform product, all the Powershell scripts that work happilly under Windows in my current project can be executed directly without ANY changes. That was amazing.

All you need to do is to follow [these instructions](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-7).

### .NET Core Command Line

You can choose to install .NET Core SDK on Mac via standalone installer, or install it as part of Visual Studio for Mac. For details, please refer to the [offical documentation](https://docs.microsoft.com/en-us/dotnet/core/install/sdk?pivots=os-windows).

After installation, all your familiar .NET Core commands work exactly the same:

```powershell
dotnet restore
dotnet build
dotnet run
```

## Things That Stop Working

### SQL Server

When I got my Lenovo laptop SQL Server was one of those software that I installed almost on day one (with SQL Server Management Studio). But apparently there is no _native_ version of SQL Server for Mac (yet).

### Seq

Similarly, I installed Seq as a Windows service on my Lenovo because it's such a wonderful log server/viewer. Surprisingly there's no native version available for Mac.

### Visual Studio
Visual Studio is undoubtly the most powerful IDE in Windows; but Visual Studio for Mac is totally a different story. My current .NET Core solution contains an F# project. Unfortunately, it didn't even pass the compilation. ![screenshot below](/images/vs-mac-fs-compile-error.png). It looks like the same issue as [this one](https://developercommunity.visualstudio.com/content/problem/712960/f-46-language-features-dont-work-on-vs-for-mac-sta.html) which is still being investigated by Microsoft. It's very odd that the entire solution can be built without any error in .NET Core command line (```dotnet build```). It's the primary issue which blocks me from using Visual Studio for Mac at the moment.

Apart from the F# compilation issue, I am a bit disappointed by how Visaul Studio represents under MacOSX. From the full set of functionality to the smallest bit (as font rendering), it just give you the impression of a half-baked software rather than a mature IDE you can rely on in your daily work.

## But Luckily, I have Workarounds

### Docker

For SQL Server and Seq, luckily we have docker coming to rescue. Here's the `docker-compose.yml` I've been using since the migration:

```yml
version: "3.3"
services:
  sqlserver:
    image: mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
    ports:
      - 1433:1433
    environment:
      ACCEPT_EULA: Y
      SA_PASSWORD: <YOUR_SECURE_SA_PASSWORD>
  seq:
    image: datalust/seq:5.1
    ports:
      - 5341:80
    environment:
      ACCEPT_EULA: Y
```

Please note that you have to specify a [strong password](https://docs.microsoft.com/en-us/sql/relational-databases/security/strong-passwords?view=sql-server-ver15) when creating the SQL container otherwise it will throw an exception.

### Rider
This is a software that Microsoft should be ashamed of. It covers all the gap that Visual Studio left in Mac, and does the work ever better in some aspects. For example, in the project dependency window, it shows those projects which has already added the current project as reference which could cause a cyclic dependency if you do so. It's a small feature but can definitely save your precious time. ![](/images/rider-project-reference.png)

With the help of [Rider](https://www.jetbrains.com/rider/), I can work in the .NET Core project pretty much the same way as I did in Windows. I would highly recommend everyone who would like to use Mac to develop .NET Core projects. 
