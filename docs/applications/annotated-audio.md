# Annotated Receiver Audio SOFA Convention

The AnnotatedReceiverAudio is a proposed <a href="https://www.sofaconventions.org/mediawiki/index.php/SOFA_(Spatially_Oriented_Format_for_Acoustics)" target="_blank">SOFA (Spatially Oriented Format for Acoustics)</a> convention to record and store (binaural) audio data at the receivers, annotated with geometric information. It is being currently discussed and considered as work in progress for the SOFA standard. 

SOFA is an open standard developed to store, exchange, and manage spatial acoustic data efficiently. It is widely used in the fields of 3D audio, virtual reality, and acoustics research to handle data such as Head-Related Transfer Functions (HRTFs), Binaural Room Impulse Responses (BRIRs), and other spatially oriented measurements.

The AnnotatedReceiverAudio convention aims at storing all the raw data during an experiment with participants to be reused for validation of new auditory models, or to reproduce the experiment. All the details can be seen <a href="https://www.sofaconventions.org/mediawiki/index.php/AnnotatedReceiverAudio" target="_blank">here</a> in the SOFA web site.


## Key Fields

- **Data.Receiver**: stores the audio samples received by the ears (receivers). 
- **M**: timestamp of measurements, it indicates the instant of time at which each of the captures is made.
- **ReceiverPosition**: specifies the 3D spatial positions of the ears (receivers) relative to the listener position.
- **ListenerPosition**: listener head positions.
- **ListenerUp**: listener orientations, up vector
- **ListenerView**: listener orientations, view vector.
- **EmitterPosition**: defines the 3D positions of the sound sources (emitters) relative to the source positions.
- **SourcePosition**: positions of the virtual set.
- **Response**: the subject's responses. If an experiment is being performed, it allows storing the subject's response under these conditions.

[BeRTA Renderer](berta-renderer/index.md) is able to record files using this AnnotatedReceiverAudio convention including the delivered binaural audio together with all the above information siynchronized with the audio. See [`/playAndRecord`](/BRT-Documentation/osc/overall#playandrecord) and [`/source/playAndRecord`](/BRT-Documentation/osc/source#sourceplayandrecord) for more information about the OSC commands to use this feature. these commands can also be sent by [BeRTA GUI](berta-gui.md).

An example of AnnotatedReceiverAudio sofa file can be download from our [repository](https://github.com/GrupoDiana/BRTLibrary/releases).

