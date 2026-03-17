# SOFA Reader

## Overview

_SOFA Reader_ is the main entry point in BRT for loading <a href="https://www.sofaconventions.org/mediawiki/index.php/SOFA_(Spatially_Oriented_Format_for_Acoustics)" target="_blank">SOFA</a> files into the internal data structures used by the rendering engine. SOFA format is widely adopted in spatial audio applications due to its flexibility and compatibility with 3D audio systems.

It provides a unified interface to read different types of spatial audio data from SOFA files (e.g., HRTFs, BRIRs, directivity, filters) and converts them into BRT service modules such as: `CSphericalFIRTable`, `CSphericalInterpolatedFIRTable` or `CSphericalSOSTable`. 
Internally, it relies on a wrapper around <a href="https://github.com/hoene/libmysofa" target="_blank">libmysofa</a> to parse SOFA files and then adapts the data to the BRT conventions.

## Loaders Overview

The `SOFA Reader` class provides several specialized loaders depending on the **intended use of the data**.

### HRTF Loaders
- Load Head-Related Transfer Functions.
- Can produce: 
    - **Interpolated HRTF datasets**: A grid of interpolated data is generated from the IR values read from the file. For further information, please see [here](../service-modules/service-spherical-interpolated-fir-table.md).
    - **Raw datasets**: The stored data matches that read from the file [^1]. For further information, please see [here](../service-modules/service-spherical-fir-table.md).
- It supports any HRTF convention using _FIR_ or _FIR-E_ data types, although it has been extensively tested with <a href="https://www.sofaconventions.org/mediawiki/index.php/SimpleFreeFieldHRIR" target="_blank">SimpleFreeFieldHRIR</a>.
 
[^1]: Please note that responses to the trigger are always processed and stored at the frequency and partitioned. 

### BRIR Loader
- Loads Binaural Room Impulse Responses.
- It supports any BRIR convention using _FIR_ or _FIR-E_ data types, although it has been extensively tested with <a href="https://www.sofaconventions.org/mediawiki/index.php/SingleRoomMIMOSRIR" target="_blank">SingleRoomMIMOSRIR</a>.
- It allows the BRIR to be adjusted using fade-in and fade-out parameters. To achieve this, a rectangular window with smooth edges is applied, using high-cosine progressive input and output transitions with a configurable slope. This process allows early impulse components to be attenuated and/or late impulse components to be clipped. For further details, see [here](../listener-models/rir-models/index.md).

### FIR Filter Loader
- Loads generic impulse responses from SOFA files.
- Used for arbitrary IR-based processing.
- It supports any convention using _FIR_ or _FIR-E_ data types, although it has been extensively tested with <a href="https://www.sofaconventions.org/mediawiki/index.php/GeneralFIR" target="_blank">GeneralFIR</a>.

### SOS Filter Loader
- Loads IIR filters (Second Order Sections).
- Typically used for:
    - Near-Field Correction filters
    - Perfomn any filtering based on second-order sections
- It supports any _SOS_ convention, although it has been extensively tested with <a href="https://www.sofaconventions.org/mediawiki/index.php/SimpleFreeFieldHRSOS" target="_blank">SimpleFreeFieldHRSOS</a>.

### Directivity Loaders
- Load source directivity patterns.
- Requires specific SOFA convention: `SourceDirectivityFIR`.
- Can produce: 
    - **Interpolated directivity**: A grid of interpolated data is generated from the IR values read from the file. For further information, please see [here](../service-modules/service-spherical-interpolated-fir-table.md).
    - **Raw directivity**.The stored data matches that read from the file [^1]. For further information, please see [here](../service-modules/service-spherical-fir-table.md).
- It works with the new _SourceDirectivityFIR_ convention. This convention is currently under development, so this loader will adapt to any changes that may occur. It is currently compatible with version 0.3.


## Supported SOFA Data Types

The reader supports the following SOFA data types:

| Data Type | Description |
|----------|-------------|
| `FIR`    | Finite Impulse Response filters |
| `FIR-E`  | FIR with additional metadata (extended format) |
| `SOS`    | Second Order Sections (IIR filters) |



<details>
<summary>For C++ developer</summary>

<h3>General Methods</h3>
<table>
  <thead>
    <tr>
      <th>Method</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>GetLastError()</code></td>
      <td>Returns the last error message and resets it</td>
    </tr>
    <tr>
      <td><code>GetSampleRateFromSofa(const std::string&)</code></td>
      <td>Returns the sampling rate of the SOFA file</td>
    </tr>
    <tr>
      <td><code>GetDataTypeFromSofa(const std::string&)</code></td>
      <td>Returns the detected SOFA data type</td>
    </tr>
  </tbody>
</table>

<hr/>

<h3>HRTF Loaders</h3>
<table>
  <thead>
    <tr>
      <th>Method</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>ReadHRTFFromSofa(...)</code></td>
      <td>Loads an interpolated HRTF dataset into <code>CSphericalInterpolatedFIRTable</code></td>
    </tr>
    <tr>
      <td><code>ReadHRTFRawFromSofa(...)</code></td>
      <td>Loads raw HRTF data into <code>CSphericalFIRTable</code></td>
    </tr>
  </tbody>
</table>

<hr/>

<h3>BRIR Loader</h3>
<table>
  <thead>
    <tr>
      <th>Method</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>ReadBRIRFromSofa(...)</code></td>
      <td>Loads BRIR data and applies fade-in/out to separate direct and reverberant components</td>
    </tr>
  </tbody>
</table>

<hr/>

<h3>FIR Filter Loader</h3>
<table>
  <thead>
    <tr>
      <th>Method</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>ReadFIRFilterFromSofa(...)</code></td>
      <td>Loads a generic FIR dataset into a spherical FIR table</td>
    </tr>
  </tbody>
</table>

<hr/>

<h3>Directivity Loaders</h3>
<table>
  <thead>
    <tr>
      <th>Method</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>ReadDirectivityFromSofa(...)</code></td>
      <td>Loads interpolated source directivity</td>
    </tr>
    <tr>
      <td><code>ReadDirectivityRawFromSofa(...)</code></td>
      <td>Loads raw directivity without interpolation</td>
    </tr>
  </tbody>
</table>

<hr/>

<h3>SOS Filter Loader</h3>
<table>
  <thead>
    <tr>
      <th>Method</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>ReadSOSFiltersFromSofa(...)</code></td>
      <td>Loads SOS filters into <code>CSphericalSOSTable</code></td>
    </tr>
  </tbody>
</table>

</details>