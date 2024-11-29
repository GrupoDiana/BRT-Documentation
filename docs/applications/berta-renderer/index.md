# BeRTA Renderer
BeRTA Renderer is an audio renderer that integrates the BRT library and works for Windows x64 (Windows 10 and 11) and MacOS Universal. It is an application that is entirely by <a href="https://en.wikipedia.org/wiki/Open_Sound_Control" target="_blank">OSC</a>. Thus its graphical interface is limited almost exclusively to reporting the rendering status.

The GUI is a simple combination of terminal-like areas which provide information on the status of the application: the rendering parameters (what is enabled and what is not), or the parametres of audio sources and listener. Other terminals show the OSC commands received and sent, the commands executed by BeRTA, and the messages signaling the errors that BeRTA is programmed to detect.

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/berta_renderer.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">BeRTA Renderer Interface</p>
</div>