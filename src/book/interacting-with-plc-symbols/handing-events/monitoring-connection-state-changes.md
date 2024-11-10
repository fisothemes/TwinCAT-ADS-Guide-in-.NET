# Monitoring Connection State Changes

The [`ConnectionStateChanged`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9407908619.html) event allows you to track changes in the connection status of the ADS client. This is useful for managing connectivity and taking appropriate actions when the connection to the PLC is established or lost.

> **Note:** The connection state changes only when the client actively connects to or disconnects from the PLC, or when communication is initiated by the user.

The code below demonstrates how to register an event handler to the `ConnectionStateChanged` event:
```cs
var connectionStateChangedHandler = new EventHandler<ConnectionStateChangedEventArgs>
(
    (sender, e) =>
    {
        Console.WriteLine($"Client connection state: {e.NewState}.");
    }
);

client.ConnectionStateChanged += connectionStateChangedHandler;
```