# Interacting with PLC Symbols

After defining the symbols we can now begin interacting with them in C# .NET.

In this section, we’ll leverage the [.NET Dynamic Language Runtime (DLR)](https://learn.microsoft.com/en-us/dotnet/framework/reflection-and-codedom/dynamic-language-runtime-overview) to create dynamic objects that represent each PLC symbol. These generated objects mirror the structure and data of the symbols on the PLC, including standard IEC61131 types defined in the PLC. For instance, the PLC symbol `"MAIN.nValue"` is accessed as `MAIN.nValue` in C#, allowing seamless interaction with your PLC's variables.