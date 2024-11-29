
The AnnotatedReceiverAudio convention is part of the SOFA (Spatially Oriented Format for Acoustics) standard. SOFA (Spatially Oriented Format for Acoustics) is an open standard developed to store, exchange, and manage spatial acoustic data efficiently. It is widely used in the fields of 3D audio, virtual reality, and acoustics research to handle data such as Head-Related Transfer Functions (HRTFs), Binaural Room Impulse Responses (BRIRs), and other spatially oriented measurements.


The annotated audio file aims at storing all the raw data during an experiment with participants to be reused for validation of new auditory models, or to reproduce the experiments. All the details can be seen <a href="https://www.sofaconventions.org/mediawiki/index.php/AnnotatedReceiverAudio" target="_blank">here</a> in the SOFA web site.

**Key Fields in the AnnotatedReceiverAudio Convention:**

- Data.IR: stores the impulse responses measured at the receivers. 
- Data.SamplingRate: indicates the sampling rate of the audio data, ensuring proper playback and analysis.
- ReceiverPosition: specifies the 3D spatial positions of the receivers relative to a reference point, using Cartesian coordinates.
- ReceiverOrientation:  describes the orientation of the receivers in 3D space, often provided as a rotation matrix or quaternion.
- EmitterPosition: defines the 3D position of the sound sources (emitters) relative to the reference coordinate system.
- EmitterOrientation: describes the orientation of the sound sources in 3D space.

