# RacecarNeo-Simulator WITH FIXED AR TAGS FOR GP2020 and TT2020

###! This is a fork of the [MITRacecarNeo RacecarNeo-Simulator](https://github.com/MITRacecarNeo/RacecarNeo-Simulator) !###

Thanks to the easy to change Unity Project uploaded in the [repo](https://github.com/MITRacecarNeo/RacecarNeo-Simulator) I have changed the ar tags to work correctly with ID and Orientation in the GrandPrix 2020 and in the TimeTrial 2020 if you have any issues please inform me.

Releases:
  - [WINDOWS](https://github.com/JohnThyWizard/RacecarNeo-Simulator/releases/download/v2.6.0AR/WINDOWS.zip)
  - [LINUX](https://github.com/JohnThyWizard/RacecarNeo-Simulator/releases/download/v2.6.0AR/LINUX.zip)
  - [MACOS](https://github.com/JohnThyWizard/RacecarNeo-Simulator/releases/download/v2.6.0AR/MACOS.app.zip)

I suggest that you put them in a different folder than the racecar default to avoid conflicts<br/><br/>

Few things to take in mind if you want to build this instead of using the Releases
- Use Unity version 2019.4.40f1 (You will get prompted to use this specific version if you try and open the project from Unity Hub)
- If you want to build press File --> Build Settings then select your operating system and press Build or Build and run and select an empyu folder
- Or alternatively you can just download the fixed files that are located in `Assets/Scenes/GrandPrixFiles/` which are `GrandPrix.unity` and `TimeTrial.unity` and paste them in the original repo and then build it<br/><br/>
#############################################################################
This original README from the [repo](https://github.com/MITRacecarNeo/RacecarNeo-Simulator) is the following, and have a great Day
#############################################################################

_The MIT Beaver Works RACECAR simulation environment_

You can learn more about RacecarSim and download the current version [here](https://sites.mit.edu/mit-racecar/resources/).

## Changelog
- **v2.6.0**: Added Mini Grand Prix Spring 2025 Final Challenge [5/17/2025]
- **v2.5.2**: Added Micro Grand Prix 2025 Final Challenge, fixed bugs in Lab E - Stoplight Challenge [3/24/2025]
- **v2.5.1**: Fixed incorrect StartPos offset in Lab D autograder (Driving in Mazes) [2/7/2025]
- **v2.5.0**: Added Community Levels from Greece - Mathisi RACECAR. Time Trial 2025 & Grand Prix 2025 added. [1/17/2025]
- **v2.4.2**: Fixed incorrect scoring in autograder for Labs E-I, updated app logo [11/23/2024]
- **v2.4.1**: Fixed minor bug with Lab E lvl.5 autograder which had incorrect startpos [11/16/2024]
- **v2.4.0**: Added Mini Grand Prix Fall 2024, minor visual changes
- **v2.3.0**: Updated Neo Labs (Lab D - Lab I) with autograder
- **v2.2.0**: Grand Prix 2024 from BWSI RACECAR Summer 2024
- **v2.1.0**: Mini Grand Prix Spring 2024: Speical Course Update
- **v2.0.1**: Minor bug fixes (IMU, Physics)
- **v2.0.0**: Update from MN -> Neo Curriculum, Neo Labs updated, course organization implemented





## Getting Started

RacecarSim is built with Unity and C#. Before you begin, you will need to download Unity [here](https://unity3d.com/get-unity/download). Select the option that says "Download Unity Hub".

Open Unity Hub and install the latest version of Unity in the "Installs" page. In the "Projects" page, click the "Add" button and select the `RacecarSim` directory located at the root of this repository. This will import the RacecarSim project into Unity.

If you installed [Microsoft Visual Studio](https://visualstudio.microsoft.com/) with Unity, you should see a file called `RacecarSim.sln` in `RacecarSim` directory. This is the Visual Studio Solution for the project. Open this in Visual Studio to edit the RacecarSim C# scripts.

Inside of the Unity Editor, you can build RacecarSim from `File -> Build Settings`.

## Repository Contents

You will find the following directories inside of `RacecarSim/Assets`:

- **2D**: Images, including textures, HUD content, etc.
- **Fonts**: Non-standard fonts for use in the HUD and menus.
- **Materials**: Render materials and physics materials.
- **Models**: 3D models.
- **Prefabs**: Pre-made gameobjects containing a hierarchy of models, C# scripts, colliders, and other game elements.
- **Scenes**: The "levels" of the simulation. The "Main" scene provides the main menu, and the other scenes correspond to labs of the [RACECAR course](https://mitll-racecar-mn.readthedocs.io/en/latest/curriculum.html).
- **Scripts**: The C# scripts which control the simulator.
- **Textures**: Render textures to which cameras render, such as the textures to which the color and depth camera render.

Inside of the `RacecarSim/Assets/Scripts` directory, you will find the following organization:

- **(root level)**: Miscellaneous scripts.
- **LevelManagement**: Simple scripts which control objects in a level, such as the finish line.
- **Racecar**: Modules which control the RACECAR and all of its hardware.
- **UI**: Scripts controlling the 2D user interface, including the HUD and menus.

### Racecar Modules

The [`Racecar`](https://github.com/MITLLRacecar/Simulation/tree/master/RacecarSim/Assets/Scripts/Racecar) class (see `RacecarSim/Assets/Scripts/Racecar/Racecar.cs`) controls the RACECAR and roughly mirrors the structure of the [`racecar_core` library](https://github.com/MITLLRacecar/Student/tree/master/library) used by the real RACECAR. The following sub-modules each handle a particular aspect of the car's hardware:

- **`CameraModule`**: Models the color and depth imaging capabilities of the car's camera. The Racecar prefab includes a color camera which renders to a dedicated render texture. `CameraModule` pulls this texture from the GPU when requested. The depth image is created by performing a ray cast at "each pixel" , although this is done at a lower resolution to preserve performance. Both of these operations are quite expensive, so color and depth images are cached per frame.
- **`Controller`**: Handles input from the keyboard and Xbox controller. Xbox controllers are mapped differently per operating system, so this module includes a compilation constant which must be set based on the operating system.
- **`Drive`:** Handles the motors on the car, namely the speed of the rear wheels and the angle of the front wheels. At the moment, this physics is implemented with the Unity [`WheelColider` class](https://docs.unity3d.com/Manual/class-WheelCollider.html).
- **`Lidar`**: Models the 2D LIDAR onboard the car. The LIDAR rotates on top of the car and collects samples through ray casts.
- **`PhysicsModule`**: Models the car's IMU. Angular velocity is taken directly from the car's rigidbody. Linear acceleration is calculated as change in linear velocity, taken from the car's rigidbody.

_The `CameraModule` and `PhysicsModule` are named as such to avoid conflicting with the `Camera` and `Physics` classes provided by Unity._

## Python Interface

RacecarSim communicates with a user's Python program using a custom protocol sent over a UDP connection. This is all handled in the [`PythonInterface`](https://github.com/MITLLRacecar/Simulation/blob/master/RacecarSim/Assets/Scripts/PythonInterface.cs) class (see `RacecarSim/Assets/Scripts/PythonInterface.cs`). On the Python end, this communication is handled by [`racecar_core_sim.py`](https://github.com/MITLLRacecar/Student/blob/master/library/simulation/racecar_core_sim.py) in the `racecar_core` library.

`PythonInterface` has two UdpClients operating on fixed ports. The async client operates on a separate thread and handles asynchronous data requests from Jupyter and connection requests from user programs. Once connection has been established, RacecarSim communicates with the user's program through the synchronous client, which operates on the main thread.

In `PythonInterface`, the `Header` enum declares the reserved messages used in the communication protocol.

### Synchronous Communication Protocol (used for Python scripts)

1. The Python program periodically sends `connect` messages to the RacecarSim async client.
2. The async client receives a `connect` message, records the port of the Python program, and responds with a `connect` message. From now on, communication will occur through the synchronous client.
3. When the user enters "User Program" mode in RacecarSim, the sync client sends a `unity_start` message to Python and blocks to await a response from Python.
4. Python runs the user's `start` function. If the `start` function calls any of the `racecar_core` APIs, these API calls are passed back to the sync client with the corresponding header. For example, if the user calls `rc.camera.get_color_image()`, Python will send the `camera_get_color_image` message to the sync client. RacecarSim then responds with the requested data.
5. Once the Python program finishes executing the user's `start` function, it sends back a `python_finished` message. RacecarSim stops blocking.
6. Once per frame, the sync client sends a `unity_update` message to Python and blocks to await a response from Python.
7. Python runs the user's `update` function. Once again, it passes API calls back to the sync client.
8. When Python finishes executing the user's `update` function, it sends back a `python_finished` message. RacecarSim stops blocking.
9. This process repeats until the user exits or restarts the level in RacecarSim. In either of these cases, the sync client sends a `unity_exit` message to Python.
10. Upon receiving the `unity_exit` message, the Python program closes.

Note that after a connection is established, this protocol is **completely synchronous**: both systems are synchronized to the Unity "update" clock. RacecarSim will always block the main thread until receiving a message from Python (or until a timeout occurs).

### Asynchronous Communication Protocol (used for Jupyter Notebook)

1. Jupyter Notebook send the async client a request for data from a particular sensor (such as a `camera_get_color_image`, `camera_get_depth_image`, or `lidar_get_samples` message).
2. The async client processes receives the request and tells the corresponding sensor on the main thread that data has been requested.
3. In the subsequent Update frame, the main thread updates the data from that sensor.
4. After a short waiting period, the async client reads the updated data and returns it to Jupyter Notebook.

### Send Fragmented

Color images are too large to send in a single UDP packet so are split across several UDP packets. After each packet, RacecarSim blocks until Python responds with the `python_send_next` message, which indicates that it is ready to receive the next fragment.

## Modeling Error

When the "Realism" option is enabled via the settings, a realistic amount of random gaussian error is added to the following sensors:

- depth camera
- LIDAR
- IMU

The error rate is based on the data sheets for these sensors.
