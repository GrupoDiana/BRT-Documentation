#BeRTA GUI

BeRTA GUI is a cross-platform interface (Windows x64 and macOS) for controlling the BeRTA Renderer via OSC. It enables seamless setup and management of the renderer, including launching, relaunching with custom settings, and loading essential resources like HRTFs and BRIRs. The GUI provides real-time control over sound sources, listener movement, and rendering features while visually displaying the scene's layout.

The key functionalities include:

- **Setting BeRTA Renderer**: 
    - Launching BeRTA GUI Will launch BeRTA Renderer and connect both.
    - BeRTA GUI can relaunch BeRTA Renderer with different custom setting files.
    - Sending OSC commands to load all type of resources (HRTFs, BRIRs, Directivities, Near-field compensations).
- **Real Time Control**:
    - Sending commands to play, pause, stop or mute sound sources.
    - Sending commands to set line-in sources.
    - Sending commands to move listener and source (6DoF)
    - Sending commands to switch on the fly rendering characteristics in BeRTA Render (as HRTF of near-field compensation)
- **Scene display**:
    - Showing graphically the position and orientation of sources with respect to the listener.
    - Receiving and showing updates on the scene sent to BeRTA Renderer by a third application (e.g. an app running in Unity which updates the Listener position)

 
 <div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/berta_gui.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">BeRTA GUI Interface</p>
</div>

