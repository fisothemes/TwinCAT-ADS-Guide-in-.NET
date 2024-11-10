# Setting Up the ADS Connection

To start, let’s establish a connection to the PLC. The code below assumes you have an active TwinCAT project ready to connect with. For this guide, I recommend creating a new .NET project for simplicity, then pasting the code below into your `Program.cs` file (or whichever file contains your entry point).

```cs
using TwinCAT.Ads;

using (AdsClient client = new())
{
    client.Connect(AmsNetId.Local, 851);
    Console.WriteLine
    (
        "Hello there!\n" +
        $"You're connected to {client.Address} from {client.ClientAddress}.\n" +
        $"The current state of the PLC is: {client.ReadState().AdsState}.\n" +
        "\nPress any key to exit...\n"
    );
    Console.ReadKey(true);
}
```

This code should produce output similar to the following when run:
```console
Hello there!
You're connected to 192.168.137.1.1.1:851 from 192.168.137.1.1.1:XXXXX.
The current state of the PLC is: Run.

Press any key to exit...
```

This code connects to the PLC on the local AMS Net ID with port `851`. It displays the connection details, including the PLC state, which should confirm a successful connection if everything is set up correctly. Pressing any key will then terminate the application. 

This setup gives you a basic foundation for communicating with your PLC through ADS in .NET.