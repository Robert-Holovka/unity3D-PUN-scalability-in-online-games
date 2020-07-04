# unity3D-PUN-scalability-in-online-games

An online multiplayer FPS game made with Unity3D game engine and PUN (Photon Unity Networking) package. <br>
This game shows some of the techniques that are usually used to achieve:
 - Scalability,
 - Consistency and
 - To reduce the impact of the network latency and packet loss.
**Implemented techniques: consistency management, dead reckoning, interpolation, network culling and world division.**

<h3>1. Matchmaking</h3>
Each player has the option to create a new room or join existing one. <br>
Only the master client (room creator) can start the game, scene loading for other players in the same room is automatically synced.
<br>

| <img  alt="Login" src="./Readme%20Resources/image/login.png"> | <img  alt="Matchmaking Options" src="./Readme%20Resources/image/matchmaking.png"> |
| :--------------------------------------------------------------: | :--------------------------------------------------------------: |
| <img  alt="New Room" src="./Readme%20Resources/image/new_room.png"> | <img  alt="Room Info" src="./Readme%20Resources/image/in_room.png"> |
| <img  alt="Rooms List" src="./Readme%20Resources/image/room_list.png"> | <img  alt="Loading" src="./Readme%20Resources/image/loading.png"> |

<h3>2. Combat</h3>

|||
| :--------------------------------------------------------------: | :--------------------------------------------------------------: |
| ![Combat](Readme%20Resources/gif/combat.gif) | <img  alt="Respawning" src="./Readme%20Resources/image/respawning.png"> |

<h3>3. Options</h3>

|||
| :--------------------------------------------------------------: | :--------------------------------------------------------------: |
| <img  alt="Pause Menu" src="./Readme%20Resources/image/pause_menu.png"> | <img  alt="Game Options" src="./Readme%20Resources/image/in_game_options.png"> |

<h3>4. Player UI</h3>
<img  alt="Player UI" src="./Readme%20Resources/image/player_ui.png">

<h3>5. Network Simulation</h3>
<img  alt="Network Simulation" src="./Readme%20Resources/image/network_simulation.png">

<h3>6. Synchronization</h3>
<img  alt="Synchronization" src="./Readme%20Resources/image/sync_options.png">

| Without interpolation | With interpola |
| :--------------------------------------------------------------: | :--------------------------------------------------------------: |
| ![Without Interpolation](Readme%20Resources/gif/without_interpolation.gif) | ![With Interpolation](Readme%20Resources/gif/with_interpolation.gif) |

| packet loss at 30% | packet loss at 30% + Dead Reckoning |
| :--------------------------------------------------------------: | :--------------------------------------------------------------: |
| ![Without Extrapolation](Readme%20Resources/gif/network_simulation.gif) | ![With Extrapolation](Readme%20Resources/gif/extrapolation.gif) |

<h3>7. Network Culling</h3>

|||
| :--------------------------------------------------------------: | :--------------------------------------------------------------: |
| <img  alt="Network Culling" src="./Readme%20Resources/image/network_culling.png"> | <img  alt="Map" src="./Readme%20Resources/image/map.png"> |


| Network Culling on, player2 in zone 2 | Dynamic FOV |
| :--------------------------------------------------------------: | :--------------------------------------------------------------: |
| ![Network Culling](Readme%20Resources/gif/network_culling.gif) | ![Dynamic FOV](Readme%20Resources/gif/dynamic_FOV.gif) |

<h3>8. Consistency Management</h3>

<h3>9. Controls</h3>

|           Input          |     Action    |
|:------------------------:|:-------------:|
| W, A, S, D or Arrow Keys |    Movement   |
|      Mouse Rotation      | Look Rotation |
|        Left Click        |    Shooting   |
|           Space          |    Running    |
|          Escape          |   Pause Menu  |
