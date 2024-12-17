# BRT Applications: General Overview

The **Binaural Rendering Toolbox (BRT)** includes a collection of applications designed to provide researchers and developers with ready-to-use tools for creating, testing, and deploying immersive audio experiences. These applications leverage the core functionalities of the BRT Library and extend them into practical, real-time tools for diverse use cases. This section introduces the main applications, their features, and intended use cases.

## Introduction

The BRT Applications are standalone software tools built around the BRT Library, enabling seamless integration of binaural rendering into various workflows. These applications are tailored for both experimental research and real-world implementation, offering high configurability and interoperability with existing systems. From immersive auditory experiments to wearable solutions, BRT Applications are designed to meet the demands of modern psychoacoustics and virtual acoustics research.

## Key Features

1. **Interoperability**:
    - All BRT Applications support the **Open Sound Control (OSC)** protocol for external control, allowing integration with other tools and frameworks such as MATLAB, PureData, and Unity 3D.
    - Compatible with standard file formats like **SOFA** and **WAV**, ensuring smooth data exchange and reproducibility.

2. **Reproducibility**:
    - Applications record all relevant parameters of binaural simulations, facilitating accurate reproduction and analysis of experimental results.
    - Support for a new SOFA-based convention for annotated audio, capturing dynamic spatial configurations alongside binaural audio.

---

## Applications

### BeRTA Renderer (BRT Audio Renderer)

**BeRTA Renderer** is the flagship application of the BRT Toolbox, providing a high degree of configurability for real-time spatial audio rendering. Key functionalities include:

- **Real-time Rendering**:
    - Dynamic control of sound source locations, listener positions, and other spatial parameters via OSC commands.
    - Support for multiple sound sources and HRTF swapping in real time.

- **Configurable Virtual Scenarios**:
    - User-defined configurations for different buffer sizes, sample rates, and rendering parameters.
    - GUI-based feedback for monitoring application status, including rendering states and OSC commands.

- **Data Saving and Export**:
    - Saves all parameters and audio in the annotated audio format for reproducibility.
    - Exports simulation data for further analysis or use in auditory modeling.

### BeRTA GUI (BRT Audio Renderer GUI)

**BeRTA GUI** is a graphical interface (Windows x64 and MacOS) to control BeRTA Renderer via OSC. 

### Wearable Solutions

BRT Toolbox also includes applications designed for portable, low-latency rendering on wearable devices:

- **BeRTA-mini**:
    - A lightweight version of BeRTA tailored for wearable platforms, combining a BELA audio board, IMU sensors (gyroscope, magnetometer, accelerometer), and headphones.
    - Designed for on-the-go binaural rendering with high responsiveness and accuracy.

- **Tracker Interoperability**:
    - Integrates with MBIENTLAB’s MetaMotionRL and other motion tracking devices.
    - Converts tracking data into OSC commands for seamless interaction with other BRT applications.

### Future Applications

Several additional tools are under development or planned for future releases, including:
    - Multi-listener rendering applications.
    - Tools for simulating advanced reverberation models (e.g., convolution-based and hybrid approaches).
    - Hearing loss and hearing aid simulations.

## Usage

BRT Applications are ideal for:
- Psychoacoustic experiments requiring dynamic spatial configurations.
- Integration into existing experimental setups with OSC-controlled workflows.
- Real-time virtual acoustics rendering for VR/AR applications and wearable devices.

These applications are designed to minimize the coding effort required to deploy sophisticated audio rendering capabilities, making them accessible to a wide range of users.

---

For more information about the BRT Applications and their functionalities, visit the [BRT GitHub Repository](https://github.com/GrupoDiana/BRTLibrary).
