In this section, we describe the various OSC commands used to control the listener. These commands not only manage basic parameters like position and orientation but also define the models applied for simulation. These models come with a range of controllable parameters and can simulate either just the listener’s head through a Head-Related Transfer Function (HRTF), or include the surrounding environment using a Binaural Room Impulse Response (BRIR), as the BRIR models the combined impulse response of a head situated within a room. See [listener models](/BRT-Documentation/library/listener/models) for more information.

<!----------------------------------------------------------------------------------->
---

## `/listener/location`

Set location of listener. Position is set in global x,y,z coordinates, expressed in meters. The new location is sent to all remotes except the one has sent the message.

#### Syntax

`/listener/location <string listener_id> <float x> <float y> <float z>`

`listener_id`: identifier assigned to the listener.

`x`: X global coordinate, expressed in metres. As a reference, X axis is positive to the front.

`y`: Y global coordinate, expressed in metres. As a reference, Y axis is positive to the left.

`z`: Z global coordinate, expressed in metres. As a reference, Z axis is positive to up.

#### Return

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/listener/location defaultListener 0 0 0`

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

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/listener/orientation defaultListener 0.1 0 0`

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

`/control/actionResult /listener/setHRTF <string listener_id> <bool set> <string description>`

The return confirmation refers to the `listener_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/listener/setHRTF defaultListener HRTF1`

BeRTA sends back to the sender: `/control/actionResult /listener/setHRTF defaultListener true "HRTF HRTF1 has been set to listener defaultListener"`

<!----------------------------------------------------------------------------------->
---


## `/listener/setBRIR`

Sets a BRIR using the BRIR id. The BRIR should be loaded previously using the command `/resources/loadBRIR`. After receiving this command `listener/enableSpatialization` is called automatically. The BRIR is assigned to all the models linked to the listener which may accept it. See the [Listener+Environment models](/BRT-Documentation/library/environment/rirModels) based on Room Impulse Responses for more infortmation. These models currently are:

* [Listener + Environment Model based on Direct Convolution with BRIR](/BRT-Documentation/library/environment/directBRIR). 

* [Listener + Environment Model based on convolution with BRIR in the Ambisonic domain](/BRT-Documentation/library/environment/ambisonicBRIR)

#### Syntax

`/listener/setBRIR <string listener_id> <string BRIR_id>`

`listener_id`: identifier assigned to the listener.

`BRIR_id`: identifier assigned to the BRIR.

#### Return

`/control/actionResult /listener/setBRIR <string listener_id> <bool set> <string description>`

The return confirmation refers to the `listener_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/listener/setBRIR defaultListener BRIR1`

BeRTA sends back to the sender: `/control/actionResult /listener/BRIR1 defaultListener true "BRIR BRIR1 has been set to listener defaultListener"`

<!----------------------------------------------------------------------------------->
---


## `/listener/setSOSFilters`

Sets a set of filters for the Near Field Compensation to be applied afgter the convolution with the HRTF. The filters are identified by an id, which was defined when they were previously loaded using the command `/resources/loadSOSFilters`. The NFC filters are assigned to all the models linked to the listener which may accept it. These currently are:

* [Listener Model based on Direct Convolution with HRTF](/BRT-Documentation/library/listener/directHRTF). 

* [Listener Model based on convolution with HRTF in the Ambisonic domain](/BRT-Documentation/library/listener/ambisonicHRTF)

#### Syntax

`/listener/setSOSFilters <string listener_id> <string NFCFilters_id>`

`listener_id`: identifier assigned to the listener.

`NFCFilters_id`: identifier assigned to the Near Field Compensation filters.

#### Return

`/control/actionResult /listener/setSOSFilters <string listener_id> <bool set> <string description>`

The return confirmation refers to the `listener_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/listener/setSOSFilters DefaultListener NFCFilters1`

BeRTA sends back to the sender: `/control/actionResult /listener/setSOSFilters defaultListener true "SOSFilters NFCFilters1 has been set to listener defaultListener"`

<!----------------------------------------------------------------------------------->
---


## `/listener/enableInterpolation`

In the case of a [Listener Model based on Direct Convolution with HRTF](/BRT-Documentation/library/listener/directHRTF), this command switch on or off the interpolation among HRIRs. In case it is on, a barycentric interpolation is applied among the three nearest HRIRs surrounding the source direction of arrival. See [HRTF grids]() for more information. If there are several models based on direct convolution with HRTF linked to the listener, this command will affect all of them.

#### Syntax

`/listener/enableInterpolation <string listener_id> <boolean enable>`

`listener_id`: identifier assigned to the listener.

`enable`: If true (1), enables interpolation of the HRTF of the listener. If false (0), interpolation is disabled.

#### Return

`/control/actionResult /listener/enableInterpolation <string listener_id> <bool enable> <string description>`

The return confirmation refers to the `listener_id`, indicating `enable=true` if the model has been enabled and `enable=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/listener/enableInterpolation DefaultListener true`

BeRTA sends back to the sender: `/control/actionResult /listener/enableInterpolation defaultListener true "Interpolation enabled to defaultListener"`

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

`/control/actionResult /listener/enableNearFieldEffect <string listener_id> <bool enable> <string description>`

The return confirmation refers to the `listener_id`, indicating `enable=true` if the model has been enabled and `enable=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender:``/listener/enableNearFieldEffect DefaultListener true`

BeRTA sends back to the sender: `/control/actionResult /listener/enableNearFieldEffect DefaultListener true "NearField Effect enabled to DefaultListener"`

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

`/control/actionResult /listener/enableITD <string listener_id> <bool enable> <string description>`

The return confirmation refers to the `listener_id`, indicating `enable=true` if the model has been enabled and `enable=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/listener/enableITD DefaultListener true`

BeRTA sends back to the sender: `/control/actionResult /listener/enableITD DefaultListener true "ITD enabled to DefaultListener"`

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

`/control/actionResult /listener/enableITD <string listener_id> <bool enable> <string description>`

The return confirmation refers to the `listener_id`, indicating `enable=true` if the model has been enabled and `enable=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/listener/enableParallaxCorrection DefaultListener true`

BeRTA sends back to the sender: `/control/actionResult /listener/enableParallaxCorrection DefaultListener true "Parallax Correction enabled to DefaultListener"`

<!----------------------------------------------------------------------------------->
---



## `/listener/enableModel`

This command switches on or off a listener model. When a listener model is disabled it does not process the input signal and provides silence at its output. This feature must be implemented in all listener models. The listener model to be enabled or disabled is didentified by an identifier defined in the used [settings file](/BRT-Documentation/setup/settingsFile).

#### Syntax

`/listener/enableModel <string listenerModel_id> <boolean enable>`

`listenerModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the model. If false (0), the model is disabled and its output will be silent.

#### Return

`/control/actionResult /listener/enableModel <string listenerModel_id> <bool enable> <string description>`

The return confirmation refers to the `listenerModel_id`, indicating `enable=true` if the model has been enabled and `enable=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/listener/enableModel DirectPath true`

BeRTA sends back to the sender: `/control/actionResult /listener/enableModel DirectPath true "Listener model DirectPath enabled."`

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

`/control/actionResult /listener/setAmbisonicsOrder <string listenerModel_id> <bool set> <string description>`

The return confirmation refers to the `listener_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/listener/setAmbisonicsOrder DirectPath 3`

BeRTA sends back to the sender: `/control/actionResult /listener/setAmbisonicsOrder DirectPath true "Ambisonics order set to 3"`

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

`/control/actionResult /listener/setAmbisonicsNormalization <string listenerModel_id> <bool set> <string description>`

The return confirmation refers to the `listener_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/listener/setAmbisonicsNormalization ReverbPath N3D`

BeRTA sends back to the sender: `/control/actionResult /listener/setAmbisonicsNormalization DirectPath true "Ambisonics normalization set to N3D"`. 


<!----------------------------------------------------------------------------------->
---



