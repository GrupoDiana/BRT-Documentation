# Listener Acoustic Models
:warning:*(Ready for review)*:warning:

Listener models are high-level modules of the BRT library that handle audio rendering. More specifically these modules try to simulate the binaural sound perceived by the subjects. These models can simulate only the direct sound, or only the reverberation, or all of them together. Currently, four models have been implemented, grouped into two categories:

## Listener Models based on HRTFs

The BRT library includes listening models that use head impulse responses (HRTF) to simulate the sound perceived by a user in a realistic way. These models work by convolving the signals from the sound sources with the binaural head impulse response (HRTF). This HRTF will typically be read from a SOFA file. Currently two such models have been implemented:

- [Based on convolution with HRTF](./hrtf-models/listener-acoustic-model-hrtf.md): Simulates the direct path using convolution with HRTFs.
- [Based on convolution with HRTF in the Ambisonics domains](./hrtf-models/listener-acoustic-model-ambisonic-hrtf.md): Simulates the direct path using convolution with HRTFs in the ambisonic domain.

## Listener & Environment Models based on BRIRs

The BRT library includes environment models that leverage room impulse responses (RIRs) to simulate realistic acoustic spaces. These models function by convolving the signals from sound sources with the binaural room impulse response (BRIR) of the room. The RIR will be read from a SOFA file and can contain impulse responses with both emmitter and receiver located in several locations in the room. Two models are currently implemented:

- [Based on convolution with BRIR](./rir-models/listener-acoustic-environment-model-brir.md): Simulates both the direct path and reverberation through convolution with a BRIR.

- [Based on convolution with BRIR in the Ambisonics domain](./rir-models/listener-acoustic-environment-model-ambisonic-brir.md): Simulates the direct path and reverberation using convolution with a BRIR in the ambisonic domain.

## Others##

- *Listener Auditory Model of Hearing Loss*: *(Under development)*.
- *Listener Model of Hearing Aid*: *(Under development)*.