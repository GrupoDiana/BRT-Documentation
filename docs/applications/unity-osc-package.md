# Unity-BRT Integration Package

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

### `BRTAudioListener.cs`
This script sends the listener's position and orientation to the BRT renderer via OSC commands.

To use this:

- Attach the script to your scene's camera object, or
- Instantiate our prefab (`CenterEarAnchor.prefab`) in the scene, as a child of a camera object.

For the scripts described above to work, they must be added to the same object, as illustrated in the following image.

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/scripts.png" alt="Scripts impoted in Unity." style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Scripts impoted in Unity.</p>
</div>

### `CenterEarAnchor.prefab`
A prefab designed to be placed under the "center eye" object in a virtual reality scene. Its purpose is to apply an offset, positioning it virtually at the center of the listener's head. This prefab also includes the `BRTAudioListener.cs` script.

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/AudioListener.png" alt="CenterEarAnchor prefab in the Unity inspector" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">CenterEarAnchor prefab in the Unity inspector.</p>
</div>

### `OSC.cs`
A third-party library that implements the OSC protocol. Configure the IP and port for the remote application and the listening port for the Unity app in this script.

## License

Except for the OSC.cs class, which was not developed by us, all other scripts in this package are distributed under the MIT license.
