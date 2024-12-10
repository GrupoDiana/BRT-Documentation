# Processing Modules
:warning:*(Section under construction)*:warning:

The **Processing Modules** encompass a variety of signal processing components used by high-level models to perform simulations and audio rendering tasks. These modules handle essential operations such as convolution, filtering, and encoding, acting as the core signal processing units for the BRT Library's functionality.  

### Modular Design  

To promote flexibility and ease of development, the implementation of these modules typically separates signal processing from the connectivity required within the library. This design choice facilitates both the development of new modules and their reuse in other contexts. Each processing module is generally implemented with two distinct classes or files [^1]:  

- A **signal processing class**, which is independent of the BRT Library’s architecture but may rely on common components such as type definitions and vector classes. E.g. *MySignalProcessing.hpp*
- A **processor class**, which extends the signal processing class by integrating connectivity features required for interaction with other library modules. E.g. *MySignalProcessingProcessor.hpp*  

For instance, in the case of the **HRTF Convolver**, the class `HRTFConvolver.hpp` implements the convolution logic without depending on connections to any other BRT module. On the other hand, the class `HRTFConvolverProcessor.hpp` integrates this functionality within the library while adding the necessary mechanisms for connecting to other modules.  


[^1]: There are processing modules that are used internally by other modules or signal processors. In this type of module, the class that adds connectivity functionality has not been implemented. 

### Reusability and Integration  

This modular design not only streamlines development within the BRT Library but also enhances the reusability of signal processing components. By decoupling core signal processing functions from library-specific connectivity, these modules can be easily integrated into external systems or reused in other audio processing contexts, making them highly versatile and portable.  


## List of signal processor modules

- [Ambisonic Domain Convolver & Processor](./ambisonic-domain-convolver.md):  The module performs ambisonic convolution for the N channels of each of the M different input sources, resulting in a monaural signal.
- **Ambisonic Encoder**: Encodes sound sources into an ambisonic format for spatial audio processing and rendering.  
- [Bilateral Ambisonic Encode & Processor](./bilateral-ambisonic-encoder.md):  bla bla
- **Binaural Filter**:  
- **DirectivityTFConvolver**:
- **Distance Attenuator**: Implements distance-based attenuation to simulate the natural decrease in sound intensity over distance.  
- **HRTFConvolver & Processor**:
- **NearFieldEffectProcessor**:
- **[Uniform Partitioned Convolution](uniform-partitioned-convolution.md)**:  
  Implements efficient frequency-domain convolution using uniformly partitioned overlap-save methods, optimized for long impulse responses.  
