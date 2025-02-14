# Calibration

BeRTA provides a calibration option, which allows establishing the relationship between the system's dBFS (Decibels Full Scale) and the SPL (Sound Pressure Level) in dB of the output signal.

Calibration is essential in 3D audio systems to ensure that the perceived loudness of sounds matches real-world expectations. In digital audio, dBFS represents the signal level relative to the maximum possible amplitude, where 0 dBFS corresponds to the system's peak. However, real-world loudness is measured in dB SPL, which quantifies sound pressure in the air.

By calibrating BeRTA, users can map the digital signal levels to actual sound pressure levels, ensuring consistency across different playback environments. This is particularly useful for applications requiring precise loudness control.

## Calibration Process in BeRTA

The calibration process ensures that the system's digital audio levels (dBFS) correspond accurately to real-world sound pressure levels (dB SPL). It involves the following steps:

1. **Generating the Calibration Signal** – The process begins by sending the **Start Calibration** OSC command. This command triggers BeRTA to generate a pink noise signal at a specified dBFS level, which is provided as a parameter.

2. **Measuring the Output** – Using a sound level meter, the output signal from the headphones is measured. The measured value corresponds to the sound pressure level (dB SPL) of the emitted signal.

3. **Applying Calibration** – Once the measurement is taken, the user sets the calibration using the **SetCalibration** command in BeRTA. This step establishes the relationship between the system's digital audio levels (dBFS) and the actual sound pressure level (dB SPL).

4. **Verifying Calibration** – To ensure that the calibration has been correctly applied, the **PlayCalibrationTest** OSC command can be used. This command specifies the desired dB SPL level, and BeRTA emits a pink noise signal at that volume. The user can then verify the output level using a sound level meter.

After completing these steps, the system is calibrated, ensuring that the audio output maintains a consistent and accurate loudness reference for immersive and professional applications.

