# Settings Menu

The **Settings** menu allows users to view and configure general application parameters. It is divided into two sections:
 **Audio Interfaces** and **General settings**

## General settings

This section displays global parameters defined in the `settings.json` configuration file. Some of these parameters can be modified directly from the interface, while others are read-only.

<div style="border: 1px solid #000; padding: 10px; display: block; max-width: 800px; margin: 0 auto;">
    <img src="/BRT-Documentation/assets/berta_renderer_setting_menu_general.png" alt="BeRTA Renderer - General Settings menu" style="display: block; margin: 0 auto; max-width: 100%;">
    <p style="text-align: center;">BeRTA Renderer - General Settings menu</p>
</div>

### Parameters

| Parameter | Description | Editable |
|----------|-------------|----------|
| **Sample rate** | Defines the sampling rate used by the application. All loaded audio files and audio devices must operate at this same rate. | ❌ No |
| **Buffer size** | Number of frames per processing block. Larger values increase latency but provide more processing time, enabling more complex simulations (e.g., reverberation). | ❌ No |
| **Buffer Frames** | Parameter related to the underlying audio backend (e.g., RtAudio on Windows). It defines the number of internal buffers used before sending audio to the output device. The operating system will try to honor this value, but it is not guaranteed. Higher values may improve stability but increase latency. | ✅ Yes |
| **OSC Listen Port** | UDP port where BeRTA listens for incoming OSC messages. | ✅ Yes |
| **Extrapolation Method** | Defines the extrapolation strategy used during interpolation when loading HRTFs or directivity data. Available options: <br><br>• **ZeroInsertion**: Missing data points are filled with zeros. This method avoids introducing artificial information but may produce discontinuities. <br>• **NearestPoint**: Uses the closest available data point. This provides smoother results but may introduce spatial bias. | ✅ Yes |


---

## Audio Interfaces

This section allows selecting the audio devices used for input and output.

Two lists are displayed:

- Available **input devices**
- Available **output devices**

Each device entry includes:

- Audio backend (e.g., ASIO, WASAPI)
- Device name
- Number of available channels

!!! info API limitation
    Both input and output devices must use the **same audio backend/API**.  This is a limitation imposed by the underlying audio library (audio backend).

<div style="border: 1px solid #000; padding: 10px; display: block; max-width: 800px; margin: 0 auto;">
    <img src="/BRT-Documentation/assets/berta_renderer_setting_menu_audio_interfaces.png" alt="BeRTA Renderer - Audio Interfaces menu" style="display: block; margin: 0 auto; max-width: 100%;">
    <p style="text-align: center;">BeRTA Renderer - Audio Interfaces menu</p>
</div>

### Input Interface

The input interface is used for **line-in sound sources**.

#### Input Channels

The **Input Channels** dropdown defines how many channels from the selected input device will be available for use.

- Channels are assigned **sequentially starting from 1**
- If set to `0`, no input channels are used
- If set to `N`, the first `N` channels are enabled

When creating a line-in source, the parameter `linein_channel` specifies which of these enabled channels is used.

**Example:**

- Device with 4 input channels
- Input Channels set to `2`
- Available channels for use: **1 and 2**
- Valid `linein_channel` values: `1` or `2`


### Output Interface

The output interface defines the audio device used for playback.

#### Output Channels

The **Output Channels** dropdown determines how many output channels are enabled.

- Channels are enabled **in pairs** (2, 4, 6, ...)
- Each **listener requires 2 channels** (left and right)

**Example:**

- If `4` channels are selected:
  - The system can render **2 listeners simultaneously**

This allows multi-listener simulations using a single multi-channel audio device.

