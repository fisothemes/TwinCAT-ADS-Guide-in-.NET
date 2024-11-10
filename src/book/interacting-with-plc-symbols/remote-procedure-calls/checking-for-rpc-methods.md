# Checking for RPC Methods

Before attempting to invoke an RPC method, it’s helpful to confirm whether a PLC symbol supports RPC functionality. Both `DynamicInterfaceInstance` and `DynamicStructInstance` classes include an `HasRpcMethods` property, which returns `true` if RPC methods are available for the symbol. Checking this property can help you avoid errors from attempting to call unsupported methods.

Here’s how you can check if a function block or interface has RPC methods:

```cs
var formatYesNoResponse = (bool answer) => answer ? "Yes" : "No";

Console.WriteLine
(
    $"Does {MAIN.fbValue.InstancePath} have RPC methods?: " +
    formatYesNoResponse(MAIN.fbValue.HasRpcMethods)
);

Console.WriteLine
(
    $"Does {MAIN.ipValue.InstancePath} have RPC methods?: " +
    formatYesNoResponse(MAIN.ipValue.HasRpcMethods)
);
```

In this example, the `HasRpcMethods` property provides a straightforward way to determine whether a given symbol, such as `fbValue` or `ipValue`, supports RPC calls. If `HasRpcMethods` returns `true`, you can proceed with further steps, such as listing and invoking available RPC methods.