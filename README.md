# Manipulating TwinCAT ADS Symbols using Dynamic Objects in .NET

When I first explored TwinCAT ADS for C#/.NET, I quickly realised that the official Infosys documentation was scattered and missed key use cases. Important areas, such as reading complex structs or invoking RPC methods, weren't covered well, and following the examples provided felt like a struggle.

This guide is designed to ease that experience by focusing on how to use TwinCAT ADS with dynamic objects in C#. We’ll cover:

1. Setting up a TwinCAT ADS client in .NET
2. Reading and writing symbol values dynamically
3. Event-driven symbol reading
4. Invoking RPC methods on function blocks

With these examples, you’ll be able to access and control your PLC data smoothly, going beyond what’s typically available in official documentation.

## Prerequisites

To follow along, you’ll need the following:

1. **Beckhoff TwinCAT XAE (eXtended Automation Engineering) or XAR (eXtended Automation Runtime)** for configuring and deploying PLC applications. You can download them using the [download finder](https://www.beckhoff.com/en-en/support/download-finder/) on Beckhoff's website.

2. **[Microsoft .NET SDK 8.0](https://dotnet.microsoft.com/en-us/download/dotnet)** to provide the necessary tools and libraries to develop and run .NET applications.

   You can install it using `winget` with the following command:
   ```ps
   winget install Microsoft.DotNet.SDK.8
   ```

3. An IDE to develop in C#. This guide uses Visual Studio Code with the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the .NET CLI. You can find more setup details in the [Visual Studio Code .NET documentation](https://code.visualstudio.com/docs/languages/dotnet).
   
   If you're using the .NET CLI with VS Code and are new to the setup, here’s a brief overview. In your desired directory, create a new console application project with:
   ```ps
   dotnet new console -n <ProjectName>
   ```
   To see a list of available project templates:
   ```ps
   dotnet new list
   ```
   To build and run your project, use:
   ```ps
   dotnet run
   ``` 

   You can find a comprehensive guide on the .NET CLI [here](https://learn.microsoft.com/en-us/dotnet/core/tools/).

4. The **[Beckhoff TwinCAT ADS NuGet package](https://www.nuget.org/packages/Beckhoff.TwinCAT.Ads)** for .NET to establish an ADS connection to the PLC.

   You can install it using this .NET CLI command on the root directory of your project:
   ```ps
   dotnet add package Beckhoff.TwinCAT.Ads
   ```

5. The **[Newtonsoft.Json NuGet package](https://www.nuget.org/packages/Newtonsoft.Json)** for JSON serialisation, which is useful for printing complex symbol values in a structured form.

   You can install it using the .NET CLI command:
   ```ps
   dotnet add package Newtonsoft.Json
   ```

## Setting Up the ADS Connection

To start, let’s establish a connection to the PLC. The code below assumes you have an active TwinCAT project ready to connect with. For this guide, I recommend creating a new .NET project for simplicity, then pasting the code below into your `Program.cs` file (or whichever file contains your entry point).

```cs
using TwinCAT.Ads;

using (AdsClient client = new())
{
    client.Connect(AmsNetId.Local, 851);
    Console.WriteLine
    (
        "Hello there!\n" +
        $"You're connected to {client.Address} from {client.ClientAddress}.\n" +
        $"The current state of the PLC is: {client.ReadState().AdsState}.\n" +
        "\nPress any key to exit...\n"
    );
    Console.ReadKey(true);
}
```

This code should produce output similar to the following when run:
```console
Hello there!
You're connected to 192.168.137.1.1.1:851 from 192.168.137.1.1.1:XXXXX.
The current state of the PLC is: Run.

Press any key to exit...
```

This code connects to the PLC on the local AMS Net ID with port `851`. It displays the connection details, including the PLC state, which should confirm a successful connection if everything is set up correctly. Pressing any key will then terminate the application. 

This setup gives you a basic foundation for communicating with your PLC through ADS in .NET.

## Defining the PLC Symbols

Now that we’ve established a connection to the PLC, let's look at the symbols we’ll be working with in our TwinCAT project. 

The **MAIN** program in the TwinCAT project is defined as follows:
```iec-st
PROGRAM MAIN
VAR
    nValue  : DINT                  := 42;
    fValue  : LREAL                 := 3.14;
    eValue  : E_Value               := E_Value.Winter;
    arValue : ARRAY[0..2] OF LREAL  := [273.15, 2.71, 9.80665];
    stValue : ST_Value              := (bValue := TRUE, sValue := 'Hello there!');
    fbValue : FB_Value;
    ipValue : I_Value               := fbValue; 
END_VAR
```

We’ll be interacting with four symbols, each representing different data types and complexities:

1. **`nValue`**: A basic integer type.
2. **`fValue`**: A floating-point value (LREAL).
3. **`eValue`**: An enumeration.
4. **`arValue`**: An array containing floating-point numbers (LREAL).
5. **`stValue`**: A structured type that we’ll define below.
6. **`fbValue`**: A function block that includes an RPC method and a property.
7. **`ipValue`**: An interface that includes an RPC method.

### Enumeration Definition: `E_Value`

The `eValue` variable is an instance of type `E_Value`, an enumeration that represents seasons. Here’s its definition:

```iec-st
TYPE E_Value :
(
    _ := 0,
    Summer,
    Autumn,
    Winter,
    Spring
)UINT;
END_TYPE
```

### Struct Definition: `ST_Value`

The `stValue` variable is an instance of the structured data type `ST_Value`. Here’s its definition:

```iec-st
TYPE ST_Value :
STRUCT
    bValue : BOOL;
    sValue : STRING;
END_STRUCT
END_TYPE
```

The struct `ST_Value` contains a boolean (`bValue`) and a string (`sValue`). These members allow us to test interactions with complex data types.

### Interface Definition: `I_Value`

The `I_Value` interface defines a method that will enable Remote Procedure Call (RPC) access. Here’s the definition:

```iec-st
INTERFACE I_Value
{attribute 'TcRpcEnable'}
METHOD Sum : LREAL
VAR_INPUT
    fA, fB : LREAL;
END_VAR
VAR_OUTPUT
    sMessage : STRING;
END_VAR
END_METHOD
END_INTERFACE
```

The `Sum` method is enabled for RPC through the `{attribute 'TcRpcEnable'}` pragma, allowing it to be called remotely via ADS. This method takes two `LREAL` inputs and returns a sum along with a descriptive message in `sMessage`.

### Function Block Definition: `FB_Value`

The `fbValue` variable is an instance of the function block `FB_Value` which implements the `I_Value` interface. This function block demonstrates remote procedure calls (RPC) and property access through ADS. 

The function block `FB_Value` includes:

- An **RPC method** called `Sum` that calculates the sum of two `LREAL` inputs and returns both the sum and a descriptive string message.
- A **property** called `Value` that can be accessed and modified remotely. This property uses the `{attribute 'monitoring' := 'call'}` pragma to allow read and write operations over ADS, though note that this feature isn’t supported on Windows CE-based PLCs.

Below is the full implementation of `FB_Value`:

```iec-st
FUNCTION_BLOCK FB_Value
VAR
   _fValue : LREAL;
END_VAR

{attribute 'TcRpcEnable'}
METHOD Sum : LREAL
VAR_INPUT
    fA, fB : LREAL;
END_VAR
VAR_OUTPUT
    sMessage : STRING;
END_VAR
    Sum := fA + fB;
    sMessage := 
        CONCAT('The sum of ', 
        CONCAT(TO_STRING(fA),
        CONCAT(' and ', 
        CONCAT(TO_STRING(fB), 
        CONCAT(' is ', TO_STRING(Sum)
    )))));
END_METHOD

{attribute 'monitoring' := 'call'}
PROPERTY Value : LREAL
GET
    Value := THIS^._fValue;
SET
    THIS^._fValue := Value * 2;
END_PROPERTY

END_FUNCTION_BLOCK
```

- **Method: `Sum`**: This RPC-enabled method takes two `LREAL` parameters (`fA` and `fB`) as input and returns their sum. The output `sMessage` provides a summary of the calculation in text format, giving both the input values and the result.
- **Property: `Value`**: This property provides controlled access to the internal `_fValue` variable. In the `GET` accessor, it simply returns the current `_fValue`. In the `SET` accessor, it doubles the input value before storing it back into `_fValue`.

These symbols provide a variety of data types and access patterns that will demonstrate how to interact with TwinCAT symbols using ADS.

## Interacting with PLC Symbols in .NET

Now that we’ve established a connection, we can start interacting with our PLC symbols from .NET. Our first task is to load the symbols using a dynamic symbol loader. This loader integrates with the [.NET Dynamic Language Runtime (DLR)](https://learn.microsoft.com/en-us/dotnet/framework/reflection-and-codedom/dynamic-language-runtime-overview) to generate dynamic objects that represent PLC symbols. Each generated dynamic object directly mirrors the target object on the Symbol Server, such as the IEC61131 types defined in the PLC.

### Loading Symbols with the Dynamic Symbol Loader

The following code uses the [SymbolLoaderFactory](https://infosys.beckhoff.com/content/1031/tcadsnetref/7313976459.html?id=3714501762720183437) to create an [IDynamicSymbolLoader](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9410088715.html?id=2842232253864591757). This loader provides access to a [DynamicSymbolsCollection](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409856267.html?id=8767024214869055963), which holds all the symbols available in the PLC:

```cs
var symbolLoader = (IDynamicSymbolLoader)SymbolLoaderFactory.Create
(
    client,
    new SymbolLoaderSettings(SymbolsLoadMode.DynamicTree)
);
var symbols = (DynamicSymbolsCollection)symbolLoader.SymbolsDynamic;
```

To list the top-level symbols in the PLC, use this code snippet:

```cs
foreach (var symbol in symbols) Console.WriteLine(symbol.InstancePath);
```

Assuming a fresh TwinCAT project is active, this should output the following symbols to the console:

```console
Constants
Global_Version
MAIN
TwinCAT_SystemInfoVarList
```

The top-level symbols represent global variables (GVLs) or programs (PROGRAMs). We can now drill down into specific PROGRAMs or GVLs to explore their contents.

### Accessing the MAIN Program Symbol

To access our **MAIN** program symbol, use the following code:

```cs
dynamic MAIN = symbols["MAIN"];
```

Using the DLR, we create a `DynamicSymbol` object representing `MAIN`. Keep in mind that, since this is a dynamic type, IntelliSense won’t provide property or method suggestions. You may need to refer to the [DynamicSymbol documentation](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409775371.html?id=8821441701441924477) as you work.

To see all the sub-symbols under `MAIN`, we can use the `SubSymbols` property. Here’s how:

```cs
foreach (dynamic symbol in MAIN.SubSymbols) Console.WriteLine(symbol.InstancePath);
```

With the TwinCAT project configured as described in the previous section, the output should resemble:

```console
MAIN.arValue
MAIN.eValue
MAIN.fbValue
MAIN.fValue
MAIN.ipValue
MAIN.nValue
MAIN.stValue
```

### Complete Code Example

Here’s the complete code for this section, which establishes a connection to the PLC, loads the symbols, and lists the sub-symbols of the `MAIN` program:

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
    foreach (dynamic symbol in MAIN.SubSymbols)
        Console.WriteLine(symbol.InstancePath);

    Console.WriteLine("\nPress any key to exit...\n");
    Console.ReadKey(true);
}
```

### Expected Output

Running this code should display the following in the console:

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

Now that we've set the foundation for interacting with our symbols. We’re now ready to explore reading, writing, and invoking methods on these dynamic objects for real-world usage.

## Reading and Writing Values from PLC Symbols

Now that we can access symbols, let’s explore how to retrieve and set their values in the **MAIN** program using the dynamic symbol loader. It’s essential that the names of dynamic symbols in .NET match those in the PLC program. For instance, the symbol `"MAIN.nValue"` in the PLC is accessed as `MAIN.nValue` in .NET.

This section demonstrates how to read and write values of various symbol types, starting with primitive types.

### Primitives

Primitive types, such as booleans, integers, and floating-point numbers, are represented by instances of the [DynamicSymbol](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409775371.html?id=8821441701441924477) class. To read their values, simply call the `ReadValue()` method. You can assign this output to a typed variable to ensure type safety and optimize performance by minimizing round-trip calls to the PLC. To modify values, use the `WriteValue(Object)` method, passing the new value as a parameter.

Here’s how to read primitive values:

```cs
dynamic MAIN = symbols["MAIN"];

int plcIntValue = MAIN.nValue.ReadValue();
double plcDblValue = MAIN.fValue.ReadValue();
```

And here’s how to update these values:

```cs
MAIN.nValue.WriteValue(88);
MAIN.fValue.WriteValue(6.626);
```

### Enums

Enums in the PLC, like primitives, are instances of the [DynamicSymbol](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409775371.html?id=8821441701441924477) class. To read an enum's value, call the `ReadValue()` method. Since enums in the PLC are represented by their underlying integer type (e.g., `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`), it’s recommended to specify the enum’s numeric type in your PLC code to ensure type consistency:

```iec-st
TYPE E_Value :
(
    _ := 0,
    Summer,
    Autumn,
    Winter,
    Spring
) UINT;
END_TYPE
```

This lets you safely cast the enum to the correct numeric type in .NET:

```cs
dynamic MAIN = symbols["MAIN"];
ushort plcEnumValue = (ushort)MAIN.eValue.ReadValue();
```

To write a value to an enum, use the `WriteValue(Object)` method, passing either the numeric value directly or the converted integer representation of the enum.

For instance, to set `eValue` to `Autumn`, which has the value `2` in the PLC, you can write directly as follows:

```cs
MAIN.eValue.WriteValue(2);
```

Alternatively, to convert an enum member name to its corresponding numeric value, use the [`IDataTypeCollection`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9410065163.html?id=5972020133665255142) and [`IEnumType`](https://infosys.beckhoff.com/content/1033/tcadsnetref/7314516107.html?id=5847315386802257082) features:

```cs
var enumType = (IEnumType)symbolLoader.DataTypes["E_Value"];
var enumValue = (ushort)enumType.Parse("Autumn");
MAIN.eValue.WriteValue(enumValue);
```

The `IEnumType` interface allows you to retrieve both the names and numeric values of enum members:

- **Names**: Use `GetNames()` to get an array of all enum names:
    ```cs
    string[] enumNames = enumType.GetNames();
    ```

- **Values**: Use `GetValues()` to get an array of all enum values, returned as `IConvertible[]`:
    ```cs
    IConvertible[] enumValues = enumType.GetValues();
    ```

### Arrays

Arrays in ADS are represented as instances of the [DynamicArrayInstance](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409694475.html?id=6573674385667625432) class, allowing both read and write operations on individual elements or the entire array.

To read the complete contents of an array, call the `ReadValue()` method:

```cs
dynamic MAIN = symbols["MAIN"];
double[] plcDblArray = MAIN.arValue.ReadValue();
```

To read a single element in an array, you have several options. Each of these methods retrieves the value of the first element:

```cs
MAIN.arValue[0].ReadValue();
MAIN.arValue.Elements[0].ReadValue();
MAIN.arValue.SubSymbols[0].ReadValue();
```

The first method (`MAIN.arValue[0]`) is recommended, especially for multidimensional arrays, as it simplifies syntax.

Writing to a single array element is straightforward with the `WriteValue(Object)` method:

```cs
MAIN.arValue[0].WriteValue(6.626);
```

Attempting to write directly to an entire array using `WriteValue(Object)` will result in an error. Here’s an example that produces an `ArgumentOutOfRangeException`:

```cs
MAIN.arValue.WriteValue(new double[] { 1.602, 6.022, 1.380 });
```

The error is due to the current limitation of `WriteValue(Object)` on array symbols. Instead, each element must be updated individually, as shown below:

```cs
double[] newValues = { 1.602, 6.022, 1.380 };
for (int i = 0; i < Math.Min(newValues.Length, MAIN.arValue.Elements.Count); i++)
    MAIN.arValue[i].WriteValue(newValues[i]);
```

While this method works reliably, it isn’t the most efficient way to write to an array, as it involves multiple write operations to individual elements. However, this is currently the only available approach. If you believe this limitation is a bug, consider reaching out to Beckhoff Technical Support for further assistance.

> **Tip:** For multidimensional arrays, extend this approach by nesting loops for each dimension.

The IEC-61131-3 standard allows for arrays with custom, non-zero-based bounds. To read and write to elements in non-zero-based arrays, use their specified indices:

```cs
double plcArrayValue = MAIN.arNewValues[-1].ReadValue();
MAIN.arNewValues[-1].WriteValue(10_973_731.568160);
```

The `Dimensions` property provides valuable metadata about an array, including bounds, dimensions, lengths, and if it’s non-zero-based:

```cs
int[] lowerBounds = MAIN.arValue.Dimensions.LowerBounds;
int[] upperBounds = MAIN.arValue.Dimensions.UpperBounds;
int numOfDimensions = MAIN.arValue.Dimensions.Count;
int[] dimensionLengths = MAIN.arValue.Dimensions.GetDimensionLengths();
bool isNonZeroBased = MAIN.arValue.Dimensions.IsNonZeroBased;

foreach (var dim in MAIN.arValue.Dimensions) Console.WriteLine(dim.ElementCount);
```

The `Dimensions` property is especially useful for handling complex arrays, offering flexibility when working with arrays of various sizes and bounds.

### Structs