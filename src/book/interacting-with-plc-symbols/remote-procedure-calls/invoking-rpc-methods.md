# Invoking RPC Methods

The ADS .NET library provides two approaches to call an RPC method: 

1. **Using the `InvokeRpcMethod` method** — This method provides flexibility, especially when handling output parameters, and ensures compatibility with the RPC’s structure.
2. **Direct method invocation syntax** — For conciseness, if you don’t require handling output parameters.

### Using `InvokeRpcMethod`

To call an RPC method using `InvokeRpcMethod`, provide the name of the RPC method, an array containing input parameters, and an optional array for output parameters. This method is suitable when you need precise control over both input and output parameters.

```cs
object[] inParams = { 1, 2 };
object[] outParams;

double result = MAIN.ipValue.InvokeRpcMethod("Sum", inParams, out outParams);

Console.WriteLine($"Sum result: {result}");
Console.WriteLine($"Output message: {outParams[0]}");
```

### Using Direct Method Invocation Syntax

For a more concise call, you can use direct method invocation syntax. This approach simplifies the code, although it may not populate output parameters as expected.

```cs
string message;
double result = MAIN.ipValue.Sum(1, 2, out message);

Console.WriteLine($"Sum result: {result}");
Console.WriteLine($"Output message: {message}");
```

> **Note:** When using direct invocation syntax, output parameters might not be populated. This may be a limitation in the ADS .NET library, so consider reaching out to Beckhoff Technical Support for clarification. If output parameter values are essential, it’s recommended to use `InvokeRpcMethod` instead for reliable handling of output data.