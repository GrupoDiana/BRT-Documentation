# Bilateral Filters Models

The objective of this model is to provide capabilities for filtering binaural audio signals. In other words, the filters can be adjusted separately for each channel, and these are processed independently. The implementation of this filtering is not spatially oriented, which means that the filter coefficients do not depend on the position of the source or the listener. Filtering is performed either using IIR filters based on second-order sections or by convolution with impulse responses. 

The BRT library currently offers two models of bilateral filters: 

- [SOS Bilateral Filter Model](./bilateral-sos-filter-model.md): Generic module to perform binaural filtering based on second-order sections, enabling the simulation of devices such as ear protection devices. The SOS coeficients are loaded from SOFA file via a dedicated service module (see [SOS Coeficients Service Module](../service-modules/service-sos-coefficients.md)).

- [FIR Bilateral Filter Model](./bilateral-fir-filter-model.md): Perform binaural filtering through convolution (in the frequency domain) with finite impulse responses, enabling applications such as **headphone compensation**, device equalization, or other binaural transfer function processing. The impulse responses are loaded from SOFA files via dedicated service modules (see [GeneralFIR](../service-modules/service-general-fir.md)).
