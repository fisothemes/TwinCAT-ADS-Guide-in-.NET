# Arrays

Arrays are represented as instances of the [`DynamicArrayInstance`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409694475.html?id=6573674385667625432) class, supporting read and write operations for both individual elements and entire arrays.

### Reading Arrays

To read an entire array, use the `ReadValue()` method:

```cs
dynamic MAIN = symbols["MAIN"];
double[] plcDblArray = MAIN.arValue.ReadValue();
```

To retrieve a single element in an array, there are multiple approaches. Each of these reads the first element:

```cs
MAIN.arValue[0].ReadValue();
MAIN.arValue.Elements[0].ReadValue();
MAIN.arValue.SubSymbols[0].ReadValue();
```

Using `MAIN.arValue[0]` is recommended, particularly for multidimensional arrays, as it simplifies syntax.

### Writing to Array Elements

To modify an entire array, use the `WriteValue(Object)` method:

```cs
MAIN.arValue.WriteValue(new double[] { 1.602, 6.022, 1.380 });
```

To write a single element in an array simply index into it:

```cs
MAIN.arValue[0].WriteValue(6.626);
```

You can also use a loop to write multiple elements at once:

```cs
double[] newValues = { 1.602, 6.022, 1.380 };
for (int i = 0; i < Math.Min(newValues.Length, MAIN.arValue.Elements.Count); i++)
    MAIN.arValue[i].WriteValue(newValues[i]);
```

### Handling Non-Zero-Based Arrays

The IEC-61131-3 standard permits arrays with custom, non-zero-based bounds. To interact with these arrays, use their specific indices:

```cs
double plcArrayValue = MAIN.arNewValues[-1].ReadValue();
MAIN.arNewValues[-1].WriteValue(10_973_731.568160);
```

### Array Metadata with `Dimensions`

The `Dimensions` property offers valuable metadata about an array, such as bounds, dimensions, and whether it’s non-zero-based:

```cs
int[] lowerBounds = MAIN.arValue.Dimensions.LowerBounds;
int[] upperBounds = MAIN.arValue.Dimensions.UpperBounds;
int numOfDimensions = MAIN.arValue.Dimensions.Count;
int[] dimensionLengths = MAIN.arValue.Dimensions.GetDimensionLengths();
bool isNonZeroBased = MAIN.arValue.Dimensions.IsNonZeroBased;

foreach (var dim in MAIN.arValue.Dimensions) Console.WriteLine(dim.ElementCount);
```

The `Dimensions` property is especially useful when working with complex arrays of various sizes and bounds, providing flexibility in managing diverse array structures.