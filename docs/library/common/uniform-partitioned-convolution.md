:warning:*(Ready for review)*:warning:

For both anechoic and reverberation paths, the BRT utilizes the Uniformly Partition Overlap-Save (UPOLS) convolution method in the frequency domain, as described by <a href="https://www.researchgate.net/publication/280979094_Partitioned_convolution_algorithms_for_real-time_auralization" target="_blank">Wefers (2015)</a>. This FFT-based approach divides the impulse response (IR) into multiple blocks, each matching the frame size (N). These blocks are processed individually as separate IRs using a standard overlap-save technique. This method enables efficient convolution with long IRs by breaking them into shorter, more manageable segments. It is particularly advantageous for long IRs, such as BRIRs.

The figure shows the whole process of the UPOLS in the case of the HRIR, but it is the same for the BRIR. The partitions and FFTs of the HRTF are computed offline, where the whole HRTF is partitioned in blocks of length N (same length as the frame size) and stored. At run-time, for every audio frame, a new input buffer arrives,  the content of the input block buffer is shifted N samples to the left and the new input of N samples is then placed on the right (see bottom-left part of the diagram). Then, the whole input buffer is transformed into the frequency domain using a 2N-point real-to-complex FFT and stored in a delay-line, following an overlap-save scheme. In this way, in each audio frame, the input signal stored in the delay line is shifted up by one frame slot. During the audio frame, each delayed input buffer is convolved with each HRIR segment, by a multiplication in the complex domain. Finally, all the multiplications results are mixed and transformed back into the time-domain. To do so, the BRT implement a 2N-point complex-to-real IFFT, where the first N points are discarded, as we are using an overlap-save scheme. These sample removal and the size used for the FFT and IFFT of 2N-point are implemented to avoid aliasing in the time domain. In addition,  zero-padding is used to complete the signal buffer in case it is needed. 

Additionally, UPOLS assumes a stable impulse response, which is suitable for static sources and listeners. To accommodate the changes in the HRIR caused by the movement of sources or the listener, the BRT introduces a new delay line for the partitioned impulse response (top-left grey box). For each audio frame, both delay lines (HRIR and input buffer signal) are shifted by one frame slot. This allows a new HRIR, corresponding to the source's position, to be inserted into the delay line at each frame, with its first segment multiplied by the current audio frame. In subsequent frames, the HRIR remains in the delay line, convolving its remaining segments with the incoming audio frames. This method significantly reduces the number and intensity of artifacts caused by the movement of sources.

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/partitioned_convolution.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Uniformly Partition Overlap-Save (UPOLS).</p>
</div>

## Functional Overview

The methods of the UPOLS convolution class are designed to efficiently manage and execute frequency-domain convolution for both anechoic and reverberation paths. The process begins with the **Setup method**, where the input size, frequency block size, number of IR blocks, and memory usage settings are configured to prepare the system for operation. Convolution can then be performed using **ProcessUPConvolution method**, which processes the input signal with the given impulse responses (IRs) and outputs the result. For scenarios with moving source and listener, **Process UPConvolution With Memory** extends this functionality. These methods collectively enable efficient convolution with long IRs by leveraging the Uniformly Partitioned Overlap-Save (UPOLS) algorithm.


### Configuration Options

The methods provided by this class are as follows.

- **Setup**: Configures the system for UPOLS convolution with specified input and IR settings.
- **Process UPConvolution**: Executes convolution of the input signal with the provided impulse responses, generating the output.
- **Process UPConvolutionWithMemory**: Performs convolution while retaining intermediate states, for scenarios wirh moving sources and listener.
- **Calculate IFFT**: Computes the inverse FFT of a buffer for frequency-to-time domain transformation.
- **Reset**: Resets the system, clearing all internal states and memory for a fresh start.

<details>
<summary>For C++ developer</summary>
Section under construction
</details>