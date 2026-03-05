# Spherical Interpolated FIRT able

## Overview

_SphericalInterpolatedFIRTable_ is a Service Module designed to store and manage **finite impulse response (FIR) data organized in spherical coordinates**. It extends the concept of spatial FIR storage by generating a **regular spherical sampling grid through interpolation**. This regularized representation provides predictable spatial indexing and facilitates efficient access during rendering. The module is particularly suited for datasets such as HRTFs or source directivity measurements that originate from irregular measurement distributions.

## Role in the Architecture

Within the BRT architecture, Service Modules provide structured access to resources used by rendering algorithms. _SphericalInterpolatedFIRTable_ stores spatial FIR datasets that are typically loaded by **Readers** from external formats such as SOFA files. **Processing Models** then query this module during runtime to retrieve impulse responses corresponding to specific spatial directions. This separation allows algorithms to remain independent from file formats and raw dataset organization.

## Data Organization

The data is organized as a collection of **distance-dependent spherical tables**. Each distance defines a *distance bucket*, representing a measurement sphere centered on the listener. Inside each bucket, impulse responses are indexed by **azimuth and elevation**, forming a regular directional grid. This structure allows the system to manage datasets that contain measurements at multiple listener-source distances.

For HRTF datasets, each distance bucket corresponds to a **measurement sphere around the listener**. During rendering, the system typically selects the **closest available sphere** to the requested source distance. This approach preserves the physical meaning of distance-dependent measurements while keeping runtime access efficient.

Within each sphere, the directional grid is generated through **barycentric interpolation of the original measurements**. This produces a consistent spatial sampling even when the source dataset is irregular. At runtime, Processing Models can either retrieve the **nearest grid direction** or perform a **fast barycentric interpolation** between neighboring directions for smoother spatial transitions.

**Internal FIR Representation**

Before being stored in the module, the HRIRs are transformed into a representation optimized for real-time convolution. Each HRIR is partitioned into fragments matching the input buffer size in order to support **[Uniformly Partitioned Overlap-Save (UPOLS) convolution](../processing-modules/uniform-partitioned-convolution.md)**. An FFT is then applied to every partition and the resulting spectra are stored in memory by the Service Module. As a result, the binaural rendering stage performs convolution directly in the **frequency domain**, significantly improving computational efficiency.

The following diagram summarizes the **preprocessing** and **runtime stages** involved in the internal representation of HRIR data.

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/HRTF_offlineProcess.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Offline interpolation process diagram.</p>
</div>

<figure markdown style="width:100%; border: 1px solid #000;">
```mermaid
flowchart LR

A[Processing Model requests HRIR]

subgraph Spherical Interpolated
B[Find Nearest]
C[Interpolation]
end

H[Frequency-domain convolution<br>UPOLS]
I[Binaural output signal]


A --> B
B --> C
C -->|Retrieve frequency partitions| H
H --> I
```
<figcaption>Online process diagram.</figcaption>
</figure>

### Data store hierarchy

```mermaid
graph LR

B[Distance Sphere]
B --> C[Direction: Azimuth / Elevation]
C --> D[Partitioned FR of the left ear]
C --> E[Partitioned FR of the right ear]
```

## Interpolation Strategy

For applications requiring **smooth and continuous 3DoF binaural rendering**, BRT provides interpolation capabilities within this Service Module. This is particularly useful in dynamic virtual environments when the available HRTF measurements do not form a regular or dense directional grid. Many datasets contain sparse measurements or large regions without data, which can lead to discontinuities when rendering directly from the measured points.

To address this limitation, _SphericalInterpolatedFIRTable_ estimates HRIRs for the **exact direction and distance of the sound source**. During an **offline preprocessing stage**, the algorithm identifies the three closest measured directions surrounding each target grid point. A **barycentric interpolation** is then performed between the corresponding HRIRs to estimate the impulse response at that location.

The result of this preprocessing step is a **regular spherical grid of FIR responses**, which is stored internally by the module. The density of this grid is controlled by a configurable parameter called **spatial resolution**, which defines the angular sampling of the generated spherical mesh. Higher spatial resolutions produce denser grids and more accurate spatial reconstruction, at the cost of increased memory usage and preprocessing time.  This regularization allows Processing Models to perform efficient spatial lookups during rendering. Additional real-time interpolation between neighboring grid points can also be applied when smoother directional transitions are required. You can find more details on how this grid is generated in this <a href="../../../assets/technical-report/SONICOM_TR3.1_BRT REGULAR GRID DISTRIBUTION OF POINTS IN THE SPHERE USED BY THE BRT.pdf" target="_blank">document</a>.

Interpolating HRIRs with different **interaural time differences (ITDs)** can introduce audible artifacts and degrade rendering quality. To avoid this issue, the HRTF Service Module handles **ITDs independently from the FIR interpolation and convolution processes**. This separation prevents temporal inconsistencies when combining impulse responses from different measurement directions. For this reason, user-imported HRIR datasets should provide **ITD information stored separately from the impulse responses**. 

After the spatial interpolation step, the appropriate ITD can be applied to the rendered signal. These ITDs may be estimated by interpolating the ITDs associated with the three nearest HRIR measurements, or in another extra feature, they could synthesized from geometric information such as **interaural azimuth and listener head circumference**.

## Supported Data Types

The module can store several types of spatial FIR datasets used in binaural rendering:

- **HRTFs (Head-Related Transfer Functions)** – impulse responses describing how sound from a spatial direction is filtered by the listener’s anatomy.  
- **Source directivity datasets** – directional impulse responses describing how a sound source radiates energy in space.  

These datasets share the common property of representing **direction-dependent acoustic filtering**.

## Typical Use Cases

A typical use case is **binaural rendering with interpolated HRTFs**, where the rendering engine queries impulse responses for arbitrary source directions. The regular grid generated by this module simplifies spatial interpolation and improves runtime predictability. Another use case is modeling **directional sound sources**, where the source radiation pattern is represented as FIR responses over a spherical domain. The module can also support research workflows where measured datasets must be resampled to a consistent spatial resolution.

## Related Service Modules

**SphericalFIRTable**

Stores FIR responses indexed by spherical coordinates but **preserves the original measurement distribution**. It does not generate a regular grid and therefore relies on the spatial structure provided by the dataset.

**SphericalSOSTable**

Stores spatial filters represented as **second-order section (SOS) filter banks** instead of FIR impulse responses. This representation is typically used when parametric or IIR filter models are preferred over impulse responses.

## Summary

_SphericalInterpolatedFIRTable_ provides a structured container for **spatial FIR datasets resampled onto a regular spherical grid**. By separating spatial resource management from rendering algorithms, it supports modular and flexible binaural processing pipelines. The module enables predictable spatial lookup and simplifies interpolation during runtime. Within the BRT architecture, it acts as a bridge between **dataset Readers and binaural Processing Models**, supporting reproducible research in spatial audio rendering.
