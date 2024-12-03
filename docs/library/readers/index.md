:warning:*(Section under construction)*:warning:

## SOS filters

These difference filters are implemented as IIR filters adjusted to match the described transfer function. The BRT includes two biquad filters for each ear, where the coefficients for these filters depend on both the distance of the sound source and its interaural azimuth. These filters are pre-calculated and stored in a file as a look- up table. Each entry of the look up table provides 10 coefficients that will be applied to the two biquad filters of each ear. The table is indexed by parameters distance and interaural azimuth with range and step configurable. By default, it is configured with distance ranges from 10 cm to 2 m stepping by 1cm and the azimuth angle ranges from 0º to 355º stepping 5º. 

