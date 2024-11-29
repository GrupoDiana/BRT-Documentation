# SOS Filter
:warning:*(Section under construction)*:warning:

The BRT simulates sources in the near-field implementing a compensation for the HRTF conventional processing, where the main algorithm is the convolution with an HRTF measured at a fixed distance. The renderer considers sources in the near fields when they are located at distances lower than 2 meters to the listener’s head. The implemented approach uses the model presented by <a href="https://www.researchgate.net/publication/280979094_Partitioned_convolution_algorithms_for_real-time_auralization" target="_blank">Romblom & Cook (2008)</a>. 

These difference filters are implemented as IIR filters adjusted to match the described transfer function. The 3DTI Toolkit-BS includes two biquad filters for each ear, where the coefficients for these filters depend on both the distance of the sound source and its interaural azimuth. These filters are pre-calculated and stored in a file as a look- up table. Each entry of the look up table provides 10 coefficients that will be applied to the two biquad filters of each ear. The table is indexed by parameters distance and interaural azimuth with range and step configurable. By default, it is configured with distance ranges from 10 cm to 2 m stepping by 1cm and the azimuth angle ranges from 0º to 355º stepping 5º. 
This process can be considered as an HRIR correction since it is applied in series with the HRIR selected and interpolated in the previous stages of the pipeline. 

When the binaural spatialisation is performed, and as mentioned in previous described algorithms, a problem arises when the source or the listener are moving, since some audible artefacts can appear in the signal. In this particular case, those artefacts can be caused as the near-field correction filters have to change from frame to frame. In order to minimise this problem, at every frame each biquad filter is applied using both the previous and the new coefficients, and a linear cross-fading is performed to produce the output.

## Configuration Options

The methods provided by this service are as follows.

- **Add SOS Filter Table**: Add the table for computing SOS Filter
- **Get SOS Filter Coefficients**: Get IIR filter coefficients for SOS Filter, for one ear
