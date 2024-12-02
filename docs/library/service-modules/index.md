# Service Modules
:warning:*(Section under construction)*:warning:

Service modules are auxiliary components designed to manage and supply the essential resources required for process models to perform binaural audio synthesis algorithms effectively. These resources include various key elements, such as the Head-Related Transfer Function (HRTF), which is critical for simulating the direct sound path perceived by the listener model; the Binaural Room Impulse Response (BRIR), which enables accurate simulation of room reverberation effects; and directivity data, which is vital for capturing the directional characteristics of sound sources. Additionally, second-order filters are employed to accurately model near-field effects, ensuring precise spatial audio reproduction even in close-proximity scenarios. Together, these resources form the foundation for creating immersive and realistic binaural audio experiences.

Currently, five service modules are implemented:

- [HRTF](service-hrtf.md): Stores head-related impulse responses indexed by azimuth and elevation.
- [BRIR](service-hrbrir.md):  Stores room-related impulse responses indexed by azimuth and elevation.
- [Directivity TF](service-directivity-tf.md): Stores transfer functions of a sound source based on the position of the listener and the sources.
- [SOS Filters](service-sos-filters.md): Stores coefficients for second-order sections of a filter, which can be fixed or vary based on distance, azimuth, and/or elevation.
- [Ambisonincs BIR](./service-ambisonic-bir.md): Stores the impulse responses of the virtual loudspeakers in the ambisonic domains, in order to achieve a process with simultaneous impulse responses convolution and ambisonic decoding.
