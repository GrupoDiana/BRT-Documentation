# Bilateral Ambisonic Encoder Processor

The **Bilateral Ambisonic Encoder Processor** processes an audio source by applying delay and spatial expansion, simulating near-field effects through binaural filtering, and finally encoding the signals into left and right Ambisonic channels for spatial audio rendering.

## Architecture

The internal block diagram of this class is as follows:
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/BilateralAmbisonicEncoderProcessorInternalBlockDiagram.png" alt="Bilateral Ambisonic Encoder Processor - Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Bilateral Ambisonic Encoder Processor - Internal diagram.</p>
</div>



### Key Components and Flow

The **Bilateral Ambisonic Encoder Processor diagram** describes the internal workflow for encoding a source signal into Ambisonic channels for both left and right audio streams.

1. **AddDelayExpansionMethod**: The source signal is first passed through the `left` and `right` *AddDelayExpansionMethod* blocks. These methods apply a delay and spatial expansion to the signal to account for spatial positioning and propagation effects.

2. **Near Field Effect - Binaural Filter**: The processed `left` and `right` signals are then passed through a *Binaural Filter*.  This stage simulates near-field audio effects implementing binaural filtering from second-order stages.

3. **AmbisonicEncoder**: The filtered `left` and `right` signals are fed into their respective *AmbisonicEncoder* blocks. These encoders process the samples and output *N Ambisonic Channels* for both left and right streams.

4. **Output**: The final output consists of *Left N Ambisonics Channels* and *Right N Ambisonics Channels*, which represent spatially encoded versions of the source signal.


<details>
<summary>For C++ developer</summary>
(In progress)
</details>