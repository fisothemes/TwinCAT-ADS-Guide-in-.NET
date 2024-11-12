# Defining the PLC Symbols

Now that we’ve established a connection to the PLC, let's examine the symbols we’ll be working with in this book.

The **`MAIN`** program in the TwinCAT project is defined as follows:
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

In this book, we’ll be interacting with a variety of symbols, each representing different data types and complexities, including:

1. **`nValue`**: A basic integer type.
1. **`fValue`**: A floating-point value (LREAL).
1. **`eValue`**: An enumeration.
1. **`arValue`**: An array containing floating-point numbers (LREAL).
1. **`stValue`**: A structured type that we’ll define below.
1. **`fbValue`**: A function block that includes an RPC method and a property.
1. **`ipValue`**: An interface that includes an RPC method.