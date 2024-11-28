
The BRIR service module is responsible for managing the reading, storage, and processing of BRIRs. These functions are crucial for enabling the system to simulate room reverberation and provide accurate spatial audio rendering. This module ensures that the BRIR data is well-organized and easily accessible, facilitating efficient processing and seamless integration within the overall audio rendering pipeline.

 An HRTF consists of a collection of HRIRs, each associated with specific measurement directions, including azimuth (left-right) and elevation (up-down) and distance. The BRT is designed to handle arbitrary distributions of HRIR measurement directions, meaning it does not rely on a regular or complete directional grid and imposes no minimum density requirements. The HRTF Service Module is in charge of estimating HRIRs for the exact direction and distance of the source if these are not explicitly included in the HRTF data within the SOFA file. For this estimation, the algorithm selects the three nearest points at which the HRTF was measured, and performs a barycentric interpolation among the HRIRs corresponding to these three locations [(read more :warning:*(finish and attatch file)*:warning:)]. In this way, this module stores the HRTF in the form of a regular grid, enabling more efficient use of resources during binaural rendering. For more details about this grid, click [here](../../assets/technical-report/SONICOM_TR3.1_BRT%20REGULAR%20GRID%20DISTRIBUTION%20OF%20POINTS%20IN%20THE%20SPHERE%20USED%20BY%20THE%20BRT.pdf). 


Each HRIR is partitioned in chunks to match the input buffer length. This is done in order to use the  [UPOLS convolution](../listener-models/hrtf-models/listener-acoustic-model-hrtf.md). In addition, an FFT is applied to each of the HRIR partitions and stored in memory by the Service Module, since the convolution is done in the frequency domain.

The following diagram illustrates the processing performed on the HRTF table before it is stored. This processing is performed offline, ensuring that, in real-time, we have a regular table that is faster to access.

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/HRTF_offlineProcess.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">HRTF offline process.</p>
</div>

## Configuration Options

The methods provided by this service are as follows.

- **Add BRIR Table**: Load an HRTF table, perform the intepolation to get a regular grid and store it.
- **Set/Get sampling step for the grid**: The step to created the regular grid for the HRTF can be configured.
- **Get BRIR Partitioned**: Get the interpolated and partitioned stored HRTF for an specific direction.




