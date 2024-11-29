# SDN Environment Model  
:warning:*(Section under construction)*:warning:

The **SDN Environment Model** implements an acoustic room simulation using **Scattering Delay Networks (SDNs)**. This approach models the acoustics of a room by employing acoustic reverberators, as described by Enso De Sena et al. in the paper <a href="https://ieeexplore.ieee.org/document/7113826" target="_blank">Efficient Synthesis of Room Acoustics via Scattering Delay Networks</a>.

Scattering Delay Networks simulate the acoustics of an enclosure using a network of delay lines connected by scattering junctions. The parameters of the model are derived from the physical properties of the simulated room, enabling a realistic approximation of the room's acoustic behavior. SDNs accurately model first-order reflections and make progressively coarser approximations for higher-order reflections. The algorithm supports unequal and frequency-dependent wall absorption, directional sound sources, and microphones, making it versatile for various room configurations.  
