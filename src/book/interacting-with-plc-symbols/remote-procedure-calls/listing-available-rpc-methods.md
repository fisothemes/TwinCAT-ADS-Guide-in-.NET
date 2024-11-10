# Listing Available RPC Methods

You can retrieve a list of available methods using the `RpcMethods` property. This property provides an [`IRpcMethodCollection`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9410264843.html?id=7888281276303216541), containing instances of [`IRpcMethod`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9410254603.html?id=4575198280406047249) for each RPC method available on the symbol. Each `IRpcMethod` instance provides details like the method name, parameters, return type, and additional comments, making it easy to inspect and understand the functionality of each RPC method.

The following example demonstrates how to list the available RPC methods for two PLC symbols, `fbValue` and `ipValue`:

```cs
Console.WriteLine("RPC Methods for fbValue:");
foreach (IRpcMethod method in MAIN.fbValue.RpcMethods)
{
    Console.WriteLine(method.Name);
}

Console.WriteLine("\nRPC Methods for ipValue:");
foreach (IRpcMethod method in MAIN.ipValue.RpcMethods)
{
    Console.WriteLine(method.Name);
}
```

### Inspecting RPC Method Parameters

Each RPC method may have parameters, which you can inspect using the `Parameters` property of an `IRpcMethod` instance. This property provides a collection of [`IRpcMethodParameter`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9410275083.html?id=4592689443333571584) instances, where each instance represents a specific parameter with details like its name and type.

For example, to view the parameters of the `Sum` method on `ipValue`, use the following code:

```cs
Console.WriteLine("\nParameters for the 'Sum' method on ipValue:");
foreach (IRpcMethodParameter param in MAIN.ipValue.RpcMethods["Sum"].Parameters)
{
    Console.WriteLine($"Parameter Name: {param.Name}, Type: {param.TypeName}");
}
```

In this example, the `Parameters` property allows you to access and display each parameter’s name and type, which is useful for understanding what inputs the RPC method expects. This step ensures that you supply the correct parameter types and values when you invoke the method.