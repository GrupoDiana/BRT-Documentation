# BeRTA Renderer

:warning:*(Ready for review)*:warning:

BeRTA Renderer is the flagship application of the BRT Toolbox, providing a high degree of configurability for real-time spatial audio rendering. The application integrates the BRT library and works for Windows x64 (Windows 10 and 11) and MacOS Universal. 

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

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/berta_renderer.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">BeRTA Renderer Interface</p>
</div>