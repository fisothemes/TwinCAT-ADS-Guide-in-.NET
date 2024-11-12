# Function Block Definition

The `fbValue` variable is an instance of the function block `FB_Value` which implements the `I_Value` interface. This function block will be used to demonstrate remote procedure calls (RPC) and property access through ADS. 

The function block `FB_Value` includes:

- An **RPC method** called `Sum` that calculates the sum of two `LREAL` inputs and returns both the sum and a descriptive string message.
- A **property** called `Value` that can be accessed and modified remotely. This property uses the `{attribute 'monitoring' := 'call'}` pragma to allow read and write operations over ADS.

    > **Note:** This feature isn’t supported on Windows CE-based devices.

Below is the full implementation of `FB_Value`:

```iec-st
FUNCTION_BLOCK FB_Value IMPLEMENTS I_Value
VAR
   _fValue : LREAL;
END_VAR

{attribute 'TcRpcEnable'}
METHOD Sum : LREAL
VAR_INPUT
    fA, fB : LREAL;
END_VAR
VAR_OUTPUT
    sMessage : STRING;
END_VAR
    Sum := fA + fB;
    sMessage := 
        CONCAT('The sum of ', 
        CONCAT(TO_STRING(fA),
        CONCAT(' and ', 
        CONCAT(TO_STRING(fB), 
        CONCAT(' is ', TO_STRING(Sum)
    )))));
END_METHOD

{attribute 'monitoring' := 'call'}
PROPERTY Value : LREAL
GET
    Value := THIS^._fValue;
SET
    THIS^._fValue := Value * 2;
END_PROPERTY

END_FUNCTION_BLOCK
```

- **Method: `Sum`**: This RPC-enabled method takes two `LREAL` parameters (`fA` and `fB`) as inputs and returns their sum. The output `sMessage` provides a summary of the calculation in text format, giving both the input values and the result.
- **Property: `Value`**: This property provides controlled access to the internal `_fValue` variable. In the `GET` accessor, it simply returns the current `_fValue`. In the `SET` accessor, it doubles the input value before storing it back into `_fValue`.