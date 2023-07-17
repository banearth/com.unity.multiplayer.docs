---
id: nm_start
title: Start the NetworkManager as a Server, Host, or Client
---

Sending messages requires a running server that listens for connections with at least one client (a server can send RPCs to itself when running as a host). You must invoke one of the following methods to start your [**NetworkManager**](./networkmanager.md) as a server, host, or client:

To start the **NetworkManager** as a server:

```csharp
NetworkManager.Singleton.StartServer(); 
```

To start the **NetworkManager** as a client:

```csharp
NetworkManager.Singleton.StartClient();
```

To start the **NetworkManager** as both a server and a client:

```csharp
NetworkManager.Singleton.StartHost();
```

:::important
Starting a **NetworkManager** within a **NetworkBehaviour**'s `Awake` method as this can lead to undesirable results depending upon your project's settings!
:::

:::note
 The **NetworkManager** can spawn a client-owned Player Object

 When starting a Server or joining an already started session as client, the **NetworkManager** can spawn a "Player Object" belonging to the client.

 For more information about player prefabs see:
 - [NetworkObject Player Prefab Documentation](../basics/networkobject.md#player-objects)
 - [Connection Approval](../basics/connection-approval)  
:::