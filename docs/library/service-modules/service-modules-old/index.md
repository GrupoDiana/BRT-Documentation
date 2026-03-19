# Old Services

!!! info "Note"
    Old services present in the library up to version 2.5.0.

Service modules are auxiliary components designed to manage and supply the essential resources required for process models to perform binaural audio synthesis algorithms effectively. These resources include various key elements, such as the Head-Related Transfer Function (HRTF), which is critical for simulating the direct sound path perceived by the listener model; the Binaural Room Impulse Response (BRIR), which enables accurate simulation of room reverberation effects; and directivity data, which is vital for capturing the directional characteristics of sound sources. Additionally, second-order filters are employed to accurately model near-field effects, ensuring precise spatial audio reproduction even in close-proximity scenarios. Together, these resources form the foundation for creating immersive and realistic binaural audio experiences.

Currently, five service modules are implemented:

- [HRTF](./service-hrtf.md): Stores head-related impulse responses indexed by azimuth and elevation.
- [BRIR](./service-hrbrir.md): Stores room-related impulse responses indexed by azimuth and elevation.
- [Directivity TF](./service-directivity-tf.md): Stores transfer functions of a sound source based on the position of the listener and the sources.
- SOS Coefficients: Stores coefficients for second-order sections of a filter, which can be fixed or vary based on distance, azimuth, and/or elevation.
- Ambisonincs BIR: Stores the impulse responses of the virtual loudspeakers in the ambisonic domains, in order to achieve a process with simultaneous impulse responses convolution and ambisonic decoding.
- General FIR: Stores impulse responses indexed by azimuth and elevation.
- Room: It stores the vertices and walls of a room. It also stores the absorption coefficients per band for each of the walls.
