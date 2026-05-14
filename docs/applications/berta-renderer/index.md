# BeRTA Renderer

BeRTA Renderer is the flagship application of the BRT Toolbox, providing a high degree of configurability for real-time spatial audio rendering. The application integrates the BRT library and works for Windows x64 (Windows 10 and 11) and MacOS Universal. 

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/berta_renderer.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">BeRTA Renderer Interface</p>
</div>

Key functionalities include:

- **Real-time Rendering**:
    - Dynamic control of sound source locations, listener positions, and other spatial parameters via OSC commands.
    - Support for multiple sound sources and HRTF swapping in real time.
- **Configurable Virtual Scenarios**:
    - User-defined configurations for different buffer sizes, sample rates, and rendering parameters.
    - GUI-based feedback for monitoring application status, including rendering states and OSC commands.
- **Data Saving and Export**:
    - Saves all parameters and audio using the proposed annotated audio SOFA convention format for reproducibility.
    - Exports simulation data for further analysis or use in auditory modeling.


The application is fully controlled through  <a href="https://en.wikipedia.org/wiki/Open_Sound_Control" target="_blank">OSC</a> (Open Sound Control), resulting in a graphical interface focused primarily on reporting rendering status. The GUI consists of terminal-like sections that display critical information, such as rendering parameters (e.g., enabled or disabled features) and the settings of audio sources and the listener. Additional terminals provide detailed logs of OSC commands received and sent, actions executed by BeRTA, and error messages detected by BeRTA's built-in diagnostic system.

Some of the relevant menus are:

- [**Settings menu**](settings-menu.md): allows users to view and configure general application parameters.
- [**Resources menu**](resources-menu.md): displays all resources currently loaded in the application.

Other important features:

- [**Settings file**](../settingsFile.md): How to define an application configuration file that is read when the application starts.
- [**Calibration**](../calibration.md): BeRTA provides a calibration option, which allows establishing the relationship between the system's dBFS (Decibels Full Scale) and the SPL (Sound Pressure Level) in dB of the output signal.
- [**Safety Limiter**](../safety-limiter.md): BeRTA includes a Safety Limiter that allows users to define a maximum SPL threshold, preventing output levels from exceeding safe limits. 