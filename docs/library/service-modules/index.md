# Service Modules
:warning:*(Section under construction)*:warning:

Service modules are auxiliary components designed to manage and supply the essential resources required for process models to perform binaural audio synthesis algorithms effectively. These resources include various key elements, such as the Head-Related Transfer Function (HRTF), which is critical for simulating the direct sound path perceived by the listener model; the Binaural Room Impulse Response (BRIR), which enables accurate simulation of room reverberation effects; and directivity data, which is vital for capturing the directional characteristics of sound sources. Additionally, second-order filters are employed to accurately model near-field effects, ensuring precise spatial audio reproduction even in close-proximity scenarios. Together, these resources form the foundation for creating immersive and realistic binaural audio experiences.

Currently, five service modules are implemented:

- [HRTF](service-hrtf.md)
- [BRIR](service-hrbrir.md)
- [Directivity TF](service-directivity-tf.md)
- [SOS FIlters](service-sos-filters.md)
- [Ambisonincs BIR](./service-ambisonic-bir.md)

