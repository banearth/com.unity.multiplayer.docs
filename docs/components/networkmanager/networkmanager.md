---
id: networkmanager
title: NetworkManager
---

The **NetworkManager** is a required, central component for all of your project's **Netcode for GameObjects (NGO)**-related settings.

## NetworkManager Inspector Properties

| Property | Description |
| --- | --- |
| **LogLevel** | Sets the network logging level |
| **PlayerPrefab** |  When a Prefab is assigned, the Prefab will be instantiated as the player object and assigned to the newly connected and authorized client. |
| **NetworkPrefabs** | Where you register your network prefabs.  You can also create a single network Prefab override per registered network Prefab here. |
| **Protocol Version** | Set this value to help distinguish between builds when the most current build has new assets that can cause issues with older builds connecting. |
| **Network Transport** | Where your network specific settings and transport type is set.  This field accepts any INetworkTransport implementation.  However, unless you have unique transport specific needs UnityTransport is the recommended transport to use with Netcode for GameObjects. |
| **Tick Rate** | This value controls the network tick update rate. |
| **Ensure Network Variable Length Safety** | (Increases cpu processing and bandwidth) When this property is checked, Netcode for GameObjects will prevent user code from writing past the boundaries of a NetworkVariable. |
| **Connection Approval** | This enables [connection approval](../basics/connection-approval.md) when this is checked and the `NetworkManager.ConnectionApprovalCallback` is assigned. |
| **Client Connection Buffer Timeout** | This value sets the amount of time that has to pass for a connecting client to complete the connection approval process.  If the time specified is exceeded the connecting client will be disconnected. |
| **Force Same Prefabs** | When checked it will always verify that connecting clients have the same registered network prefabs as the server.  When not checked, Netcode for GameObjects will ignore any differences. |
| **Recycle Network Ids** | When checked this will re-use previously assigned `NetworkObject.NetworkObjectIds` after the specified period of time. |
| **Network Id Recycle Delay** | The time it takes for a previously assigned but currently unassigned identifier to be available for use. |
| **Enable Scene Management** | When checked Netcode for GameObjects will handle scene management and client synchronization for you.  When not checked, users will have to create their own scene management scripts and handle client synchronization. |
| **Load Scene Time Out** | When Enable Scene Management is checked, this specifies the period of time the `NetworkSceneManager` will wait while a scene is being loaded asynchronously before `NetworkSceneManager` considers the load/unload scene event to have failed/timed out. |

## NetworkManager Sub-Systems

The **NetworkManager** also references other NGO-related management systems. These sub-systems instantiate after the **NetworkManager** starts (`NetworkManager.IsListening == true`). 

| Sub-System | Use|
| --- | --- |
| [NetworkManager.PrefabHandler](../advanced-topics/object-pooling.md) | This provides access to the NetworkPrefabHandler that is used for NetworkObject pools and to have more control overriding network prefabs. |
| [NetworkManager.SceneManager](../basics/scenemanagement/using-networkscenemanager.md) | When scene management is enabled, this is used to load and unload scenes, register for scene events, and other scene management related actions. |
| [NetworkManager.SpawnManager](../basics/object-spawning.md) | This handles NetworkObject spawn related functionality. |
| [NetworkManager.NetworkTimeSystem](../advanced-topics/networktime-ticks.md) | a synchronized time that can be used to handle issues with latency between a client and the server. |
| [NetworkManager.NetworkTickSystem](../advanced-topics/networktime-ticks#network-ticks.md) | Use this to adjust the frequency of when NetworkVariables are updated. |
| [NetworkManager.CustomMessagingManager](../advanced-topics/message-system/custom-messages.md) | Use this system to create and send custom messages. |

## Using the NetworkManager
- Starting a Server, Host, or Client
- Connecting
- Disconnecting and Stopping