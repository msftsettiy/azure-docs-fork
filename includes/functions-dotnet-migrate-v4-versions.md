---
author: mattchenderson
ms.service: azure-functions
ms.topic: include
ms.date: 07/28/2023
ms.author: mahender
---

When you migrate your function app, you have the opportunity to choose the target version of .NET. You can upgrade your C# project to one of the following versions of .NET that are supported by Functions version 4.x: 

| .NET version | [.NET Official Support Policy] release type |  Functions process model<sup>1</sup> | 
| --- | --- | --- |
| .NET 8 (preview<sup>2</sup>) | LTS (preview) | [Isolated worker model] | 
| .NET 7 | STS (end of support May 14, 2024) |  [Isolated worker model] | 
| .NET 6 | LTS (end of support November 12, 2024) |  [Isolated worker model],<br/>[In-process model]  | 
| .NET Framework 4.8 | [See policy][netfxpolicy] | [Isolated worker model] |  

<sup>1</sup> The [isolated worker model] supports Long Term Support (LTS) and Standard Term Support (STS) versions of .NET, as well as .NET Framework. The [in-process model] only supports LTS releases of .NET. For a full feature and functionality comparison between the two models, see [Differences between in-process and isolate worker process .NET Azure Functions](../articles/azure-functions/dotnet-isolated-in-process-differences.md). 

<sup>2</sup> Preview support for .NET 8 function apps is currently limited to Linux applications. To develop using .NET 8 Preview SDKs in Visual Studio, you must use [Visual Studio 2022 Preview](https://visualstudio.microsoft.com/vs/preview/). Within Visual Studio, you must also get the latest toolset and template updates by navigating to `Tools->Options`, selecting `Azure Functions` under `Projects and Solutions`, and then clicking the `Check for updates` button and installing updates as prompted.

[.NET Official Support Policy]: https://dotnet.microsoft.com/platform/support/policy
[netfxpolicy]: https://dotnet.microsoft.com/platform/support/policy/dotnet-framework
[Isolated worker model]: ../articles/azure-functions/dotnet-isolated-process-guide.md
[In-process model]: ../articles/azure-functions/functions-dotnet-class-library.md
