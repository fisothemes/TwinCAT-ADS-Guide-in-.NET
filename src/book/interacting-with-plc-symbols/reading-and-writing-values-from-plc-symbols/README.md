# Reading and Writing Values from PLC Symbols

In this section, we’ll explore how to retrieve and set values for various PLC symbols, focusing on those defined in the **`MAIN`** program from earlier sections using the dynamic symbol loader. Remember, it’s essential that the names of dynamic symbols in C# .NET precisely match those in the PLC program. For instance, the symbol `"MAIN.nValue"` in the PLC is accessed as `MAIN.nValue` in .NET.

We’ll begin by working with primitive types, then move on to more complex data structures, demonstrating how to interact with each type through dynamic symbols.