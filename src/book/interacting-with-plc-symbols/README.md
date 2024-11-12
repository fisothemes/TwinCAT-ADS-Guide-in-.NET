# Interacting with PLC Symbols

Now that we’ve defined the symbols, we can begin interacting with them in C# .NET.

In this section, we’ll use the [.NET Dynamic Language Runtime (DLR)](https://learn.microsoft.com/en-us/dotnet/framework/reflection-and-codedom/dynamic-language-runtime-overview) to create dynamic objects representing each PLC symbol. These objects mirror the structure and data of the symbols on the PLC, including standard IEC 61131 types. For example, the PLC symbol `"MAIN.nValue"` can be accessed as `MAIN.nValue` in C#, allowing straightforward interaction with your PLC’s variables.