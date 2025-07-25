# Source Models

Each monaural sound source to be rendered requires the instantiation of a source model. Each sound source must be connected to a source model that can transform the sound depending on the source position (location and orientation).
These models therefore serve as the main library entry points for applications during rendering. At a minimum, applications must provide the audio samples of each source for each frame and, if applicable, update their position and orientation.

There are currently two fundamental source models and one virtual source model:

- [Omnidirectional Model](simple-omnidirectional-source-model.md)
- [Directivity Model](directivity-source-model.md)
- [Virtual Sources Model](virtual-source-model.md)




