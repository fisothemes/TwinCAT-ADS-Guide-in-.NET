# Structs

Structs are represented by the [`DynamicStructInstance`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409764107.html?id=6274677468644360560) class, which enables both reading and writing operations for individual members or the entire structure.

### Reading Structs

To retrieve all members of a struct at once, use the `ReadValue()` method:

```cs
dynamic MAIN = symbols["MAIN"];
dynamic plcStructValue = MAIN.stValue.ReadValue();
```

Individual struct members can be accessed directly by name:

```cs
bool plcBoolValue = MAIN.stValue.bValue.ReadValue();
string plcStrValue = MAIN.stValue.sValue.ReadValue();
```

### Writing to Struct Members

To modify specific struct members, use the `WriteValue(Object)` method:

```cs
MAIN.stValue.bValue.WriteValue(false);
MAIN.stValue.sValue.WriteValue("General Kenobi!");
```

### Limitations of Writing Entire Structs

Attempting to write an entire struct at once using an anonymous object will raise an error, as shown below:

```cs
// This produces an error: "Struct member 'bValue' (of ValueType: <>f__AnonymousType0`2) not found!"
MAIN.stValue.WriteValue(new
{
    bValue = true,
    sValue = "Another happy landing!"
});
```

This error occurs because `DynamicValueMarshaler` expects either a `DynamicValue` or a compatible ADS struct type, not a .NET anonymous object. If writing an entire struct in one call would be useful for your application, consider contacting Beckhoff Technical Support to confirm if this behavior is a limitation or intended. 

> **Note:** If you find an alternative approach or improvement, feel free to submit a pull request to enhance this guide for other developers!