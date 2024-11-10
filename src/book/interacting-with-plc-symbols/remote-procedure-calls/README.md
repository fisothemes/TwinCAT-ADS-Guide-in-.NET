# Remote Procedure Calls (RPCs)

Remote Procedure Calls (RPCs) provide a powerful way to interact with PLC function blocks and interfaces by invoking methods directly from your .NET client application. This approach is particularly useful for handling structured data and executing business logic encapsulated within the PLC, allowing for streamlined communication and control.

In the TwinCAT ADS .NET library, function blocks and interfaces are represented as dynamic types ([`DynamicStructInstance`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409764107.html?id=6274677468644360560) for function blocks and [`DynamicInterfaceInstance`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/12395675659.html?id=7340312406529576735) for interfaces). `DynamicStructInstance` inherits from `DynamicInterfaceInstance`, giving both classes shared access to RPC-related properties and methods. 

This section will guide you through checking for RPC support, listing RPC methods, and invoking them to extend your control over PLC operations.