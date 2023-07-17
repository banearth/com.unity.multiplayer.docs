---
id: nm_disconnect
title: Disconnect
---

## Disconnecting and Shutting Down

Disconnecting is rather simple, but you can't use/access any subsystems (that is, `NetworkSceneManager`) once the `NetworkManager` is stopped because they will no longer be available.  For client, host, or server modes, you only need to call the `NetworkManager.Shutdown` method as it will disconnect while shutting down.  

:::info
When no network session is active and the `NetworkManager` has been shutdown, you will need to use `UnityEngine.SceneManagement` to load any non-network session related scene. 
::: 

```csharp
public void Disconnect()
{
    NetworkManager.Singleton.Shutdown();
    // At this point we must use the UnityEngine's SceneManager to switch back to the MainMenu
    UnityEngine.SceneManagement.SceneManager.LoadScene("MainMenu");
}
```

## Disconnecting Clients (Server Only)

At times you might need to disconnect a client for various reasons without shutting down the server.  To do this, you can call the `NetworkManager.DisconnectClient` method while passing the identifier of the client you wish to disconnect as the only parameter.  The client identifier can be found within:
- The `NetworkManager.ConnectedClients` dictionary that uses the client identifier as a key and the value as the [`NetworkClient`](../api/Unity.Netcode.NetworkClient.md).
- As a read only list of `NetworkClients`  via the `NetworkManager.ConnectedClientsList`.
- A full list of all connected client identifiers can be accessed via `NetworkManager.ConnectedClientsIds`.
- The client identifier is passed as a parameter to all subscribers of the `NetworkManager.OnClientConnected` event.
- The player's `NetworkObject` has the `NetworkObject.OwnerClientId` property.

:::tip
One way to get a player's primary `NetworkObject` is via `NetworkClient.PlayerObject`.
:::

```csharp
void DisconnectPlayer(NetworkObject player)
{   
    // Note: If a client invokes this method, it will throw an exception.
    NetworkManager.DisconnectClient(player.OwnerClientId);
}
```

### Client Disconnection Notifications

Both the client and the server can subscribe to the `NetworkManager.OnClientDisconnectCallback` event to be notified when a client is disconnected. Client disconnect notifications are "relative" to who is receiving the notification.

**There are two general "disconnection" categories:**
- **Logical**: Custom server side code (code you might have written for your project) invokes `NetworkManager.DisconnectClient`.
  - Example: A host player might eject a player or a player becomes "inactive" for too long.
- **Network Interruption**: The transport detects there is no longer a valid network connection.

**When disconnect notifications are triggered:**
- Clients are notified when they're disconnected by the server.
- The server is notified if the client side disconnects (that is, a player intentionally exits a game session)
- Both the server and clients are notified when their network connection is unexpectedly disconnected (network interruption)

**Scenarios where the disconnect notification won't be triggered**:
- When a server "logically" disconnects a client.
  - _Reason: The server already knows the client is disconnected._
- When a client "logically" disconnects itself.
  - _Reason: The client already knows that it's disconnected._

### Connection Notification Manager Example
Below is one example of how you can provide client connect and disconnect notifications to any type of NetworkBehaviour or MonoBehaviour derived component. 

:::important
The `ConnectionNotificationManager` example below should only be attached to the same GameObject as `NetworkManager` to assure it persists as long as the `NetworkManager.Singleton` instance.
:::


```csharp
using System;
using UnityEngine;
using Unity.Netcode;

/// <summary>
/// Only attach this example component to the NetworkManager GameObject.
/// This will provide you with a single location to register for client 
/// connect and disconnect events.  
/// </summary>
public class ConnectionNotificationManager : MonoBehaviour
{
    public static ConnectionNotificationManager Singleton { get; internal set; }

    public enum ConnectionStatus
    {
        Connected,
        Disconnected
    }

    /// <summary>
    /// This action is invoked whenever a client connects or disconnects from the game.
    ///   The first parameter is the ID of the client (ulong).
    ///   The second parameter is whether that client is connecting or disconnecting.
    /// </summary>
    public event Action<ulong, ConnectionStatus> OnClientConnectionNotification;

    private void Awake()
    {
        if (Singleton != null)
        {
            // As long as you aren't creating multiple NetworkManager instances, throw an exception.
            // (***the current position of the callstack will stop here***)
            throw new Exception($"Detected more than one instance of {nameof(ConnectionNotificationManager)}! " +
                $"Do you have more than one component attached to a {nameof(GameObject)}");
        }
        Singleton = this;
    }

    private void Start()
    {
        if (Singleton != this){
            return; // so things don't get even more broken if this is a duplicate >:(
        }
        
        if (NetworkManager.Singleton == null)
        {
            // Can't listen to something that doesn't exist >:(
            throw new Exception($"There is no {nameof(NetworkManager)} for the {nameof(ConnectionNotificationManager)} to do stuff with! " + 
                $"Please add a {nameof(NetworkManager)} to the scene.");
        }
        
        NetworkManager.Singleton.OnClientConnectedCallback += OnClientConnectedCallback;
        NetworkManager.Singleton.OnClientDisconnectCallback += OnClientDisconnectCallback;
    }

    private void OnDestroy()
    {
        // Since the NetworkManager can potentially be destroyed before this component, only 
        // remove the subscriptions if that singleton still exists.
        if (NetworkManager.Singleton != null)
        {
            NetworkManager.Singleton.OnClientConnectedCallback -= OnClientConnectedCallback;
            NetworkManager.Singleton.OnClientDisconnectCallback -= OnClientDisconnectCallback;
        }
    }

    private void OnClientConnectedCallback(ulong clientId)
    {
        OnClientConnectionNotification?.Invoke(clientId, ConnectionStatus.Connected);
    }

    private void OnClientDisconnectCallback(ulong clientId)
    {
        OnClientConnectionNotification?.Invoke(clientId, ConnectionStatus.Disconnected);
    }
}
```
