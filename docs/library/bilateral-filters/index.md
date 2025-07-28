# Bilatera Filters

The CBilateralFilter class provides a mechanism for filtering binaural audio signals, with support for configurable filter stages. It can process signals considering both static and dynamic listener and source positions. The class allows for flexibility in enabling or disabling the processor, adjusting filter coefficients, and resetting buffers for new processing cycles.

## Functional Overview

This model implements binaural filtering through [second-order sections (SOS)](../service-modules/service-sos-filters.md). It applies filtering based on the source and listener positions, allowing for spatially accurate sound filtering in 3D environments. The filter uses a chain of second-order filters for both the left and right ears and applies various methods to configure, enable, disable, and process the audio signals.

### Key Methods:

- **Setup** : Initializes the binaural filter by setting up the number of filter stages for both left and right ears.
- **Set Coefficients** : Stores the filter coefficients for the left and right ears.
- **Enable Processor** : Enables the filter processor, allowing it to process signals.
- **Disable Processor** : Disables the filter processor, preventing it from processing audio signals.
- **Is Processor Enabled** : Returns a boolean indicating whether the processor is enabled.
- **Process** : Filters the input audio signal using the binaural filter, considering the positions and orientations of the source and listener.
- **Reset Process Buffers** : Resets the buffers used in the processing chain for both left and right ears, clearing any previous signal data.