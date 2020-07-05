# unity3D-PUN-scalability-in-online-games

An online multiplayer FPS game made with Unity3D game engine and PUN (Photon Unity Networking) package. <br>
This game shows some of the techniques that are usually used to achieve:
 - Scalability,
 - Consistency and
 - To reduce the impact of the network latency and packet loss. <br>
 
**Implemented techniques: consistency management, dead reckoning, interpolation, network culling and world division.**

<h3>1. Matchmaking</h3>
Every player has the option to create a new room or join existing one. <br>
Only the master client (room creator) can start the game, scene loading for other players in the same room is automatically synced. <br>
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
When turned on, this option allows the player to simulate network conditions. <br>
Player can add additional ping and increase packet loss chance. <br>
<br>
<img  alt="Network Simulation" src="./Readme%20Resources/image/network_simulation.png">

<h3>6. Synchronization</h3>
<img  alt="Synchronization" src="./Readme%20Resources/image/sync_options.png">

Each player sends to the server 10 times per second a package that contains information about his position and rotation.
Server distributes that package to other players. <br>
That amount of data is not enough to present smooth movement for the avatars of other players. <br>
Increasing the serialization rate would fix that problem, but the server has a potential to become a bottleneck. <br>
Interpolation is used to retain smooth movement and rotation without sending too much data. <br>
That technique locally generates additional waypoints between last 2 known states. <br>

| Movement without interpolation | Movement with interpolation |
| :--------------------------------------------------------------: | :--------------------------------------------------------------: |
| ![Without Interpolation](Readme%20Resources/gif/without_interpolation.gif) | ![With Interpolation](Readme%20Resources/gif/with_interpolation.gif) |

Interpolation can reduce the amount of data that is sent over the network, but it does not solve the problems caused by an unstable network. <br>
When a package is lost or arrives lately, movement of an avatar will still look jittery. <br>
Dead reckoning can hide those problems successfully. <br>
Since the serialization rate in this game is 10, we can expect a new package every 100 ms. <br>
After that amount of time passes without getting the new package, extrapolation of data will kick in. <br>
From the last 2 known states we can extract avatar's velocity and predict its current position. <br>

| Interpolation + Packet Loss (30%) |Interpolation + Packet Loss (30%) + Dead Reckoning|
| :--------------------------------------------------------------: | :--------------------------------------------------------------: |
| ![Without Extrapolation](Readme%20Resources/gif/network_simulation.gif) | ![With Extrapolation](Readme%20Resources/gif/extrapolation.gif) |

<h3>7. Network Culling & World Division</h3>

|||
| :--------------------------------------------------------------: | :--------------------------------------------------------------: |
| <img  alt="Network Culling" src="./Readme%20Resources/image/network_culling.png"> | <img  alt="Map" src="./Readme%20Resources/image/map.png"> |


Since PUN package does not allow server-side scripts, the world is divided into 16 zones in order to achieve network culling. 
By default, all packages are broadcasted. <br>
When some player turns on the network culling option he will receive only relevant data. <br>
The player only cares about updates in the current zone and neighbour zones. <br>
Each player has his own field of view presented by the circle on the map. <br>
When that circle touches the border of another zone, the player will become neighbour with that zone. <br>

| Player2 in zone 2, Player1 becomes neighbour with zone 2, number of incoming messages changes|
| :--------------------------------------------------------------: |
| ![Network Culling](Readme%20Resources/gif/network_culling.gif) |

Field of view represents how far players can see. <br>
If a player has turned on the Dynamic FOV option, his field of view will shrink to the shooting range if certain conditions are met. 
This will only happen if the number of incoming messages becomes too high. <br>
In that particular moment, player will mark his and neighbour zones as critical and the circle (FOV) will stay shrunk until he enters some
zone that is not on that list. <br>
Player can be subscribed up to 4 zones (his own + 3 neighbours) <br>
In those situations it is expected that the network will be overloaded, and by shrinking the circle player can reduce the 
chance of becoming the neighbour with other zones. <br>

| Dynamic FOV, Player2 in zone 2, Player1 runs through different zones |
| :--------------------------------------------------------------: |
| ![Dynamic FOV](Readme%20Resources/gif/dynamic_fov.gif) |

<h3>8. Consistency Management</h3>

Players can pick up bullets in the scene. When one player picks up the bullets he is responsible to notify all other players
about that action so they can remove that object their local copy of the game. <br>
Suppose we have 2 players, both players have the same latency 100 ms. Player1 picks up the bullets, after 20 ms player2 does the same thing. <br>
The end result of those actions will be inconsistent behavoiur because both players will pick up the same object. <br>
Because of the latency, Player1 notification arrives too late.
<br>
In this game, when one player picks up the object he will firstly send request to the other players. <br>
That request contains name of object and timestamp. Player then waits for responses from all other players, and if
all those responses are positive he will get the object. <br>
Player with the smallest timestamp wins the object.

<h3>9. Controls</h3>

|           Input          |     Action    |
|:------------------------:|:-------------:|
| W, A, S, D or Arrow Keys |    Movement   |T
|      Mouse Rotation      | Look Rotation |
|        Left Click        |    Shooting   |
|           Space          |    Running    |
|          Escape          |   Pause Menu  |
