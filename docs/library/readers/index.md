:warning:*(Ready for review)*:warning:

The SOFA Reader class is designed to handle the reading and parsing of audio resource data from files in the SOFA (Spatially Oriented Format for Acoustics) format. SOFA is an open standard used for storing spatial acoustic data, such as Head-Related Transfer Functions (HRTFs), Binaural Room Impulse Responses (BRIRs), and other directional audio characteristics. This format is widely adopted in spatial audio applications due to its flexibility and compatibility with 3D audio systems.

This reader is provided to simplify the resource loading process for the BRT (Binaural Renderer Toolkit). The BRT includes a set of service models that offer interfaces for integrating spatial audio data into the library. The SOFA Reader bridges the gap between the SOFA format and the BRT's service models, ensuring efficient data loading and compatibility. By offering a way to load resources into the BRT, the SOFA Reader ensures that spatial acoustic data is accurately and efficiently prepared for use within the renderer, enhancing the overall workflow and integration process. 

**Conventions implemented by this reader are**:


- <a href="https://www.sofaconventions.org/mediawiki/index.php/SimpleFreeFieldHRIR" target="_blank">SimpleFreeFieldHRIR</a> convention for the HRTF and BRIR SOFA files.
- <a href="https://www.sofaconventions.org/mediawiki/index.php/FreeFieldDirectivityTF" target="_blank">FreeFieldDirectivityTF</a> convention for the Directivity SOFA files.
- <a href="https://www.sofaconventions.org/mediawiki/index.php/SimpleFreeFieldHRSOS" target="_blank">SimpleFreeFieldHRSOS</a> convention for the SOS filters SOFA files.


## Configuration Options

The methods provided by this class are as follows.

- **Get Sample Rate From Sofa**: Extracts the sample rate specified in a given SOFA file.  

- **Read HRTF From Sofa**: Reads the HRTF data from a SOFA file and loads into a service class, supporting specific spatial resolution and extrapolation methods.  

- **Read SOS Filters From Sofa**: Reads the near-field compensation SOS filters from a SOFA file and loads into the corresponding service class.  

- **Read DirectivityTF From Sofa**: Loads source directivity transfer functions from a SOFA file, configuring the spatial resolution and extrapolation method.  

- **Read BRIR From Sofa**: Extracts BRIR data from a SOFA file, loading into a service class while applying windowing parameters for smooth transitions.  



