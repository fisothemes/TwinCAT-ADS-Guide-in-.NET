# Listening for Value Changes

One of the most valuable features of the ADS client is its capability to track real-time updates to PLC symbols via the [`ValueChanged`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9409850123.html) event. This event notifies you whenever a subscribed symbol’s value changes, making it essential for applications that need to respond to dynamic data.

The code below demonstrates how to register an event handler to the `ValueChanged` event:
```cs
var valueChangedHandler = new EventHandler<ValueChangedEventArgs>
(
    (sender, e) =>
    {
        dynamic val = e.Value;
        Console.WriteLine
        (
            $"Value of {e.Symbol.InstancePath} changed to " +
            (e.Symbol.IsPrimitiveType ? val : JsonConvert.SerializeObject(val))
        );
    }
);

MAIN.fValue.ValueChanged += valueChangedHandler;
MAIN.stValue.ValueChanged += valueChangedHandler;
```