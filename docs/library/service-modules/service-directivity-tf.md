# Directivity Service Module
:warning:*(Ready for review)*:warning:

The Directivity TF service module is responsible for handling the reading, storage, and processing of the transfer functions of the directivity of a sound source. These functions are essential for enabling the source model to render the directionality of a sound source [(read more)](../source-models/directivity-source-model.md). This module optimizes the accessibility and organization of the Directivity TF data, ensuring efficient processing and integration within the overall rendering pipeline.

A Directivity TF table consists of a collection of Directivity Transfer Functions, each associated with specific measurement directions, including azimuth (left-right) and elevation (up-down) and distance. The BRT is designed to handle arbitrary distributions of mesurement, meaning it does not rely on a regular or complete directional grid and imposes no minimum density requirements. The Directivity Service Module is in charge of estimating the Directivity transfer function for the exact direction and distance of the listener if these are not explicitly included in the loaded table. For this estimation, the algorithm selects the three nearest points at which the Directivity was measured, and performs a barycentric interpolation among the transfer functions corresponding to these three locations. In this way, this module stores the Directivity TF in the form of a regular grid, enabling more efficient use of resources during binaural rendering. For more details about this grid, click [here](../../assets/technical-report/SONICOM_TR3.1_BRT%20REGULAR%20GRID%20DISTRIBUTION%20OF%20POINTS%20IN%20THE%20SPHERE%20USED%20BY%20THE%20BRT.pdf). 


A set of requirements must be met to the directionality data to work properly in the BRT:

- **Frequency distribution**: the frequency distribution must be linear from 0 to the Nyquist frequency minus one. The number of samples must be the same as configured in the BRT. By default, BRT has a number of samples of 256, but it is configurable using the BeRTA application. 
- **Casual filters**: the filters defining directivity must be causal. Therefore, the transfer functions must have a phase that corresponds to a causal filter. 

## Architecture

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Directivity class diagram.</p>
</div>

## Functional Overview

These methods manage the setup, configuration, and retrieval of directivity transfer functions (TFs). They allow initialization with specific parameters, adding TFs for given directions, retrieving existing TFs with optional runtime interpolation, and setting or querying the resampling step. The process of loading a new directivity table begins by calling **BeginSetup**, followed by adding transfer functions one by one using **AddDirectivityTF**, and finalizing the configuration with **EndSetup**. To load the directivity data, a [Reader](../readers/index.md) class is used to read the data files and load them into the class's directivity table. These features support efficient handling and processing of directional audio data.


## Configuration Options

The methods provided by this service are as follows.

- **Begin Setup**: Initializes the configuration with parameters for transfer function length and extrapolation method.  
- **End Setup**: Finalizes the setup process and validates the configuration.  
- **Set/Get ResamplingStep**: Defines ans retrieves the resampling step size for directivity data grid.  
- **Get Directivity TF Length**: Returns the length of the directivity transfer functions.  
- **AddDirectivityTF**: Adds a directivity transfer function (TF) for a specific azimuth and elevation.  
- **GetDirectivityTF**: Retrieves a directivity transfer function for a given azimuth and elevation, with optional runtime interpolation.  
