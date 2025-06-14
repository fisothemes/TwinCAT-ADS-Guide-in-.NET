# Pointers

Pointers are represented by the [`DynamicPointerInstance`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409713931.html?id=4788405057911003115) class. Like other symbols, you can access or modify their raw values using the `ReadValue()` and `WriteValue(Object)` methods.

For safety, avoid using raw pointers. In practice, you typically care about the data a pointer references, not the pointer itself.

You can access the referenced data using the [`Reference`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409715979.html) property, which returns a [`DynamicSymbol`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409775371.html?id=8821441701441924477) instance. This allows you to read or write data normally, as described in the previous or following sections.

### Reading Pointer Values

```cs
dynamic MAIN = symbols["MAIN"];
double plcDblValue = MAIN.pValue.Reference.ReadValue();
```

### Writing Pointer Values

```cs
MAIN.pValue.Reference.WriteValue(6.626);
```

### Advanced Pointer Usage

For a self-referential struct like the following:

```iecst
TYPE ST_Node:
STRUCT
    arData  : ARRAY[0..3] OF ARRAY[0..3] OF ST_Data;
    pNext   : POINTER TO ST_Node;
END_STRUCT
END_TYPE
```

```iecst
PROGRAM MAIN
VAR
    stNode : ST_Node := (pNext := ADR(stNode));
END_VAR
```

You can access the current node’s data and the next node’s data using the `Reference` property:

```cs
dynamic MAIN = symbols["MAIN"];

// Access the current node’s data
dynamic current = MAIN.stNode.ReadValue();
Console.WriteLine(JsonConvert.SerializeObject(current.arData));

// Access the next node’s data
dynamic next = MAIN.stNode.pNext.Reference.ReadValue();
Console.WriteLine(JsonConvert.SerializeObject(next.arData));
```