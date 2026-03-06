# Service Modules

Service modules are auxiliary components responsible for managing and providing the resources required by the **Processing Models** to execute **binaural audio rendering algorithms** efficiently.

These resources include several key elements used during spatial audio processing:

* **[Head-Related Transfer Functions (HRTFs)](../concepts/index.md#conceptsHRTF)**, which simulate the direct sound path perceived by the listener model.
* **[Binaural Room Impulse Responses (BRIRs)](../concepts/index.md#conceptsBRIR)**, used to reproduce the reverberation characteristics of acoustic environments.
* **Source directivity data**, which represent the directional radiation patterns of sound sources.
* **Second-order filter section (SOS) coefficients**, which allow accurate modeling of, for example, near-field effects.
* **FIR filter coefficients**, which can be used for headphone equalization or hearing-protection simulations.
* **Impulse responses of virtual loudspeakers in the [Ambisonics](../concepts/ambisonics.md) domain**, enabling simultaneous convolution and ambisonic decoding.
* **Geometric and acoustic descriptions of rooms**, used to simulate acoustic environments.

!!! info "Spatial organization"
    Most resources managed by the service modules are **spatially indexed by azimuth, elevation, and distance**. 

    Non-spatial resources are also supported, such as FIR coefficients for headphone compensation filters.


Together, these resources provide the foundation required to generate **realistic and immersive binaural audio experiences**.

---

## Architecture Overview

The interaction between the main architectural components of the BRT framework is illustrated below.

```mermaid
flowchart TD

F[Models]
A[Processing Modules]
B[Service Modules]
C[Readers]
D[SOFA Files]
E[OBJ Files]

F -->|use different processing modules| A
A -->|request resources during rendering| B
B -->|get resources during rendering| A
C -->|pre-load data| B
D --> C
E --> C
```

* **Processing Models** implement the binaural rendering algorithms.
* **Service Modules** store and organize the spatial audio resources used by those algorithms.
* **Readers** load the resources from external data formats.

!!! info "Supported formats"
    The BRT library includes readers capable of loading spatial audio resources from <a href="https://www.sofaconventions.org/mediawiki/index.php/SOFA_(Spatially_Oriented_Format_for_Acoustics)" target="_blank">**SOFA**</a> and <a href="https://en.wikipedia.org/wiki/Wavefront_.obj_file" target="_blanl">**OBJ**</a> formats.
    For more details see the **[READERS](../readers/index.md)** section.

---

## Available Service Modules

The following service modules are available for storing and managing spatial audio resources.

| Module                            | Description                                                                                                                                                                               |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[SphericalFIRTable](./service-spherical-fir-table.md)**             | Stores FIR-based spatial data such as **HRTFs, BRIRs, source directivity information, and general FIR filter coefficients**.                                                              |
| **[SphericalInterpolatedFIRTable](./service-spherical-interpolated-fir-table.md)** | Stores **HRTF and source directivity data** and generates a **regular spherical grid** through interpolation, providing a predictable spatial structure.                                  |
| **[SphericalSOSTable](./service-spherical-sos-table.md)**             | Stores **second-order filter section (SOS) coefficients**, which may be constant or vary depending on **distance, azimuth, and/or elevation**.                                            |
| **AmbisonicBRIR**                 | Stores the **impulse responses of virtual loudspeakers in the ambisonic domain**, enabling a pipeline where **impulse response convolution and ambisonic decoding occur simultaneously**. |
| **Room**                          | Stores the **geometric and acoustic description of a room**, including its **vertices, walls, and frequency-band absorption coefficients** for each surface. An addition to the MTL format has been defined in order to store this acoustic information.                              |
