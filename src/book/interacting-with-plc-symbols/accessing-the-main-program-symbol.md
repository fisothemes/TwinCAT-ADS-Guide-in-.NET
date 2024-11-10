# Accessing the MAIN Program Symbol

The **`MAIN`** program symbol usually represents the primary program block in our TwinCAT PLC setup.

To retrieve the **`MAIN`** program symbol from the `DynamicSymbolsCollection`, we can use either of these commands:

```cs
dynamic MAIN = symbols["MAIN"];
```

or more succinctly:

```cs
dynamic MAIN = symbols.MAIN;
```

Here, we're creating a `DynamicSymbol` object representing `MAIN` using the Dynamic Language Runtime (DLR). This approach allows us to interact with symbols flexibly. However, since it’s a dynamic type, IntelliSense won't provide auto-suggestions for properties or methods. Please, familiarising yourself with the [`DynamicSymbol` documentation](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409775371.html?id=8821441701441924477). It will be useful as you work with these objects.

To inspect the symbols under **`MAIN`**, we can iterate over its `SubSymbols` property. This provides a list of all variables and data structures contained within **`MAIN`**, which will vary depending on your PLC configuration:

```cs
foreach (var symbol in MAIN.SubSymbols) 
{
    Console.WriteLine(symbol.InstancePath);
}
```

If you've setup your TwinCAT project using the symbols we defined earlier, the above code snippet should output:

```console
MAIN.arValue
MAIN.eValue
MAIN.fbValue
MAIN.fValue
MAIN.ipValue
MAIN.nValue
MAIN.stValue
```