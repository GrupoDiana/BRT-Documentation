# Ambisonic BRIR
:warning:*(Section under construction)*:warning:
 
The Ambisonic BRIR Convolution Module of the Binaural Rendering Toolbox enables the spatial audio rendering of multiple sound sources in an ambisonic environment. By leveraging the convolution with Binaural Room Impulse Responses (BRIR) in the ambisonic domain, this module processes the input sources considering their positions and the listener's position and orientation. It supports ambisonic encoding of orders 1, 2, or 3, and uses to decode virtual loudspeakers located at the vertices of regular polyhedra (octahedron for first-order, icosahedron for second-order, and dodecahedron for third-order). In order to save some convolutions, the module convolves the ambisonic signal directly with the ambisonic mix of the virtual loudspeakers' impulse responses, subsequently mixing all right and left channels separately. Uniformly partitioned convolution is used to compute the convolution in the frequency domain.

## Architecture

The architecture of the Ambisonic BRIR Convolution Module is designed to efficiently handle the spatial audio rendering process by utilizing linear operations. Here’s a step-by-step overview of the architecture:

![Ambisonic BRIR model architecture](/BRT-Documentation/assets/AmbisonicBRIR.png "Ambisonic BRIR model architecture")

1. **Input Handling**: The module accepts a set of audio sources along with their positions in space, as well as the position and orientation of the listener.
2. **Ambisonic Encoding**: The relative positions of each source with respect to the listener are used to encode the sources into an ambisonic format of the specified order (1st, 2nd, or 3rd).
3. **Virtual Loudspeaker**: Impulse responses are got from the BRIR file for those positions of teh virtual loudspeakers, arranged at the vertices of a regular polyhedron corresponding to the ambisonic order.
4. **Convolution**: Rather than convolving each virtual loudspeaker signal with the HRIR individually, the module convolves the ambisonic signal directly with the ambisonic mix of the BRIRs for the virtual loudspeakers.
5. **Channel Mixing**: The convoluted signals are mixed into separate left and right channels, resulting in the final binaural audio output.

## BRIR Format

The BRIR (Binaural Room Impulse Response) should be given as a SOFA format file. Any convention with data type FIR or FIR-E is supported as long as only one emmitter is used. Depending on the Ambisonic order, the direction of the virtual loudspeakers are searched in the BRIR file. If some of them is not found, an interpolation among the three closest directions is performed. If different listener positions are provided, a different set of virtual loudspeakers will be created for every listener position. When rendering, the one with the closest position to the listener is selected to convolve. To determine if two positions are the same, a resolution of 1 meter is used.   

Depending on how the BRIR is captured, several cases may be considered

* One fixed listener with several source positions around it

* One listener position with different orientations and one fixed source

* Different listener positions with sources around
