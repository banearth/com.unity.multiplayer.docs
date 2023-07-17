---
id: nm_connect
title: Connect
---

## Connecting

When Starting a Client, the `NetworkManager` uses the IP and the Port provided in your `Transport` component for connecting. While you can set the IP address in the editor, many times you might want to be able to set the IP address and port during runtime.

The below examples use [Unity Transport](../../../transport/current/about) to show a few ways you can gain access to the `UnityTransport` component via the `NetworkManager.Singleton` to configure your project's network settings programmatically: 

If you are only setting the IP address and port number, then you can use the `UnityTransport.SetConnectionData` method:
```csharp
NetworkManager.Singleton.GetComponent<UnityTransport>().SetConnectionData(
    "127.0.0.1",  // The IP address is a string
    (ushort)12345 // The port number is an unsigned short
);
```

If you are using the same code block to configure both your server and your client and you want to configure your server to listen to all IP addresses assigned to it, then you can also pass a 'listen address' of "0.0.0.0" to the `SetConnectionData` method, like so:
```csharp
NetworkManager.Singleton.GetComponent<UnityTransport>().SetConnectionData(
    "127.0.0.1",  // The IP address is a string
    (ushort)12345, // The port number is an unsigned short
    "0.0.0.0" // The server listen address is a string.
);
```

:::note
Using an IP address of 0.0.0.0 for the server listen address will make a server or host listen on all IP addresses assigned to the local system. This can be particularly helpful if you are testing a client instance on the same system as well as one or more client instances connecting from other systems on your local area network. Another scenario is while developing and debugging you might sometimes test local client instances on the same system and sometimes test client instances running on external systems.  
:::

It is possible to access the current connection data at runtime, via `NetworkManager.Singleton.GetComponent<UnityTransport>().ConnectionData`. This will return a [`ConnectionAddressData` **struct**](https://github.com/Unity-Technologies/com.unity.netcode.gameobjects/blob/11922a0bc100a1615c541aa7298c47d253b74937/com.unity.netcode.gameobjects/Runtime/Transports/UTP/UnityTransport.cs#L239-L286), holding this info. You are strongly advised to use the `SetConnectionData` method to update this info.

If you are using Unity Relay to handle connections, however, **don't use `SetConnectionData`**. The host should call [`SetHostRelayData`](https://github.com/Unity-Technologies/com.unity.netcode.gameobjects/blob/11922a0bc100a1615c541aa7298c47d253b74937/com.unity.netcode.gameobjects/Runtime/Transports/UTP/UnityTransport.cs#L575), and clients should call [`SetClientRelayData`](https://github.com/Unity-Technologies/com.unity.netcode.gameobjects/blob/11922a0bc100a1615c541aa7298c47d253b74937/com.unity.netcode.gameobjects/Runtime/Transports/UTP/UnityTransport.cs#L588). Attempting to join a **Relay**-hosted game via entering IP/port number (via `SetConnectionData`) **won't work**.


[More information about Netcode for GameObjects Transports](../advanced-topics/transports.md)