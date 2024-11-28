# Listener Models based on HRTFs
:warning:*(In progress)*:warning:

The BRT library includes listening models that use head impulse responses (HRTF) to simulate the sound perceived by a user in a realistic way. These models work by convolving the signals from the sound sources with the binaural head impulse response (HRTF). This HRTF will typically be read from a SOFA file. 

Currently two such models have been implemented:

- [Based on convolution with HRTF](./listener-acoustic-model-hrtf.md): Simulates the direct path using convolution with HRTFs.
- [Based on convolution with HRTF in the Ambisonics domains](./listener-acoustic-model-ambisonic-hrtf.md): Simulates the direct path using convolution with HRTFs in the ambisonic domain.