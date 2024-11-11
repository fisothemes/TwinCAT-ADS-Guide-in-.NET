# Prerequisites

To make full use of this guide, ensure you have the following installed and configured:

1. **The Beckhoff TwinCAT XAE (eXtended Automation Engineering) or XAR (eXtended Automation Runtime)** – Essential for configuring and deploying PLC applications. Both can be downloaded from the [Beckhoff download finder](https://www.beckhoff.com/en-en/support/download-finder/) webpage.

1. **[Microsoft .NET SDK 8.0](https://dotnet.microsoft.com/en-us/download/dotnet)** – Provides the tools and libraries for developing and running .NET applications. To install it via `winget`, run:
   ```ps
   winget install Microsoft.DotNet.SDK.8
   ```

1. **An IDE for C# Development** – For this book, I used Visual Studio Code with the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the .NET CLI. If you’re new to this setup, please refer to the [Visual Studio Code .NET documentation](https://code.visualstudio.com/docs/languages/dotnet) for additional guidance.

   To get started with the .NET CLI in your chosen directory, you can create a new console application with:
   ```ps
   dotnet new console -n <ProjectName>
   ```
   Use the following command to explore additional project templates:
   ```ps
   dotnet new list
   ```
   To build and run your project, use:
   ```ps
   dotnet run
   ```

   A comprehensive guide to the .NET CLI can be found [here](https://learn.microsoft.com/en-us/dotnet/core/tools/).

1. **[Beckhoff TwinCAT ADS NuGet Package](https://www.nuget.org/packages/Beckhoff.TwinCAT.Ads)** – This package allows you to establish an ADS connection to your device.

   Install it by running this command in your project’s root directory:
   ```ps
   dotnet add package Beckhoff.TwinCAT.Ads
   ```

1. **[Newtonsoft.Json NuGet Package](https://www.nuget.org/packages/Newtonsoft.Json)** – This package is used for JSON serialisation of dynamic objects, which is particularly helpful for displaying complex symbol values in a structured format.

   Install it by running this command in your project’s root directory:
   ```ps
   dotnet add package Newtonsoft.Json
   ```
