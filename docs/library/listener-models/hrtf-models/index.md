# Listener Models Based on HRTFs  

The BRT Library includes listener models that utilize **Head-Related Transfer Functions (HRTFs)** to realistically simulate the sound perceived by a listener. An HRTF is a mathematical representation of how an individual’s head, ears, and torso affect the sound arriving from a specific direction before reaching the eardrums. This representation captures how the human auditory system localizes sound in a 3D space, including cues such as interaural time differences (ITD) and interaural level differences (ILD). 

By convolving the signals from the sound sources with the binaural HRTFs, these models replicate the way sound interacts with the anatomy of the listener, offering a highly realistic auditory experience. This convolution process enables the simulation of the **direct sound path**, meaning the sound traveling directly from the source to the listener without reflections or reverberation.

### Data Source for HRTFs

These HRTF rendering models require HRTF data to be preloaded and preprocessed in the HRTF service module. For more details, see the documentation on the [HRTF service module](../../service-modules/service-hrtf.md).  

The HRTFs used by these models are typically read from files formatted in the <a href="https://www.sofaconventions.org/mediawiki/index.php/SOFA_(Spatially_Oriented_Format_for_Acoustics)" target="_blank">SOFA (Spatially Oriented Format for Acoustics)</a>. This standard is widely adopted in the audio research community for storing spatially oriented acoustic data, including head-related impulse responses.  

The BRT Library provides an integrated HRTF loader, capable of reading SOFA files and extracting the necessary data for simulation. For more details, see the documentation on the [Readers](../../readers/index.md).  

### Implemented Listener Models  

Currently, the following listener models based on HRTFs have been implemented:  

- [Direct HRTF Convolution Model](./listener-acoustic-model-hrtf.md): Simulates the direct sound path using convolution with HRTFs.  
- [HRTF Convolution model in the Ambisonics Domain](./listener-acoustic-model-ambisonic-hrtf.md): Simulates the direct sound path using convolution with HRTFs in the ambisonic domain.  

These models are designed to provide flexibility and precision for a variety of psychoacoustic experiments and immersive audio applications.
