# Environment Models  

Environmental simulations play a crucial role in achieving realistic binaural audio rendering by replicating how sound interacts with its surroundings. These simulations model the acoustic behavior of spaces, including reflections, reverberation, and the effects of air propagation, to recreate an immersive auditory experience that closely resembles real-world scenarios. By combining environmental models with listener and source models, the BRT Library ensures accurate spatial audio reproduction.  

Currently, the BRT Library provides two environmental models:  

- [Free Field Environment Model](./freefield-environment-model.md): Simulates direct sound propagation in open spaces, accounting for propagation delay, distance-based attenuation, and filtering effects caused by the medium.  
- [SDN Environment Model](./sdn-environment-model.md): Implements room acoustics simulation using Scattering Delay Networks, a computationally efficient algorithm for modeling reflections and reverberation in enclosed spaces.  
- *ISM*: Simulates room reverberation using the Image Source Method *(Under development)*.
- *Hybrid: ISM + Convolution*: Simulates room reverberation where early reflections are modeled using the Image Source Method, and the reverberant tail is simulated through convolution with a BRIR *(Under development)*.