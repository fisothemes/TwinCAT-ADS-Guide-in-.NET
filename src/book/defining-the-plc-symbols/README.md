# Defining the PLC Symbols

Now that we’ve established a connection to the PLC, let's look at the symbols we’ll be working with in this book. 

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

We’ll be interacting with four symbols, each representing different data types and complexities:

1. **`nValue`**: A basic integer type.
2. **`fValue`**: A floating-point value (LREAL).
3. **`eValue`**: An enumeration.
4. **`arValue`**: An array containing floating-point numbers (LREAL).
5. **`stValue`**: A structured type that we’ll define below.
6. **`fbValue`**: A function block that includes an RPC method and a property.
7. **`ipValue`**: An interface that includes an RPC method.