# Calibration

BeRTA provides a calibration option, which allows establishing the relationship between the system's dBFS (Decibels Full Scale) and the SPL (Sound Pressure Level) in dB of the output signal.

Calibration is essential in 3D audio systems to ensure that the perceived loudness of sounds matches real-world expectations. In digital audio, dBFS represents the signal level relative to the maximum possible amplitude, where 0 dBFS corresponds to the system's peak. However, real-world loudness is measured in dB SPL, which quantifies sound pressure in the air.

By calibrating BeRTA, users can map the digital signal levels to actual sound pressure levels, ensuring consistency across different playback environments. This is particularly useful for applications requiring precise loudness control.

## Calibration Process in BeRTA  

The calibration process ensures that the system's digital audio levels (dBFS) correspond accurately to real-world sound pressure levels (dB SPL). It consists of **four steps**, two performed via software and two requiring physical measurements. In summary, the process involves generating a calibration signal within BeRTA, measuring its output with a sound level meter, applying the calibration settings in the system, and verifying that the calibration is accurate.

### Step-by-Step Calibration  

1. **Generating the Calibration Signal**: The process begins by sending the **Start Calibration**[^1] OSC command. This command triggers BeRTA to generate a pink noise signal at a specified dBFS level, which is provided as a parameter. *(Software step)*  

2. **Measure the output**: The user must measure the output signal from one of the headphone channels using a sound level meter. The measured value is the sound pressure level (dB SPL) of the output signal. We recommend setting the sound level meter to slow mode and used the C-weighting curve. *(Physical step)*.  

3. **Applying Calibration**: Once the measurement is taken, the user sets the calibration using the **SetCalibration**[^1] command in BeRTA. This step establishes the relationship between the system's digital audio levels (dBFS) and the actual sound pressure level (dB SPL). *(Software step)*  

4. **Verifying Calibration** *(optional)*: To ensure that the calibration has been correctly applied, the **PlayCalibrationTest**[^1] OSC command can be used. This command specifies the desired dB SPL level, and BeRTA emits a pink noise signal at that volume. The user can then verify the output level using a sound level meter. *(Software and Physical step)*  

After completing these steps, the system is calibrated, ensuring that the audio output maintains a consistent and accurate loudness reference for immersive and professional applications. 

[^1]: For more details, see [OSC Commands](/BRT-Documentation/osc/control/#calibration).


## Monitoring Sound Levels
Once calibrated, BeRTA will display the real-time signal level[^2] in dB SPL in the bottom-right section of the interface. Additionally, users can query the current signal level programmatically using the **GetSoundLevel**[^1] OSC command.  

To estimate the signal level, BeRTA calculates the RMS of the final digital signal and converts it to dBFS. Since the system has been calibrated using a reference SPL measurement provided by the user, BeRTA can apply the calibration factor to approximate the actual dB SPL level experienced by the listener.  

Although this estimation is generally accurate, it is important to note that it remains an approximation. Several factors can introduce discrepancies, including:  

- **Hardware limitations**: The analog components of the system, such as the amplifier and headphones, may not be capable of producing the expected sound pressure level, leading to saturation or distortion.  
- **Frequency response variations**: The system may not accurately reproduce certain frequency components due to the frequency response limitations of the playback devices, such as headphones or speakers.  
- **Non-linear distortions**: The listener’s headphones may introduce non-linear distortions that alter the actual perceived sound pressure level.  

Despite these potential variations, the real-time SPL estimation provides a useful reference for maintaining consistent loudness levels across different playback environments.  

[^2]: The displayed SPL values are estimations based on the calibration factor and may not account for all variations introduced by hardware limitations and environmental factors.