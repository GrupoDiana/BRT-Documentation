<!----------------------------------------------------------------------------------->

## `/listener/location`

Set location of listener. Position is set in global x,y,z coordinates, expressed in meters. The new location is sent to all remotes except the one has sent the message.

#### Syntax

`/listener/location <string listener_id> <float x> <float y> <float z>`

`listener_id`: identifier assigned to the listener.

`x`: X global coordinate, expressed in metres. As a reference, X axis is positive to the front.

`y`: Y global coordinate, expressed in metres. As a reference, Y axis is positive to the left.

`z`: Z global coordinate, expressed in metres. As a reference, Z axis is positive to up.

#### Return

An echo is returned to all subscribers excepting the sender: `/listener/location <string listener_id> <float x> <float y> <float z>`

#### Example

BeRTA receives: `/listener/location defaultListener 0 0 0`

BeRTA sends back to all subscribers excepting the sender: `/listener/location defaultListener 0 0 0`


<!----------------------------------------------------------------------------------->
---

## `/listener/orientation`

Sets orientation of the listener. Orientation is set in egocentric coordinates yaw, pitch and roll, expressed in radians and applied in that order (Yaw, Pitch and Roll). An orinetation of (0, 0, 0) corresponds to be oriented towards positive X. The new orientation is sent to all remotes except the one has sent the message.

#### Syntax

`/listener/orientation <string listener_id> <float yaw> <float pitch> <float roll>`

`listener_id`: identifier assigned to the listener.

`yaw`: Yaw, expressed in radians. Positive to the right.

`pitch`: Pitch, expressed in radians. Positive upwards.

`roll`: Roll, expressed in radians. Positive to the right.

#### Return

An echo is returned to all subscribers excepting the sender: `/listener/orientation <string listener_id> <float yaw> <float pitch> <float roll>`

#### Example

BeRTA receives: `/listener/orientation defaultListener 0.1 0 0`

BeRTA sends back to all subscribers excepting the sender: `/listener/orientation defaultListener 0.1 0 0`

<!----------------------------------------------------------------------------------->
---


## `/listener/setHRTF`

Sets an HRTF using the HRTF id. The HRTF should be load previously using the command `/resources/loadHRTF`. After set the HRTF the command `listener/enableSpatialization` is called automatically. The HRTF is assigned to all the listener models which may accept it. See [listener models](/BRT-Documentation/library/listener/models) for more information. Currently, these are the Listener models accepting it:

* [Listener Model based on Direct Convolution with HRTF](/BRT-Documentation/library/listener/directHRTF). 

* [Listener Model based on convolution with HRTF in the Ambisonic domain](/BRT-Documentation/library/listener/ambisonicHRTF)


#### Syntax

`/listener/setHRTF <string listener_id> <string HRTF_id>`

`listener_id`: identifier assigned to the listener.

`HRTF_id`: identifier assigned to the HRTF.

#### Return

An echo is returned to all subscribers: `/listener/setHRTF <string listener_id> <string HRTF_id>`

#### Example

BeRTA receives: `/listener/setHRTF defaultListener HRTF1`

BeRTA sends back to all subscribers: `/listener/setHRTF defaultListener HRTF1`

<!----------------------------------------------------------------------------------->
---


## `/listener/setBRIR`

Sets a BRIR using the BRIR id. The BRIR should be load previously using the command `/resources/loadBRIR`. The BRIR is assigned to all the models linked to the listener which may accept it. See the [Listener+Environment models](/BRT-Documentation/library/environment/rirModels) based on Room Impulse Responses for more infortmation. These models currently are:

* [Listener + Environment Model based on Direct Convolution with BRIR](/BRT-Documentation/library/environment/directBRIR). 

* [Listener + Environment Model based on convolution with BRIR in the Ambisonic domain](/BRT-Documentation/library/environment/ambisonicBRIR)

#### Syntax

`/listener/setBRIR <string listener_id> <string BRIR_id>`

`listener_id`: identifier assigned to the listener.

`BRIR_id`: identifier assigned to the BRIR.

#### Return

An echo is returned to all subscribers: `/listener/setBRIR <string listener_id> <string BRIR_id>`

#### Example

BeRTA receives: `/listener/setBRIR defaultListener BRIR1`

BeRTA sends back to all subscribers: `/listener/setBRIR defaultListener BRIR1`

<!----------------------------------------------------------------------------------->
---


## `/listener/setNFCFilters`

Sets a set of filters for the Near Field Compensation to be applied afgter the convolution with the HRTF. The filters are identified by an id, which was defined when they were previously loaded using the command `/resources/loadNFCFilters`. The NFC filters are assigned to all the models linked to the listener which may accept it. These currently are:

* [Listener Model based on Direct Convolution with HRTF](/BRT-Documentation/library/listener/directHRTF). 

* [Listener Model based on convolution with HRTF in the Ambisonic domain](/BRT-Documentation/library/listener/ambisonicHRTF)

#### Syntax

`/listener/setNFCFilters <string listener_id> <string NFCFilters_id>`

`listener_id`: identifier assigned to the listener.

`NFCFilters_id`: identifier assigned to the Near Field Compensation filters.

#### Return

An echo is returned to all subscribers: `/listener/setNFCFilters <string listener_id> <string NFCFilters_id>`

#### Example

BeRTA receives: `/listener/setNFCFilters DefaultListener NFCFilters1`

BeRTA sends back to all subscribers: `/listener/setNFCFilters DefaultListener NFCFilters1`

<!----------------------------------------------------------------------------------->
---


## `/listener/enableInterpolation`

In the case of a [Listener Model based on Direct Convolution with HRTF](/BRT-Documentation/library/listener/directHRTF), this command switch on or off the interpolation among HRIRs. In case it is on, a barycentric interpolation is applied among the three nearest HRIRs surrounding the source direction of arrival. See [HRTF grids]() for more information. If there are several models based on direct convolution with HRTF linked to the listener, this command will affect all of them.

#### Syntax

`/listener/enableInterpolation <string listener_id> <boolean enable>`

`listener_id`: identifier assigned to the listener.

`enable`: If true (1), enables interpolation of the HRTF of the listener. If false (0), interpolation is disabled.

#### Return

An echo is returned to all subscribers: `/listener/enableInterpolation <string listener_id> <boolean enable>`

#### Example

BeRTA receives: `/listener/enableInterpolation DefaultListener true`

BeRTA sends back to all subscribers: `/listener/enableInterpolation DefaultListener true`

<!----------------------------------------------------------------------------------->
---


## `/listener/enableNearFieldEffect`

This command switches on or off the  Near Field Compensation (NFC) applied together with the convolution with the HRTF. For the NFC to work, a set of NFC filters must first be loaded from a SOFA file (`/resources/loadNFCFilters`) and then assigned to the listener model (`/listener/setNFCFilters`). If there are several models with the NFC feature linked to the listener, this command will affect all of them. The models with this feature currently are:

* [Listener Model based on Direct Convolution with HRTF](/BRT-Documentation/library/listener/directHRTF). 

* [Listener Model based on convolution with HRTF in the Ambisonic domain](/BRT-Documentation/library/listener/ambisonicHRTF)

#### Syntax

`/listener/enableNearFieldEffect <string listener_id> <boolean enable>`

`listener_id`: identifier assigned to the listener.

`enable`: If true (1), enables NFC. If false (0), NFC is disabled.

#### Return

An echo is returned to all subscribers: `/listener/enableNearFieldEffect <string listener_id> <boolean enable>`

#### Example

BeRTA receives: `/listener/enableNearFieldEffect DefaultListener true`

BeRTA sends back to all subscribers: `/listener/enableNearFieldEffect DefaultListener true`

<!----------------------------------------------------------------------------------->
---


## `/listener/enableITD`

This command switches on or off the simulation of ITD separate from the intrinsic ITD which could be included as initial delays in HRIRs. For the ITD simnulation to make sense, HRIRs stored in the HRTF SOFA file should be aligned. Then the ITD can be extracted from the initial delays stored in the Data.Delay field of the SOFA structure. However, the ITD simulation can also use a spherical head modelto sintesize ITD. See also the commands `/resources/enableWoodworthITD` and `/resources/setHRTFHeadRadius` for more information. If there are several models with the ITD simulation feature, this command will affect all of them. The models with this feature currently are:

* [Listener Model based on Direct Convolution with HRTF](/BRT-Documentation/library/listener/directHRTF). 

* [Listener Model based on convolution with HRTF in the Ambisonic domain](/BRT-Documentation/library/listener/ambisonicHRTF)

#### Syntax

`/listener/enableITD <string listener_id> <boolean enable>`

`listener_id`: identifier assigned to the listener.

`enable`: If true (1), enables ITD. If false (0), ITD is disabled.

#### Return

An echo is returned to all subscribers: `/listener/enableITD <string listener_id> <boolean enable>`

#### Example

BeRTA receives: `/listener/enableITD DefaultListener true`

BeRTA sends back to all subscribers: `/listener/enableITD DefaultListener true`

<!----------------------------------------------------------------------------------->
---



## `/listener/enableParallaxCorrection`

This command switches on or off the parallax correction used to calculate the direction of arrival to each of the two ears. To do so, the head radius is needed and it is taken from the HRTF SOFA file or the radius provided by the `/resources/setHRTFHeadRadius` command. If there are several models with this parallax correction feature, this command will affect all of them. The models with this feature currently are:

* [Listener Model based on Direct Convolution with HRTF](/BRT-Documentation/library/listener/directHRTF). 

* [Listener Model based on convolution with HRTF in the Ambisonic domain](/BRT-Documentation/library/listener/ambisonicHRTF)

#### Syntax

`/listener/enableParallaxCorrection <string listener_id> <boolean enable>`

`listener_id`: identifier assigned to the listener.

`enable`: If true (1), enables parallax correction. If false (0), parallax correction is disabled.

#### Return

An echo is returned to all subscribers: `/listener/enableParallaxCorrection <string listener_id> <boolean enable>`

#### Example

BeRTA receives: `/listener/enableParallaxCorrection DefaultListener true`

BeRTA sends back to all subscribers: `/listener/enableParallaxCorrection DefaultListener true`

<!----------------------------------------------------------------------------------->
---



## `/listener/enableModel`

This command switches on or off a listener model. When a listener model is disabled it does not process the input signal and provides silence at its output. this feature must be implemented in all listener models. The listener model to be enabled or disabled is didentified by an identifier defined in the used [settings file](/BRT-Documentation/setup/settingsFile).

#### Syntax

`/listener/enableModel <string listenerModel_id> <boolean enable>`

`listenerModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the model. If false (0), the model is disabled and its output will be silent.

#### Return

An echo is returned to all subscribers: `/listener/enableModel <string listenerModel_id> <boolean enable>`

#### Example

BeRTA receives: `/listener/enableModel DirectPath true`

BeRTA sends back to all subscribers: `/listener/enableModel DirectPath true`

<!----------------------------------------------------------------------------------->
---



## `/listener/setAmbisonicsOrder`

Sets the order of the Ambisonic encoding used by the listener model. The listener model is didentified by an identifier defined in the used [settings file](/BRT-Documentation/setup/settingsFile). This command can be only used with models based on Ambisonics. If it is referred to a different model it will be ignored. The models with this feature currently are:

* [Listener Model based on convolution with HRTF in the Ambisonic domain](/BRT-Documentation/library/listener/ambisonicHRTF)

* [Listener + Environment Model based on convolution with BRIR in the Ambisonic domain](/BRT-Documentation/library/environment/ambisonicBRIR)

#### Syntax

`/listener/setAmbisonicsOrder <string listenerModel_id> <int ambisonicsOrder>`

`listenerModel_id`: identifier assigned to the model.

`ambisonicsOrder`: Ambisonic order. Possible values are 1, 2 and 3. 

#### Return

An echo is returned to all subscribers: `/listener/setAmbisonicsOrder <string listenerModel_id> <int ambisonicsOrder>`

#### Example

BeRTA receives: `/listener/setAmbisonicsOrder DirectPath 3`

BeRTA sends back to all subscribers: `/listener/setAmbisonicsOrder DirectPath 3`

<!----------------------------------------------------------------------------------->
---



## `/listener/setAmbisonicsNormalization`

Sets the normalization type used in the Ambisonic encoding used by the listener model. The listener model is didentified by an identifier defined in the used [settings file](/BRT-Documentation/setup/settingsFile). This command can be only used with models based on Ambisonics. If it is referred to a different model it will be ignored. The models based on Ambisonics currently are:

* [Listener Model based on convolution with HRTF in the Ambisonic domain](/BRT-Documentation/library/listener/ambisonicHRTF)

* [Listener + Environment Model based on convolution with BRIR in the Ambisonic domain](/BRT-Documentation/library/environment/ambisonicBRIR)

#### Syntax

`/listener/setAmbisonicsNormalization <string listenerModel_id> <string ambisonicsNormalization>`

`listenerModel_id`: identifier assigned to the model.

`ambisonicsNormalization`: Ambisonic normalization type. Possible values are: `N3D`, `SN3D`, `maxN`. 

#### Return

An echo is returned to all subscribers: `/listener/setAmbisonicsNormalization <string listenerModel_id> <string ambisonicsNormalization>`

#### Example

BeRTA receives: `/listener/setAmbisonicsNormalization ReverbPath N3D`

BeRTA sends back to all subscribers: `/listener/setAmbisonicsNormalization ReverbPath N3D`

<!----------------------------------------------------------------------------------->
---


