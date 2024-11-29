# Listener & Environment Models Based on BRIRs  
:warning:*(Ready for review)*:warning:

The BRT Library includes environment models that use **Room Impulse Responses (RIRs)** to simulate realistic acoustic spaces. These models function by convolving the signals from sound sources with the **Binaural Room Impulse Response (BRIR)** of a specific room. A BRIR is an extension of the RIR that captures not only the acoustic characteristics of the room but also the binaural filtering effects caused by the listener's anatomy. This allows for a highly immersive audio experience, as the BRIR models how sound interacts with both the environment and the listener's physiology.

The BRIR typically includes both the direct sound path and the reverberation components of the sound field. It is recorded using microphones placed in or near the listener’s ears while a sound source emits test signals from different positions in the room. If the BRIR does not include information about the direct sound path, the rendering performed by the library will simulate only the acoustic characteristics of the environment, such as reflections and reverberation. This ensures flexibility in adapting to different BRIR datasets while maintaining accurate spatial rendering.

### Data Source for BRIRs  
The BRIR rendering models require BRIRsF data to be preloaded and preprocessed in the BRIR service module. For more details, see the documentation on the [BRIR service module](../../service-modules/service-hrbrir.md).  

The BRIRs used by these models are typically stored in files formatted according to the <a href="https://www.sofaconventions.org/mediawiki/index.php/SOFA_(Spatially_Oriented_Format_for_Acoustics)" target="_blank">SOFA (Spatially Oriented Format for Acoustics)</a>. This format is widely adopted in acoustic research for storing spatially oriented impulse responses. The BRT Library includes a dedicated module for reading and managing these files. This module is capable of loading BRIR data from SOFA files  and extracting the necessary data for simulation. For more details, see the documentation on the [Readers](../../readers/index.md).  


### Implemented Models  

Two BRIR-based environment models are currently available in the BRT Library:  

- **[Direct BRIR Convolution Model](./listener-acoustic-environment-model-brir.md)**: In this model, each sound source signal is convolved independently with the corresponding BRIR. This approach ensures the spatial resolution defined in the BRIR dataset, as each source has its own dedicated convolution process.  

- **[Ambisonic BRIR Convolution Model](./listener-acoustic-environment-model-ambisonic-brir.md)**: This model encodes all sound sources into an ambisonic format of order 1, 2, or 3 before performing convolution in the ambisonic domain. By convolving the ambisonic signals with the BRIR, this model achieves more efficient processing when rendering multiple sources, as it requires fewer BRIR datasets for the directions of arrival.  

### Refining the Impulse Response  

To optimize the impulse response, the BRT Library allows for windowing techniques that refine the BRIR. A rectangular window with smoothed edges can be applied using raised cosine fade-in and fade-out transitions, with a configurable slope. This process supports:  

- **Silencing Initial Impulse Components**: Useful for removing the direct sound path, which could be simulated separately using a [Listener Model](../index.md), or eliminating early reflections, which might be simulated using the Image Source Method in a hybrid approach.  
- **Truncating Late Impulse Components**: Reduces computational cost when working with long BRIRs and allows combining the BRIR convolution with simpler models for late reverberation tails, such as Feedback Delay Networks (FDN).  

This flexibility provides greater control over computational efficiency and ensures compatibility with hybrid modeling approaches.  

![Windowing a Room Impulse Response](/BRT-Documentation/assets/windowing.bmp "Windowing a Room Impulse Response")

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/windowing.jpg" alt="Windowing a Room Impulse Response" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Windowing a Room Impulse Response</p>
</div>
