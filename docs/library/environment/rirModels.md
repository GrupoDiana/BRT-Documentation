# Environment models based on Room Impulse Response

The BRT library includes environment models that leverage room impulse responses (RIRs) to simulate realistic acoustic spaces. These models function by convolving the signals from sound sources with the binaural room impulse response (BRIR) of the room. The RIR will be read from a SOFA file and can contain impulse responses with both emmitter and receiver located in several locations in the room. Two models are currently implemented:

* [Direct BRIR Convolution Model](directBRIR.md): In this model, each sound source signal is directly convolved with the corresponding BRIR. Each source needs a separate convolution with the BRIR, getting therefore the spatial resolution provided in the BRIR set 

* [Ambisonic BRIR Convolution Model](ambisonicBRIR.md): This model first encodes all sound sources into an ambisonic format of order 1, 2, or 3. The ambisonic signals are then convolved with the BRIR in the ambisonic domain. This method allows for more efficient processing  when many source have to be rendered and uses BRIR for only a few directions of arrival.

To further refine the impulse response, windowing can be applied using a rectangular window with smoothed edges achieved through raised cosine fade-in and fade-out. Silencing the initial part of the impulse response can be useful to remove the direct path, which could be simulated separately using a [Listener Model](/BRT-Documentation/library/listener/models/), or to eliminate early reflections, which might be simulated through the image source method in a hybrid approach. Truncating the late part of the impulse response can help limit the computational cost associated with convolving very long BRIRs or enable combining BRIR convolution with a simpler model for the late reverberation tail, such as a Feedback Delay Networks (FDN).

![Windowing a Room Impulse Response](/BRT-Documentation/assets/windowing.bmp "Windowing a Room Impulse Response")
