# SOS Filter

The BRT simulates sources in the near-field implementing a compensation for the HRTF conventional processing, where the main algorithm is the convolution with an HRTF measured at a fixed distance. The renderer considers sources in the near fields when they are located at distances lower than 2 meters to the listener’s head. The implemented approach uses the model presented by <a href="https://www.researchgate.net/publication/280979094_Partitioned_convolution_algorithms_for_real-time_auralization" target="_blank">Romblom & Cook (2008)</a>. 

These difference filters are implemented as IIR filters adjusted to match the described transfer function. The BRT includes two biquad filters for each ear, where the coefficients for these filters depend on both the distance of the sound source and its interaural azimuth. These filters are pre-calculated and stored in a file as a look- up table. This process can be considered as an HRIR correction since it is applied in series with the HRIR selected and interpolated in the previous stages of the pipeline. 

When the binaural spatialisation is performed a problem arises when the source or the listener are moving, since some audible artefacts can appear in the signal. In this particular case, those artefacts can be caused as the near-field correction filters have to change from frame to frame. In order to minimise this problem, at every frame each biquad filter is applied using both the previous and the new coefficients, and a linear cross-fading is performed to produce the output.

<!--
## Architecture

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">SOS FIlter class diagram.</p>
</div>
-->

## Functional Overview

This class handles the configuration of second-order section (SOS) filter coefficients. It allows for the addition of SOS filter coefficients based on specific azimuths and distances. The class also provides functionality to retrieve the SOS filter coefficients based on the ear's position, distance, and azimuth.

## Configuration Options

The methods provided by this service are as follows.

- **Add Coefficients**: Adds new second-order section (SOS) filter coefficients for a given interaural azimuth and distance.  
- **Set Ear Position**: Sets the position of the left or right ear within the 3D space.  
- **Get SOS Filter Coefficients**: Retrieves the second-order section (SOS) filter coefficients for a specific ear, distance, and interaural azimuth.  

