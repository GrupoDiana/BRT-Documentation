# BRIR Service Module
:warning:*(Section ready for review)*:warning:

The BRIR service module is responsible for managing the reading, storage, and processing of BRIRs. These functions are crucial for enabling the system to simulate room reverberation and provide accurate spatial audio rendering [(read more)](../environment-models/freefield-environment-model.md). This module ensures that the BRIR data is well-organized and easily accessible, facilitating efficient processing and seamless integration within the overall audio rendering pipeline.

An BRIR consists of a collection of BRIRs, each associated with specific measurement directions, including azimuth (left-right) and elevation (up-down) and distance. The BRT is designed to handle arbitrary distributions of BRIR measurement directions, meaning it does not rely on a regular or complete directional grid and imposes no minimum density requirements. The HR-BRIR Service Module is in charge of estimating BRIRs for the exact direction and distance of the source if these are not explicitly included in the HRTF data. For this estimation, the algorithm selects the three nearest points at which the BRIR was measured, and performs a barycentric interpolation among the BRIRs corresponding to these three locations [(read more :warning:*(finish and attatch file)*:warning:)]. In this way, this module stores the BRIR in the form of a regular grid, enabling more efficient use of resources during binaural rendering. For more details about this grid, click [here](../../assets/technical-report/SONICOM_TR3.1_BRT%20REGULAR%20GRID%20DISTRIBUTION%20OF%20POINTS%20IN%20THE%20SPHERE%20USED%20BY%20THE%20BRT.pdf). 


Each BRIR is partitioned in chunks to match the input buffer length. This is done in order to use the  [UPOLS convolution](../listener-models/hrtf-models/listener-acoustic-model-hrtf.md). In addition, an FFT is applied to each of the HRIR partitions and stored in memory by the Service Module, since the convolution is done in the frequency domain.

The following diagram illustrates the processing performed on the HRTF table before it is stored. This processing is performed offline, ensuring that, in real-time, we have a regular table that is faster to access.

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/BRIR_offlineProcess.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">BRIR offline process.</p>
</div>


## Architecture

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Head-Related BRIR class diagram.</p>
</div>

## Functional Overview

The methods in the HRBRIR class provide the tools necessary for configuring and managing Head-Related BRIR data within the renderer. This service supports loading multiple BRIRs, offering flexibility in spatial audio setups. To load a BRIR file, a dedicated [Reader](../readers/index.md) class is necessary to parse the file and add the BRIRs to the HRBRIR class step by step.  The process begins with initializing the configuration using **Begin Setup** and concludes with **End Setup**, which locks in the loaded data. Additional methods enable setting sampling rates, managing file names, defining sampling steps for the BRIR grid, and retrieving partitioned BRIRs for specific directions. This class also includes the SetWindowingParameters method, which sets the parameters for the [windowing process](../listener-models/rir-models/index.md) to optimize the impulse response.

## Configuration Options

The methods provided by this service are as follows.

- **Begin Setup**: Prepares the HRBRIR object for initializing and starts the configuration.
- **End Setup**: Finalizes the setup process and store in the configured HRBRIR data.
- **Set Sampling Rate**:  Specifies the sampling rate for the HRBRIR data
- **Add BRIR**: Adds a Head-Related Impulse Response (HRBRIR) to the HRBRIR dataset for a specific position.
- **Set/Get file name**: Assigns or retrieves the file name of the HRBRIR data to be used.
- **Set/Get sampling step for the grid**: Defines or retrieves the angular step size used to sample the HRBRIR grid.
- **Get BRIR Partitioned**: Retrieves  the interpolated and partitioned stored HRBRIR for an specific direction.
- **SetWindowingParameters**: Sets the parameters for the windowing process, defining the midpoint and rise time for both fade-in and fade-out windows in the impulse response.


<details>
<summary>For C++ developer</summary>
Section under construction
</details>