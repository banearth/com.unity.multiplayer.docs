---
id: object-spawning
title: Object Spawning
---

In Unity, you typically create a new game object using the `Instantiate` function. However, this only creates the game object on the local machine. For multiplayer games, you want to create a game object that synchronizes between all clients by the server. In Netcode for GameObjects (NGO), this is called spawning.

