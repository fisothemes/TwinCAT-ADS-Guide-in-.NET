# Enums

Enums, similar to primitive types, are represented by instances of the [`DynamicSymbol`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409775371.html?id=8821441701441924477) class. Reading an enum’s value involves calling the `ReadValue()` method. Since PLC enums are represented by their underlying numeric type (e.g., `byte`, `ushort`, `int`, etc.), it’s best to specify the numeric type directly in the PLC code to maintain consistency between the PLC and .NET.

Here’s an example of an enum type in Structured Text with `UINT` as its underlying type:

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

This setup allows for consistent casting to the correct numeric type in .NET when reading or writing values.

### Reading Enum Values

To read an enum value, use the `ReadValue()` method and cast it to the specified type:

```cs
dynamic MAIN = symbols["MAIN"];
ushort plcEnumValue = (ushort)MAIN.eValue.ReadValue();
```

### Writing Enum Values

To set an enum value, use the `WriteValue(Object)` method. You can either pass the integer representation directly or use .NET features to translate the enum member to its numeric value.

Here’s how to set `eValue` to `Autumn`, which corresponds to `2`:

```cs
MAIN.eValue.WriteValue(2);
```

Alternatively, if you prefer converting an enum name to its numeric equivalent, the [`IDataTypeCollection`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9410065163.html?id=5972020133665255142) and [`IEnumType`](https://infosys.beckhoff.com/content/1033/tcadsnetref/7314516107.html?id=5847315386802257082) interfaces allow for parsing:

```cs
var enumType = (IEnumType)symbolLoader.DataTypes["E_Value"];
var enumValue = (ushort)enumType.Parse("Autumn");
MAIN.eValue.WriteValue(enumValue);
```

### Accessing Enum Names and Values

The `IEnumType` interface also provides convenient access to the enum members’ names and values:

**Names**: Retrieve an array of member names:
```cs
string[] enumNames = enumType.GetNames();
```

**Values**: Retrieve an array of the corresponding numeric values as `IConvertible[]`:
```cs
IConvertible[] enumValues = enumType.GetValues();
```