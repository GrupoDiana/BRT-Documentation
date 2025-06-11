# BRT OSC API for Unity - Integration Package

This package enables the integration of a Unity scene with applications from the BRT library ([BRT Application](../applications/berta-renderer/index.md)) using OSC commands. It can be download from our [repository](https://github.com/GrupoDiana/BRTLibrary/releases).

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/Unity_OSCPackage_communication.png" alt="OSC communication" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">OSC communication between Unity and BRT Application</p>
</div>

## Package Contents

### `BRTManager.cs`
A dual-purpose class designed to:

- Serve as an example of how to use the package.
- It can be used and extended by anyone, as it implements some important OSC commands.  

### `BRTCommands.cs`
This class handles the implementation, sending, and receiving of OSC commands.

**Note:** This class is still under development. Bugs may be present, and not all commands from the BRT interface are currently implemented. For more details about the available OSC commands, refer to the [BRT OSC Documentation](../osc/index.md).

### `OSC.cs`
A third-party library that implements the OSC protocol. Configure the IP and port for the remote application and the listening port for the Unity app in this script.

For the scripts described above to work, they must be added to the same object, as illustrated in the following image.

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/scripts.png" alt="Scripts impoted in Unity." style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Scripts impoted in Unity.</p>
</div>

<hr style="border:1px solid gray">

### `BRTAudioListener.cs`
This script sends the listener's position and orientation to the BRT renderer via OSC commands.

To use this:

- Attach the script to your scene's camera object, or Instantiate our prefab (`CenterEarAnchor.prefab`) in the scene, as a child of a camera object.
- Enter the listener ID as defined in the BeRTA Renderer configuration file, see [section](../applications/settingsFile.md).

### `CenterEarAnchor.prefab`
A prefab designed to be placed under the "center eye" object in a virtual reality scene. Its purpose is to apply an offset, positioning it virtually at the center of the listener's head. This prefab also includes the `BRTAudioListener.cs` script.

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/unity_AudioListener.png" alt="CenterEarAnchor prefab in the Unity inspector" style="display: block; margin: 0 auto;" width="500">
    <p style="text-align: center;">CenterEarAnchor prefab in the Unity inspector.</p>
</div>

<hr style="border:1px solid gray">

### `BRTAudioSource.cs`
This script is designed to communicate the position and orientation of a virtual sound source. It must be installed on objects representing sound sources in the Unity virtual world.  These values are sent every time they change. It will also load the source in BeRTA Renderer at startup.

To use this:

- Attach the script to your scene's objects
- Assign a unique ID
- Enter the full name of the audio file that the BeRTA Renderer should load.

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/unity_AudioSource.png" alt="BRTAudioSource.cs" style="display: block; margin: 0 auto;" width="350"> <p style="text-align: center;">BRTAudioSource.cs</p>
</div>

<hr style="border:1px solid gray">

## 🛠️ Getting Started: Basic Unity Setup with One Source and One Listener

This guide explains how to set up a basic Unity scene that communicates with **BeRTA** using our Unity integration package. The setup includes **one static audio source** and **one listener**.

---

### ✅ Step-by-Step Guide

#### 1. Import the Unity Package
Import the Unity package provided for BeRTA integration into your Unity project.

---

#### 2. Create the Core BRT GameObject
- In your Unity scene, create an **empty GameObject**.
- Add the following scripts as components:
  - `BRTManager.cs`
  - `BRTCommands.cs`
  - `OSC.cs`
    - Open the `OSC.cs` component and configure:
    - The **remote IP and port** to match the BeRTA application.
    - The **local listening port** for incoming OSC messages.

---

#### 3. Create an Audio Source and Assign BRTCommands
- Create a new **GameObject** to act as your **audio source**.
- In the script attached to this source (e.g., `BRTAudioSource.cs`), **locate the `BRTCommands` field and drag the GameObject containing `BRTCommands.cs` into it**.
- In the audio file field, enter the absolute path to the .wav file you want BeRTA to play.

---

#### 4. Configure the Listener and Assign BRTCommands
- Drag the **`CenterEarAnchor.prefab`** into your scene as a child of the **Main Camera**.
- This prefab includes the `BRTAudioListener.cs` script and positions the listener at the center of the user's head.
- In the `BRTAudioListener.cs` script, **locate the `BRTCommands` field and drag the GameObject containing `BRTCommands.cs` into it**.

---

#### 5. Launch BeRTA
- Open and run the **BeRTA application**.
- With Unity and BeRTA running, the **OSC connection will be established**, and audio will be rendered in real time.

---

### 📌 Notes

- `BRTManager.cs`, `BRTCommands.cs`, and `OSC.cs` must be attached to the **same GameObject**.
- Both the **audio source** and the **listener prefab** must reference this GameObject through their `BRTCommands` field.
- Ensure your network settings and firewall allow OSC communication.

---

## License

Except for the OSC.cs class, which was not developed by us, all other scripts in this package are distributed under the MIT license.
