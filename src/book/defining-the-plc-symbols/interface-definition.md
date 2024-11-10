# Interface Definition

The `I_Value` interface defines a method that will enable Remote Procedure Call (RPC) access. Here’s the definition:

```iec-st
INTERFACE I_Value
{attribute 'TcRpcEnable'}
METHOD Sum : LREAL
VAR_INPUT
    fA, fB : LREAL;
END_VAR
VAR_OUTPUT
    sMessage : STRING;
END_VAR
END_METHOD
END_INTERFACE
```

The `Sum` method is enabled for RPC through the `{attribute 'TcRpcEnable'}` pragma, allowing it to be called remotely via ADS. This method takes two `LREAL` inputs and returns a sum along with a descriptive message in `sMessage`.