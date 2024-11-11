# Primitives

Primitive types, such as booleans, integers, and floating-point numbers, are handled in TwinCAT ADS .NET library by instances of the [`DynamicSymbol`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409775371.html?id=8821441701441924477) class. To retrieve values from these symbols, call the `ReadValue()` method. Assigning the output to a typed variable ensures type safety, enhances readability, and minimises round-trips to the PLC. For updating values, the `WriteValue(Object)` method allows you to set new values by passing the desired data as a parameter.

### Reading Primitive Values

Here’s an example of reading primitive values from the **`MAIN`** program:

```cs
dynamic MAIN = symbols["MAIN"];

int plcIntValue = MAIN.nValue.ReadValue();
double plcDblValue = MAIN.fValue.ReadValue();
```

### Writing Primitive Values

To modify these values, use `WriteValue()` as shown below:

```cs
MAIN.nValue.WriteValue(888);
MAIN.fValue.WriteValue(6.626);
```
