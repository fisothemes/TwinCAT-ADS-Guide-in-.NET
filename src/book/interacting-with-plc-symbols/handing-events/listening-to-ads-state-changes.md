# Listening to ADS State Changes

The [`AdsStateChanged`](https://infosys.beckhoff.com/content/1033/tc3_ads.net/9407905547.html) event enables you to monitor when the ADS client transitions between different operational states. This event is beneficial for tracking ADS state transitions and responding to conditions where the client moves between states such as `Run`, `Stop`, or `Error`.

The code below demonstrates how to register an event handler to the `AdsStateChanged` event:
```cs
var adsStateChangedHandler = new EventHandler<AdsStateChangedEventArgs>
(
    (sender, e) =>
    {
        Console.WriteLine
        (
            $"ADS state changed. New state: {e.State.AdsState}, " +
            $"Device state: {e.State.DeviceState}"
        );
    }
);

client.AdsStateChanged += adsStateChangedHandler;
```