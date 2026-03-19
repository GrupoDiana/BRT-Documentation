# OSC commands defined in BRT   

Part of the BRT components is the definition of a set of OSC commands which the BRT Renderer application understands to dynamically configure the virtual scene. This is a complete list of all commands:


### OSC Control Commands 

- [`/control/actionResult`](control.md#controlactionresult): Retrieve the result of the action performed in another application after receiving an OSC command.
- [`/control/bufferFrames`](control.md#controlbufferframes): Query the number of buffered audio frames.
- [`/control/connect`](control.md#controlconnect): Establish a connection with an IP and port.
- [`/control/disconnect`](control.md#controldisconnect): Terminate the connection and unsubscribe from updates.
- [`/control/frameSize`](control.md#controlframesize): Request the audio frame size per channel.
- [`/control/ping`](control.md#controlping): Check if BeRTA is listening by sending an echo.
- [`/control/sampleRate`](control.md#controlsamplerate): Request the current audio sample rate.
- [`/control/version`](control.md#controlversion): Retrieve the current BeRTA version.
- [`/control/playCalibration`](control.md#controlplaycalibration): Start the calibration process.
- [`/control/setCalibration`](control.md#controlsetcalibration): Set the calibration values.
- [`/control/playCalibrationTest`](control.md#controlplaycalibrationtest): Play the calibration test sound at a specified volume.
- [`/control/stopCalibrationTest`](control.md#controlstopcalibrationtest): Stop the calibration test sound playback.
- [`/control/getSoundLevel`](control.md#controlgetsoundlevel): Get the current sound level.
- [`/control/setSoundLevelLimit`](control.md#controlsetsoundlevellimit): Set the sound level limit to a specified volume.
- [`/control/soundLevelAlert`](control.md#controlsoundlevelalert): Alert sent when the output sound level exceeds the threshold set in the limiter. 


### OSC Overall Commands 

- [`/play`](overall.md#play): Starts playback of file sources and streaming from input channels.
- [`/pause`](overall.md#pause): Pauses file sources and stops streaming from input channels.
- [`/stop`](overall.md#stop): Stops playback of file sources and streaming from input channels.
- [`/removeAllSources`](overall.md#removeallsources): Removes all sound sources.
- [`/playAndRecord`](overall.md#playandrecord): Records spatialized sound and data during playback.
- [`/record`](overall.md#record): Records spatialised sound and data without real-time playback.
- [`/enableModel`](overall.md#enablemodel): Enables or disables a model.
- [`/modelGain`](overall.md#modelgain): Sets the output gain of the model in dB.

### Resources Commands 

- [`/resources/loadHRTF`](resources.md#resourcesloadhrtf): Loads a new HRTF, with interpolation, from a SOFA file and assigns an identifier.
- [`/resources/loadHRTFRaw`](resources.md#resourcesloadhrtfraw): Loads a new HRTF, without interpolation, from a SOFA file and assigns an identifier.
- [`/resources/removeHRTF`](resources.md#resourcesremovehrtf): Removes an HRTF from the loaded resources.
- [`/resources/getHRTFInfo`](resources.md#resourcesgethrtfinfo): Gets information about one of the loaded HRTFs.
- [`/resources/setHRTFHeadRadius`](resources.md#resourcessethrtfheadradius): Sets the head radius to be stored in the HRTF.
- [`/resources/getHRTFHeadRadius`](resources.md#resourcesgethrtfheadradius): Retrieves the head radius of one of the loaded HRTFs.
- [`/resources/restoreHRTFHeadRadius`](resources.md#resourcesrestorehrtfheadradius): Sets the HRTF head radius to the value stored in the SOFA file.
- [`/resources/enableWoodworthITD`](resources.md#resourcesenablewoodworthitd): Enables or disables the calculation of the ITD using the Woodworth formula.
- [`/resources/loadBRIR`](resources.md#resourcesloadbrir): Loads a new Binaural Room Impulse Response from a SOFA file.
- [`/resources/removeBRIR`](resources.md#resourcesremovebrir): Removes a Binaural Room Impulse Response from the loaded resources.
- [`/resources/getBRIRInfo`](resources.md#resourcesgetbririnfo): Gets information about one of the loaded Binaural Room Impulse Responses (BRIR).
- [`/resources/loadDirectivity`](resources.md#resourcesloaddirectivity): Loads a Directivity Transfer Function from a SOFA file.
- [`/resources/loadDirectivityRaw`](resources.md#resourcesloaddirectivityraw): Loads a Directivity Transfer Function without interpolation from a SOFA file.
- [`/resources/removeDirectivity`](resources.md#resourcesremovedirectivity): Removes a Directivity Transfer Function from the loaded resources.
- [`/resources/getDirectivityInfo`](resources.md#resourcesgetdirectivityinfo): Gets information about one of the loaded Directivity Transfer Functions.

- [`/resources/loadDirectivityTF`](resources.md#resourcesloaddirectivitytf): Loads a Directivity Transfer Function from a SOFA file.<span style="font-size: 1em; color: grey; font-style: italic;">(Removed, use '/resources/loadDirectivity' instead.)</span>
- [`/resources/removeDirectivityTF`](resources.md#resourcesremovedirectivitytf): Removes a Directivity Transfer Function from the loaded resources.<span style="font-size: 1em; color: grey; font-style: italic;">(Removed, use '/resources/removeDirectivity' instead.)</span>
- [`/resources/getDirectivityTFInfo`](resources.md#resourcesgetdirectivitytfinfo): Gets information about one of the loaded Directivity Transfer Functions.<span style="font-size: 1em; color: grey; font-style: italic;">(Removed, use '/resources/getDirectivityInfo' instead.)</span>

- [`/resources/loadFilter`](resources.md#resourcesloadfilter): Load a new set of filter coefficients (IIR or FIR) from a SOFA file.
- [`/resources/removeFilter`](resources.md#resourcesremovefilter): Removes a set of filter coefficientes from the loaded resources.
- [`/resources/getFilterInfo`](resources.md#resourcesgetfilterinfo): Gets some information about one of the loaded set of filters coefficients.

- [`/resources/loadSOSFilters`](resources.md#resourcesloadsosfilters): Loads a set of SOS filters from a SOFA file.<span style="font-size: 1em; color: grey; font-style: italic;">(Deprecated, use '/resources/loadFilter' instead.)</span>
- [`/resources/removeSOSFilters`](resources.md#resourcesremovesosfilters): Removes a set of SOS filters from the loaded resources.<span style="font-size: 1em; color: grey; font-style: italic;">(Deprecated, use '/resources/removeFilter' instead.)</span>
- [`/resources/getSOSFiltersInfo`](resources.md#resourcesgetsosfiltersinfo): Gets information about one of the loaded sets of SOS filters.<span style="font-size: 1em; color: grey; font-style: italic;">(Deprecated, use '/resources/getFilterInfo' instead.)</span>

- [`/resources/loadRoom`](resources.md#resourcesloadroom): Loads a new room from a OBJ file
- [`/resources/loadShoeBoxRoom`](resources.md#resourcesloadshoeboxroom): Set up a shoebox-shaped room with specified dimensions.
- [`/resources/removeRoom`](resources.md#resourcesremoveroom): Removes a loaded room resource.
- [`/resources/setRoomWallAbsorption`](resources.md#resourcessetroomwallabsorption): Sets absorption coefficients for the walls of a room resource.
- [`/resources/enableRoomWall`](resources.md#resourcesenableroomwall): Enables or disables specific room walls in a room resource.


### Source Models Commands 

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
- [`/source/record`](source.md#sourcerecord): Record a file of specified duration with the source's audio and spatial information without real-time playback.
- [`/source/removeSource`](source.md#sourceremovesource): Removes a sound source from the system.
- [`/source/setDirectivity`](source.md#sourcesetdirectivity): Assign a directivity to a sound source.
- [`/source/solo`](source.md#sourcesolo): Mutes all sources except the specified one.
- [`/source/stop`](source.md#sourcestop): Stops a specific sound source.
- [`/source/unmute`](source.md#sourceunmute): Unmutes a previously muted sound source.
- [`/source/unsolo`](source.md#sourceunsolo): Unmute all sources except the specified one.



### Listener Models Commands 

- [`/listener/enableInterpolation`](listener.md#listenerenableinterpolation): Enable or disable interpolation among HRIRs.
- [`/listener/enableITD`](listener.md#listenerenableitd): Enable or disable the simulation of Interaural Time Difference (ITD).
- [`/listener/enableModel`](listener.md#listenerenablemodel): Enable or disable a listener model. <span style="font-size: 1em; color: grey; font-style: italic;">(Deprecated, use '/enableModel' instead.)</span>
- [`/listener/enableNearFieldEffect`](listener.md#listenerenablenearfieldeffect): Enable or disable Near Field Compensation (NFC) with HRTF.
- [`/listener/enableParallaxCorrection`](listener.md#listenerenableparallaxcorrection): Enable or disable parallax correction for direction of arrival.
- [`/listener/enableSpatialization`](listener.md#listenerenablespatialization): Enables or disables listener spatialization processing.
- [`/listener/location`](listener.md#listenerlocation): Set the global location of the listener in x, y, z coordinates.
- [`/listener/orientation`](listener.md#listenerorientation): Set the orientation of the listener in yaw, pitch, roll.
- [`/listener/setAmbisonicsNormalization`](listener.md#listenersetambisonicsnormalization): Set the Ambisonics normalization type.
- [`/listener/setAmbisonicsOrder`](listener.md#listenersetambisonicsorder): Set the Ambisonic encoding order.
- [`/listener/setBRIR`](listener.md#listenersetbrir): Set a Binaural Room Impulse Response (BRIR) for the listener.
- [`/listener/enableDistanceAttenuation`](listener.md#listenerenabledistanceattenuation): Enable or disable distance attenuation for listener models that support it.
- [`/listener/setDistanceAttenuationFactor`](listener.md#listenersetdistanceattenuationfactor): Set the attenuation factor used for distance attenuation.
- [`/listener/setHRTF`](listener.md#listenersethrtf): Set a Head-Related Transfer Function (HRTF) for the listener.
- [`/listener/setSOSFilters`](listener.md#listenersetsosfilters): Set filters for Near Field Compensation (NFC) after HRTF convolution.


### Environment Models Commands 

- [`/environment/enableModel`](environment.md#environmentenablemodel): Enable or disable a specified environment model. <span style="font-size: 1em; color: grey; font-style: italic;">(Deprecated, use '/enableModel' instead.)</span>
- [`/environment/enableDirectPath`](environment.md#environmentenabledirectpath): Toggle the direct path source in the environment model.
- [`/environment/enableReverbPath`](environment.md#environmentenablereverbpath): Toggle the reverb path sources in the environment model.

- [`/environment/enablePropagationDelay`](environment.md#environmentenablePropagationDelay): Enable or disable propagation delay.
- [`/environment/enableDistanceAttenuation`](environment.md#environmentenableDistanceAttenuation): Enable or disable distance attenuation.
- [`/environment/setDistanceAttenuationFactor`](environment.md#environmentsetdistanceattenuationfactor): Establish the attenuation factor, which is used for calculating attenuation.
- [`/environment/setRoom`](environment.md#environmentsetroom): Set the room resource to be used by the environment model.

- [`/environment/setReflectionOrder`](environment.md#environmentsetReflectionOrder): Set the order of reflection used in the simulation.
- [`/environment/setMaxDistanceSourcesToListener`](environment.md#environmentMaxDistanceSourcesToListener): Set the maximum distance between the sources and the listener.
- [`/environment/setFadeZoneMargin`](environment.md#environmentsetFadeZoneMargin): Set the size of the fade zone used by some models for simulation.



### Bilateral Filters Commands 

- [`/binauralFilter/enableModel`](bilateral-filter.md#binauralfilterenablemodel): Enable or disable a specified binaural filter. <span style="font-size: 1em; color: grey; font-style: italic;">(Deprecated, use '/enableModel' instead.)</span>
- [`/binauralFilter/setSOSFilter`](bilateral-filter.md#binauralfiltersetsosfilter): Set up sos filter for an specific listener. <span style="font-size: 1em; color: grey; font-style: italic;">(Deprecated, use '/bilateralFilter/setFilter' instead.)</span>
- [`/bilateralFilter/setFilter`](bilateral-filter.md#bilateralfiltersetfilter): Set up the filter coefficient database.
