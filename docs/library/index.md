# BRT Library: General Overview

The **Binaural Rendering Toolbox (BRT)** is an advanced C++ library designed to support psychoacoustic research and the development of immersive audio applications. As a core component of the BRT Toolbox, the library provides a modular, open-architecture framework that facilitates the creation of dynamic and reproducible virtual acoustic scenarios. This section introduces the main features, purpose, and usage of the BRT Library.

## Introduction

The BRT Library was developed as part of the SONICOM project to address the increasing need for flexible, extensible, and reproducible tools in psychoacoustic research. It builds upon the experience gained from the 3D Tune-In Toolkit while introducing significant architectural improvements, including modularity and ease of extension. The library is intended for researchers and developers aiming to simulate complex binaural scenarios, such as real-time dynamic environments with multiple listeners and sound sources.

## Key Features

1. **Modularity and Extensibility**:
    - The library adopts a header-only structure, simplifying integration into various projects.
    - It is organized into three layers:
        - **Bottom Layer**: Implements low-level, template-based interconnection mechanisms using the observer design pattern.
        - **Middle Layer**: Contains processing modules (e.g., convolvers, filters) and service modules (e.g., SOFA file readers).
        - **Top Layer**: Combines modules into high-level models (e.g., listener, source, and environment models).
2. **Dynamic Virtual Acoustics**:
    - Supports anechoic and reverberant environments.
    - Enables simulation of near-field effects and dynamic HRTF swapping.
3. **Applications and Portability**:
    - Easily deployable on various systems, including standalone executables, mobile and wearable devices.

## Purpose and Applications (ARREGLAR)

The BRT Library is designed for researchers and developers working on psychoacoustic experiments, auditory modeling, and immersive audio rendering. Its flexibility supports use cases such as:

- Creating custom binaural rendering pipelines.
- Testing and validating auditory models with dynamic configurations.
- Simulating realistic auditory environments in real time.

Additionally, the library aims to reduce the complexity of integrating novel algorithms and adapting to different research requirements by providing well-defined internal interfaces and modular building blocks.

C++ developers can leverage the BRT Library to build tailored applications by:

- Interconnecting predefined modules to simulate specific acoustic scenarios.
- Extending the library by developing custom low-level or high-level modules.

The BRT Library is actively maintained, with ongoing efforts to expand its capabilities, including support for multi-listener scenarios, advanced reverberation models, and hearing loss simulations.

## Description

The library is organized into three layers, as previously discussed. From the perspective of a C++ programmer[^1], using the library mainly involves working with high-level and service modules.

[^1]: If you prefer a higher-level approach, you can use our applications and control them via OSC commands. For more details, see [Applications](/BRT-Documentation/applications/).

### High-Level Modules

High-level modules are responsible for audio rendering. Each module models specific physical and/or psychoacoustic phenomena, depending on the use case. Currently, several types of models have been implemented, grouped into four categories:

- **Source Models: Simulate an audio source**
    - *Simple Omnidirectional Source Model*
    - *Directional Source Model*

- **Listener Models**
    - [Listener Acoustic Model Based on Convolution with HRTF](../library/listener-models/listener-acoustic-model-hrtf.md): Simulates the direct path using convolution with HRTFs.
    - [Listener Acoustic Model Based on Ambisonics](../library/listener-models/listener-acoustic-model-ambisonic-hrtf.md): Simulates the direct path using convolution with HRTFs in the ambisonic domain.
    - [Listener & Environment Acoustic Model Based on Convolution with BRIR](../library/listener-models/listener-acoustic-model-brir.md): Simulates both the direct path and reverberation through convolution with a BRIR.
    - [Listener & Environment Acoustic Model Based on Ambisonics](../library/listener-models/listener-acoustic-model-ambisonic-brir.md): Simulates the direct path and reverberation using convolution with a BRIR in the ambisonic domain.
    - *Listener Auditory Model of Hearing Loss*: *(Under development)*.
    - *Listener Model of Hearing Aid*: *(Under development)*.

- **Environment Models**:
    - *Free Field*: Simulates propagation in a free-field environment, including propagation delay, attenuation, and filtering.
    - *SDN*: Simulates room reverberation using the Scattering Delay Networks method [URL].
    - *ISM*: Simulates room reverberation using the Image Source Method *(Under development)*.
    - *Hybrid: ISM + Convolution*: Simulates room reverberation where early reflections are modeled using the Image Source Method, and the reverberant tail is simulated through convolution with a BRIR *(Under development)*.

- **Binaural Filters**:
    - *SOS Filters*: Perform binaural filtering based on second-order sections, enabling the simulation of devices such as headphones.

### Service Modules

Service modules store the essential data required for rendering. This data typically comes from SOFA files, although other formats could be used. Key service modules include:

- **HRTF**: Stores head-related impulse responses indexed by azimuth and elevation.
- **BRIR**: Stores room-related impulse responses indexed by azimuth and elevation.
- **DirectivityTF**: Stores transfer functions of a sound source based on the position of the listener and the sources.
- **SOSFilters**: Stores coefficients for second-order sections of a filter, which can be fixed or vary based on distance, azimuth, and/or elevation.

---

## Usage

To render audio using the library, the required modules must be instantiated and interconnected based on the desired simulation (configuration). This is managed through the `brtManager` class [URL]. See the **Setup/Examples** section for detailed guidance. Below are examples of configurations that can be created:

- **Basic Anechoic Simulation**: Combine a listener model with a source model for anechoic rendering.
- **Room Simulation**: Add an environment model to simulate reverberation.
- **Custom Filters**: Incorporate binaural filters for simulating specific devices like headphones.



---

For further technical details and code examples, visit the official repository: <a href="https://github.com/GrupoDiana/BRTLibrary" target="_blank">BRT Library GitHub</a>.
