---
layout: post
title: Asp.Net Core 1 - 从dotnet new开始说起
---

如果要创建一个Asp.Net的项目，可以通过最新的Visual Studio IDE创建，版本必须是2015 with update 3 或者是2017. 但是也可以通过.net cli的命令行来创建。Microsoft Love Linux，
微软积极的拥抱开源，最近几年一直在努力拥抱开源社区，包括.Net Core， Asp.Net Core都已经开源。
.Net Cli也是一个开源项目，可以通过[地址](https://github.com/dotnet/cli) 访问。在这个开源项目的首页介绍中，可以找到不同平台对应的安装包。

 安装完cli之后，可以通过`dotnet --info`查看信息信息，也可以通过`dotnet --version`查看具体的版本
```
PS E:\git> dotnet --info
.NET Command Line Tools (1.0.0)

Product Information:
 Version:            1.0.0
 Commit SHA-1 hash:  e53429feb4

Runtime Environment:
 OS Name:     Windows
 OS Version:  6.1.7601
 OS Platform: Windows
 RID:         win7-x64
 Base Path:   C:\Program Files\dotnet\sdk\1.0.0
PS E:\git> dotnet --version
1.0.0
```

通过cli可以去创建一个web api的项目
```
PS E:\git> dotnet new webapi -n DemoCoreWebApi
Content generation time: 77.1416 ms
The template "ASP.NET Core Web API" created successfully.
```

当然也可以创建mvc，或者是console项目，可以通过`dotnet new --help`查看具体如何创建
```
PS E:\git> dotnet new --help
Template Instantiation Commands for .NET Core CLI.

Usage: dotnet new [arguments] [options]

Arguments:
  template  The template to instantiate.

Options:
  -l|--list         List templates containing the specified name.
  -lang|--language  Specifies the language of the template to create
  -n|--name         The name for the output being created. If no name is specified, the name of the current directory is used.
  -o|--output       Location to place the generated output.
  -h|--help         Displays help for this command.
  -all|--show-all   Shows all templates


Templates                 Short Name      Language      Tags
----------------------------------------------------------------------
Console Application       console         [C#], F#      Common/Console
Class library             classlib        [C#], F#      Common/Library
Unit Test Project         mstest          [C#], F#      Test/MSTest
xUnit Test Project        xunit           [C#], F#      Test/xUnit
ASP.NET Core Empty        web             [C#]          Web/Empty
ASP.NET Core Web App      mvc             [C#], F#      Web/MVC
ASP.NET Core Web API      webapi          [C#]          Web/WebAPI
Solution File             sln                           Solution

Examples:
    dotnet new mvc --auth None --framework netcoreapp1.1
    dotnet new sln
    dotnet new --help
```

通过cli让web api项目运行起来是很方便的一件事情，首先执行`dotnet restore`来安装相关的依赖，然后执行`dotnet run`让项目运行起来。在项目运行起来之后，我们可以通过cli的output给到的信息，在浏览器
访问http://localhost:5000/api/values 得到这个接口的返回值。
```
PS E:\git\DemoCoreWebApi> dotnet restore
  Restoring packages for E:\git\DemoCoreWebApi\DemoCoreWebApi.csproj...
  Generating MSBuild file E:\git\DemoCoreWebApi\obj\DemoCoreWebApi.csproj.nuget.g.props.
  Generating MSBuild file E:\git\DemoCoreWebApi\obj\DemoCoreWebApi.csproj.nuget.g.targets.
  Writing lock file to disk. Path: E:\git\DemoCoreWebApi\obj\project.assets.json
  Restore completed in 2.15 sec for E:\git\DemoCoreWebApi\DemoCoreWebApi.csproj.

  NuGet Config files used:
      C:\Users\user\AppData\Roaming\NuGet\NuGet.Config
      C:\Program Files (x86)\NuGet\Config\Microsoft.VisualStudio.Offline.config

  Feeds used:
      https://api.nuget.org/v3/index.json
      C:\Program Files (x86)\Microsoft SDKs\NuGetPackages\
PS E:\git\DemoCoreWebApi> dotnet run
Hosting environment: Production
Content root path: E:\git\DemoCoreWebApi
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

既然web api项目已经运行起来了，那么我们怎么才可以访问到它的资源呢？可以通过PowerShell的`Invoke-WebRequest` (或者缩写`iwk`)来访问。得到一个Json格式的返回，值分别为value1,value2。
```
PS C:\Users\user> iwr http://localhost:5000/api/values


StatusCode        : 200
StatusDescription : OK
Content           : ["value1","value2"]
RawContent        : HTTP/1.1 200 OK
                    Transfer-Encoding: chunked
                    Content-Type: application/json; charset=utf-8
                    Date: Mon, 03 Apr 2017 13:43:56 GMT
                    Server: Kestrel

                    ["value1","value2"]
Forms             : {}
Headers           : {[Transfer-Encoding, chunked], [Content-Type, application/json; charset=utf-8], [Date, Mon, 03 Apr 2017 13:43:56 GMT], [Server, Kestrel]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 19
```