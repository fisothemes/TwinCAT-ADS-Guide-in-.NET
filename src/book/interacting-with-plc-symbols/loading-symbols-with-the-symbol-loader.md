# Loading Symbols with the Symbol Loader

To interact with PLC variables, we first need to load the symbols using a dynamic symbol loader.

The following code uses the [`SymbolLoaderFactory`](https://infosys.beckhoff.com/content/1031/tcadsnetref/7313976459.html?id=3714501762720183437) to create and return an instance of an object that implements [`IDynamicSymbolLoader`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9410088715.html?id=2842232253864591757) internally. This object which provides access to the [`DynamicSymbolsCollection`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409856267.html?id=8767024214869055963). A collection holds all the symbols available in the PLC, allowing us to browse and interact with them dynamically:

```cs
var symbolLoader = (IDynamicSymbolLoader)SymbolLoaderFactory.Create
(
    client,
    new SymbolLoaderSettings(SymbolsLoadMode.DynamicTree)
);

var symbols = (DynamicSymbolsCollection)symbolLoader.SymbolsDynamic;
```

To list the top-level PLC symbols, use the following snippet:

```cs
foreach (var symbol in symbols) 
{
    Console.WriteLine(symbol.InstancePath);
}
```

Assuming a fresh TwinCAT project is active, this should output the following symbols in the console:

```console
Constants
Global_Version
MAIN
TwinCAT_SystemInfoVarList
```

These top-level symbols represent either global variable lists (GVLs) or programs (PROGRAMs) in the PLC. From here, you can drill down further into specific PROGRAMs or GVLs to explore their contained variables, properties and methods.