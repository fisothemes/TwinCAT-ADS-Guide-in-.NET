# Complete Code Example

Now that we've explored accessing the **`MAIN`** program symbol and listing its sub-symbols, let’s put it all together in a full example. This complete code snippet covers each step we've discussed: connecting to the PLC, loading symbols dynamically, and accessing the **`MAIN`** program symbol and its contents.

The code example below demonstrates how to import the necessary libraries, establish a connection to your device, load the available symbols using the `SymbolLoaderFactory`, and list both the top-level symbols and the sub-symbols within **`MAIN`**.

```cs
using TwinCAT;
using TwinCAT.Ads;
using TwinCAT.Ads.TypeSystem;
using TwinCAT.TypeSystem;

using (AdsClient client = new())
{
    client.Connect(AmsNetId.Local, 851);
    var symbolLoader = (IDynamicSymbolLoader)SymbolLoaderFactory.Create
    (
        client,
        new SymbolLoaderSettings(SymbolsLoadMode.DynamicTree)
    );

    var symbols = (DynamicSymbolsCollection)symbolLoader.SymbolsDynamic;

    foreach (var symbol in symbols) Console.WriteLine(symbol.InstancePath);
    Console.WriteLine();

    dynamic MAIN = symbols["MAIN"];
    
    foreach (var symbol in MAIN.SubSymbols) Console.WriteLine(symbol.InstancePath);

    Console.WriteLine("\nPress any key to exit...\n");
    Console.ReadKey(true);
}
```

Running this code will display the following:

```console
Constants
Global_Version
MAIN
TwinCAT_SystemInfoVarList

MAIN.arValue
MAIN.eValue
MAIN.fbValue
MAIN.fValue
MAIN.ipValue
MAIN.nValue
MAIN.stValue

Press any key to exit...
```

This is a list of the top-level symbols, followed by all sub-symbols within **`MAIN`**, demonstrating how to examine the structure of your PLC program and begin working with its data. This setup provides a solid foundation for the dynamic handling of symbols that will be used the following sections of this book.