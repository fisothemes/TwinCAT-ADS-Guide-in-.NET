# Function Blocks

Function blocks can be accessed similarly to structs, allowing access to both local variables and properties. Properties marked with the `{attribute 'monitoring' := 'call'}` pragma can be accessed just like struct members. When properties contain code within their getter or setter blocks, that code will execute upon reading or writing to the property directly.

### Reading Function Blocks

To retrieve the entire function block, use the `ReadValue()` method:

```cs
dynamic MAIN = symbols["MAIN"];
dynamic plcFBValue = MAIN.fbValue.ReadValue();
```

Once you have retrieved the function block as a whole, you can access its local variables directly as shown below:

```cs
double localValue = plcFBValue._fValue; 
```

You can also retrieve property values using this method, but be aware that doing so will **NOT** trigger any logic within the getter. The following code retrieves the property value without executing its getter logic:

```cs
double propValue = plcFBValue.Value;
```

To ensure that the getter logic is called, you need to access the property directly and use the `ReadValue()` method, as shown here:

```cs
double propValue = MAIN.fbValue.Value.ReadValue();
```

You can also directly read the value of any function block local variable:

```cs
double localValue = MAIN.fbValue._fValue.ReadValue();
``` 

### Writing Function Blocks

The only way to write to function block local variables and properties is to use `WriteValue(Object)` directly on them:

```cs
MAIN.fbValue._fValue.WriteValue(2001);
MAIN.fbValue.Value.WriteValue(66);
```

You cannot modify PLC symbols in a function block that has been retrieved as a whole:

```cs
// Compiles but does nothing.
plcFBValue._fValue = 1999.0;
plcFBValue.Value = 300.0;
```