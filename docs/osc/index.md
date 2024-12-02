# OSC commands defined in BRT   

Part of the BRT components is the definition of a set of OSC commands which the BRT Renderer application understands to dynamically configure the virtual scene. This is a complete list of all commands:


### OSC Control Commands 

- [`/control/bufferFrames`](control.md#controlbufferframes): Query the number of buffered audio frames.
- [`/control/connect`](control.md#controlconnect): Establish a connection with an IP and port.
- [`/control/disconnect`](control.md#controldisconnect): Terminate the connection and unsubscribe from updates.
- [`/control/frameSize`](control.md#controlframesize): Request the audio frame size per channel.
- [`/control/ping`](control.md#controlping): Check if BeRTA is listening by sending an echo.
- [`/control/sampleRate`](control.md#controlsamplerate): Request the current audio sample rate.
- [`/control/version`](control.md#controlversion): Retrieve the current BeRTA version.

### OSC Overall Commands Index

- [`/pause`](overall.md#pause): Pauses file sources and stops streaming from input channels.
- [`/play`](overall.md#play): Starts playback of file sources and streaming from input channels.
- [`/playAndRecord`](overall.md#playandrecord): Records spatialized sound and data during playback.
- [`/removeAllSources`](overall.md#removeallsources): Removes all sound sources.
- [`/stop`](overall.md#stop): Stops playback of file sources and streaming from input channels.


### Resources Commands Index

- [`/resources/enableWoodworthITD`](resources.md#resourcesenablewoodworthitd): Enables or disables the calculation of the ITD using the Woodworth formula.
- [`/resources/getBRIRInfo`](resources.md#resourcesgetbririnfo): Gets information about one of the loaded Binaural Room Impulse Responses (BRIR).
- [`/resources/getDirectivityTFInfo`](resources.md#resourcesgetdirectivitytfinfo): Gets information about one of the loaded Directivity Transfer Functions.
- [`/resources/getHRTFHeadRadius`](resources.md#resourcesgethrtfheadradius): Retrieves the head radius of one of the loaded HRTFs.
- [`/resources/getHRTFInfo`](resources.md#resourcesgethrtfinfo): Gets information about one of the loaded HRTFs.
- [`/resources/getSOSFiltersInfo`](resources.md#resourcesgetsosfiltersinfo): Gets information about one of the loaded sets of Near Field Compensation filters.
- [`/resources/loadBRIR`](resources.md#resourcesloadbrir): Loads a new Binaural Room Impulse Response from a SOFA file.
- [`/resources/loadDirectivityTF`](resources.md#resourcesloaddirectivitytf): Loads a Directivity Transfer Function from a SOFA file.
- [`/resources/loadHRTF`](resources.md#resourcesloadhrtf): Loads a new HRTF from a SOFA file and assigns an identifier.
- [`/resources/loadSOSFilters`](resources.md#resourcesloadsosfilters): Loads a set of Second Order Section filters from a SOFA file.
- [`/resources/removeBRIR`](resources.md#resourcesremovebrir): Removes a Binaural Room Impulse Response from the loaded resources.
- [`/resources/removeDirectivityTF`](resources.md#resourcesremovedirectivitytf): Removes a Directivity Transfer Function from the loaded resources.
- [`/resources/removeHRTF`](resources.md#resourcesremovehrtf): Removes an HRTF from the loaded resources.
- [`/resources/removeSOSFilters`](resources.md#resourcesremovesosfilters): Removes a set of Near Field Compensation filters from the loaded resources.
- [`/resources/restoreHRTFHeadRadius`](resources.md#resourcesrestorehrtfheadradius): Sets the HRTF head radius to the value stored in the SOFA file.
- [`/resources/setHRTFHeadRadius`](resources.md#resourcessethrtfheadradius): Sets the head radius to be stored in the HRTF.



### Source Models Commands Index

- [`/source/addLineIn`](source.md#sourceaddlinein): Adds a new source from an audio input channel.
- [`/source/enableDirectivity`](source.md#sourceenabledirectivity): Enable or disable the directivity of a sound source.
- [`/source/gain`](source.md#sourcegain): Set the gain of a sound source in dB.
- [`/source/location`](source.md#sourcelocation): Set the global location of a sound source.
- [`/source/loadSource`](source.md#sourceloadsource): Loads a new sound file and assigns an identifier.
- [`/source/loop`](source.md#sourceloop): Set loop mode for a sound source.
- [`/source/mute`](source.md#sourcemute): Mutes a specific sound source.
- [`/source/orientation`](source.md#sourceorientation): Set the orientation (yaw, pitch, roll) of a sound source.
- [`/source/pause`](source.md#sourcepause): Pauses a specific sound source.
- [`/source/play`](source.md#sourceplay): Plays a specific sound source.
- [`/source/playAndRecord`](source.md#sourceplayandrecord): Record a file of specified duration with the source's audio and spatial information.
- [`/source/removeSource`](source.md#sourceremovesource): Removes a sound source from the system.
- [`/source/setDirectivity`](source.md#sourcesetdirectivity): Assign a directivity to a sound source.
- [`/source/solo`](source.md#sourcesolo): Mutes all sources except the specified one.
- [`/source/stop`](source.md#sourcestop): Stops a specific sound source.
- [`/source/unmute`](source.md#sourceunmute): Unmutes a previously muted sound source.
- [`/source/unsolo`](source.md#sourceunsolo): Unmute all sources except the specified one.



### Listener Models Commands Index

- [`/listener/enableInterpolation`](listener.md#listenerenableinterpolation): Enable or disable interpolation among HRIRs.
- [`/listener/enableITD`](listener.md#listenerenableitd): Enable or disable the simulation of Interaural Time Difference (ITD).
- [`/listener/enableModel`](listener.md#listenerenablemodel): Enable or disable a listener model.
- [`/listener/enableNearFieldEffect`](listener.md#listenerenablenearfieldeffect): Enable or disable Near Field Compensation (NFC) with HRTF.
- [`/listener/enableParallaxCorrection`](listener.md#listenerenableparallaxcorrection): Enable or disable parallax correction for direction of arrival.
- [`/listener/location`](listener.md#listenerlocation): Set the global location of the listener in x, y, z coordinates.
- [`/listener/orientation`](listener.md#listenerorientation): Set the orientation of the listener in yaw, pitch, roll.
- [`/listener/setAmbisonicsNormalization`](listener.md#listenersetambisonicsnormalization): Set the Ambisonics normalization type.
- [`/listener/setAmbisonicsOrder`](listener.md#listenersetambisonicsorder): Set the Ambisonic encoding order.
- [`/listener/setBRIR`](listener.md#listenersetbrir): Set a Binaural Room Impulse Response (BRIR) for the listener.
- [`/listener/setHRTF`](listener.md#listenersethrtf): Set a Head-Related Transfer Function (HRTF) for the listener.
- [`/listener/setSOSFilters`](listener.md#listenersetsosfilters): Set filters for Near Field Compensation (NFC) after HRTF convolution.


### Environment Models Commands Index

- [`/environment/enableDirectPath`](environment.md#environmentenabledirectpath): Toggle the direct path source in the environment model.
- [`/environment/enableModel`](environment.md#environmentenablemodel): Enable or disable a specified environment model.
- [`/environment/enableReverbPath`](environment.md#environmentenablereverbpath): Toggle the reverb path sources in the environment model.
- [`/environment/setShoeBoxRoom`](environment.md#environmentsetshoeboxroom): Set up a shoebox-shaped room with specified dimensions.
- [`/environment/setWallAbsorption`](environment.md#environmentsetwallabsorption): Set absorption coefficients for a wall.

### Binaural Filters Commands Index