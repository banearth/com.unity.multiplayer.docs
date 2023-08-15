---
id: networkmanager
title: NetworkManager
---

`NetworkManager` is a required component in Netcode for GameObjects (NGO) that contains all of your project's netcode-related settings, properties, and sub-systems. It is the netcode control center for your netcode-enable project.

## `NetworkManager` Inspector window properties

| Property | Description |
|---|---|
| LogLevel | Sets the amount of detail you want in your network-related logs |
| PlayerPrefab | Spawns a Prefab as the player object assigned to the newly connected and authorized client |
| NetworkPrefabs | Register your network prefabs and override a single network prefab per registed network prefab here |
| Protocol Version | Set this value to differentiate between different builds |
| Network Transport | Set your transport type and network-specific settings. This field accepts any INetworkTransport implementation; however, we recommend using Unity Transport Protocol (UTP) with NGO. |
| Tick Rate | Controls how often networking updates occur |
| Ensure Network Variable Length Safety | Prevents overwriting variable boundaries. When enabled, it does increase CPU processing and bandwidth. |
| Connection Approval |
| Client Connection Buffer Timeout |
| Force Same Prefabs |
| Recycle Network Ids |
| Network Id Recycle Delay |
| Enable Scene Management |
| Load Scene Time Out |

LogLevel: Sets how much detail you want in network-related logs.
PlayerPrefab: When connected, a player's character is created from this prefab.
NetworkPrefabs: Register networked objects here. You can override prefab settings.
Protocol Version: Helps differentiate between different builds.
Network Transport: Settings for networking. UnityTransport is recommended.
Tick Rate: How often networking updates occur.
Ensure Network Variable Length Safety: Prevents overwriting variable boundaries.
Connection Approval: Approve connections before they're established.
Client Connection Buffer Timeout: Time for client connection approval.
Force Same Prefabs: Ensures consistent networked objects.
Recycle Network Ids: Reuse network object IDs.
Network Id Recycle Delay: Delay before reusing IDs.
Enable Scene Management: Handles scene and client synchronization.
Load Scene Time Out: Time before considering scene load/unload failed.