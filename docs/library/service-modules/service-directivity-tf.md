The Directivity TF service module is responsible for handling the reading, storage, and processing of the trnsfer functions of the directivity of a sound source. These functions are essential for enabling the source model to render sthe directionality of a sound source [(read more)](../source-models/directivity-source-model.md). This module optimizes the accessibility and organization of the Directivity TF data, ensuring efficient processing and integration within the overall rendering pipeline.

A Directivity TF table consists of a collection of Directivity Transfer Functions, each associated with specific measurement directions, including azimuth (left-right) and elevation (up-down) and distance. The BRT is designed to handle arbitrary distributions of mesurement, meaning it does not rely on a regular or complete directional grid and imposes no minimum density requirements. The Directivity Service Module is in charge of estimating the Directivity transfer function for the exact direction and distance of the listener if these are not explicitly included in the loaded table. For this estimation, the algorithm selects the three nearest points at which the Directivity was measured, and performs a barycentric interpolation among the transfer functions corresponding to these three locations [(read more :warning:*(finish and attatch file)*:warning:)]. In this way, this module stores the Directivity TF in the form of a regular grid, enabling more efficient use of resources during binaural rendering. For more details about this grid, click [here](../../assets/technical-report/SONICOM_TR3.1_BRT%20REGULAR%20GRID%20DISTRIBUTION%20OF%20POINTS%20IN%20THE%20SPHERE%20USED%20BY%20THE%20BRT.pdf). 


A set of requirements must be met to the directionality data to work properly in the BRT:

- **Frequency distribution**: the frequency distribution must be linear from 0 to the Nyquist frequency minus one. The number of samples must be the same as configured in the BRT. By default, BRT has a number of samples of 256, but it is configurable using the BeRTA application. 
- **Casual filters**: the filters defining directivity must be causal. Therefore, the transfer functions must have a phase that corresponds to a causal filter. 

## Configuration Options

The methods provided by this service are as follows.

- **Add Directivity Table**: Load an Directivity TF table, perform the intepolation to get a regular grid and store it.
- **Set/Get sampling step for the grid**: The step to created the regular grid for the table can be configured.
- **Get HRIR DIrectivity TF**: Get the loaded or interpolated TF for an specific direction.
