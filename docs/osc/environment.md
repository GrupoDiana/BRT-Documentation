This section covers the OSC commands responsible for /controlling the environment. These commands handle parameters related to the characteristics of the space, such as room geometry and the absorption profiles of the walls, particularly in models based on geometrical acoustics. 

<!--These are the models implemented so far:- SDN->


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">


### **/environment/enableModel**
<div style="overflow: auto;">
    <span style="font-size: 0.8em; color: grey; font-style: italic; float: right; margin-right: 15px;">Deprecated since BeRTA v3.6.0, use '/enableModel' instead.</span>
</div>

This command switches on or off an environment model. When an environment model is disabled it does not process the input signal and provides silence at its output. this feature must be implemented in all environment models. The environment model to be enabled or disabled is didentified by an identifier defined in the [settings file](/BRT-Documentation/setup/settingsFile).

#### Syntax

`/environment/enableModel <string environmentModel_id> <boolean enable>`

`environmentModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the model. If false (0), the model is disabled and its output will be silent.

#### Return

`/control/actionResult /environment/enableModel <string environmentModel_id> <bool enable> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating `enable=true` if the model has been enabled and `enable=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/enableModel SDN true`

BeRTA sends back to the sender: `/control/actionResult /environment/enableModel SDN true "Environment model SDN enabled".`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/environment/enableDirectPath**
<div style="overflow: auto;">
  <span style="font-size: 0.8em; color: green; font-style: italic; float: left;">Compatible models: SDN</span>  
</div>

This command switches on or off the virtual source coresponding to the direct path. Disabling it will produce silence at its output. This feature is only implemented by some models. 

#### Syntax

`/environment/enableDirectPath <string environmentModel_id> <boolean enable>`

`environmentModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the direct path source. If false (0), mutes the direct path source.

#### Return

`/control/actionResult /environment/enableDirectPath <string environmentModel_id> <bool enabled> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating `enabled=true` if the direct path has been enabled and `enabled=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/enableDirectPath SDN false`

BeRTA sends back to the sender: `/control/actionResult /environment/enableDirectPath SDN true "Environment model SDN Direct path enabled"`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/environment/enableReverbPath**
<div style="overflow: auto;">
  <span style="font-size: 0.8em; color: green; font-style: italic; float: left;">Compatible models: SDN, ISM</span>  
</div>

This command switches on or off all virtual sources related to reverberation; it does not apply to the direct path. When deactivated, all outputs will be silent. This function can be implemented in all environment models.

#### Syntax

`/environment/enableReverbPath <string environmentModel_id> <boolean enable>`

`environmentModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the reverb path sources. If false (0), mutes all reverb path sources.

#### Return

`/control/actionResult /environment/enableReverbPath <string environmentModel_id> <bool enabled> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating `enabled=true` if the model direct path has been enabled and `enabled=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/enableReverbPath SDN false`

BeRTA sends back to the sender: `/control/actionResult /environment/enableReverbPath SDN true "Environment model SDN Reverb path enabled"`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">


### **/environment/enablePropagationDelay**
<div style="overflow: auto;">
  <span style="font-size: 0.8em; color: green; font-style: italic; float: left;">Compatible models: FreeField</span>  
  <span style="font-size: 0.8em; color: grey; font-style: italic; float: right; margin-right: 15px;">Available from BeRTA v3.6.0</span>
</div>

This command enables or disables the simulation of propagation delay throughout the entire environment, allowing users to control whether or not the system accounts for delays in signal propagation during audio rendering. 

#### Syntax

`/environment/enablePropagationDelay <string environmentModel_id> <boolean enable>`

`environmentModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the simulation of the propagation delay. If false (0), disable the simulation of the propagation delay.

#### Return

`/control/actionResult /environment/enablePropagationDelay <string environmentModel_id> <bool set> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/enablePropagationDelay FreeFieldEnvironmentModel true`

BeRTA sends back to the sender: `/control/actionResult /environment/enablePropagationDelay FreeFieldEnvironmentModel true "Propagation delay in the FreeFieldEnvironmentModel model enabled."`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/environment/enableDistanceAttenuation**
<div style="overflow: auto;">
  <span style="font-size: 0.8em; color: green; font-style: italic; float: left;">Compatible models: FreeField</span>  
  <span style="font-size: 0.8em; color: grey; font-style: italic; float: right; margin-right: 15px;">Available from BeRTA v3.6.0</span>
</div>

This command enables or disables the distance simulation in the environment model, if available. This simulation consists in applying a global attenuation, where doubling the distance between the source and the listener reduces the sound level by a predefined attenuation value.

#### Syntax

`/environment/enableDistanceAttenuation <string environmentModel_id> <boolean enable>`

`environmentModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the simulation of the distance attenuation. If false (0), disable the simulation of the distance attenuation.

#### Return

`/control/actionResult /environment/enableDistanceAttenuation <string environmentModel_id> <bool set> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details.

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/enableDistanceAttenuation FreeFieldEnvironmentModel true`

BeRTA sends back to the sender: `/control/actionResult /environment/enableDistanceAttenuation FreeFieldEnvironmentModel true "Distance attenuation enabled in the model (FreeFieldEnvironmentModel)."`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/environment/setDistanceAttenuationFactor**
<div style="overflow: auto;">
  <span style="font-size: 0.8em; color: green; font-style: italic; float: left;">Compatible models: FreeField</span>  
  <span style="font-size: 0.8em; color: grey; font-style: italic; float: right; margin-right: 15px;">Available from BeRTA v3.6.0</span>
</div>

This command allows you to change the attenuation factor per distance, in decibels. This is the amount of attenuation applied to the signal each time the distance is doubled from the reference distance. By default, this attenuation has a value of -6.0206 dB and the default reference distance is 1 meter. The value to set must always be negative. For more information, see the [section](/BRT-Documentation/library/environment-models/freefield-environment-model/) on the environment model.

#### Syntax

`/environment/setDistanceAttenuationFactor <string environmentModel_id> <float distanceAttenuationFactor>`

`environmentModel_id`: identifier assigned to the model.

`distanceAttenuationFactor`: a float value, expressed in dB, representing the distance attenuation factor. This value must be negative. 

#### Return

`/control/actionResult /environment/setDistanceAttenuationFactor <string environmentModel_id> <bool setAttenuation> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating if the attenuation factor has been set or not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/setDistanceAttenuationFactor FreeField -3`

BeRTA sends back to the sender: `/control/actionResult /environment/setDistanceAttenuationFactor FreeField true "Distance Attenuation Factor updated in the model (FreeField) to -3".`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/environment/setFadeZoneMargin**

<div style="overflow: auto;">
  <span style="font-size: 0.8em; color: green; font-style: italic; float: left;">Compatible models: ISM</span>  
  <span style="font-size: 0.8em; color: grey; font-style: italic; float: right; margin-right: 15px;">Available from BeRTA v3.9.0</span>
</div>

This command allows you to change the size of the fade zone used by the model for simulation. This value is used to fade out samples from virtual sources that enter that zone. Its value must be a positive number and is expressed in metres. By default, this parameter has a value of 0.6m (equivalent to 2ms). For more information, see the section on environment models, for example, the [ISM](/BRT-Documentation/library/environment-models/ism-environment-model/).

#### Syntax

`/environment/setFadeZoneMargin <string environmentModel_id> <float fadeZoneMargin>`

`environmentModel_id`: identifier assigned to the model.

`fadeZoneMargin`: a float value, which represents the size of the fading zone used in the model simulation. This value must be positive and is expressed in metres.

#### Return

`/control/actionResult /environment/setFadeZoneMargin <string environmentModel_id> <bool set> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating if the maximum distance between the sources and the listener has been set or not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/setFadeZoneMargin ISM 10.5`

BeRTA sends back to the sender: `/control/actionResult /environment/setFadeZoneMargin ISM true "Max Distance Sources to Listener updated in the model (ISM) to 10.5m.`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/environment/setMaxDistanceSourcesToListener**

<div style="overflow: auto;">
  <span style="font-size: 0.8em; color: green; font-style: italic; float: left;">Compatible models: ISM</span>  
  <span style="font-size: 0.8em; color: grey; font-style: italic; float: right; margin-right: 15px;">Available from BeRTA v3.9.0</span>
</div>


This command allows you to change the maximum distance between the sources and the listener used by the model for the simulation. This value is used to prune the number of image sources that are simulated. Its value must be a positive number and is expressed in metres. By default, this parameter has a value of 3.43m. For more information, see the section on environment models, for example, the [ISM](/BRT-Documentation/library/environment-models/ism-environment-model/).

#### Syntax

`/environment/setMaxDistanceSourcesToListener <string environmentModel_id> <float maxDistanceSourcesToListener>`

`environmentModel_id`: identifier assigned to the model.

`maxDistanceSourcesToListener`: a float value, representing the maximum distance between the sources and the listener to be used by the model simulation. This value must be positive.

#### Return

`/control/actionResult /environment/setMaxDistanceSourcesToListener <string environmentModel_id> <bool set> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating if the maximum distance between the sources and the listener has been set or not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/setMaxDistanceSourcesToListener ISM 10.5`

BeRTA sends back to the sender: `/control/actionResult /environment/setMaxDistanceSourcesToListener ISM true "Max Distance Sources to Listener updated in the model (ISM) to 10.5m.`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/environment/setReflectionOrder**

<div style="overflow: auto;">
  <span style="font-size: 0.8em; color: green; font-style: italic; float: left;">Compatible models: ISM</span>  
  <span style="font-size: 0.8em; color: grey; font-style: italic; float: right; margin-right: 15px;">Available from BeRTA v3.9.0</span>
</div>


This command allows you to change the reflection order used by the model for simulation. Its value determines the number of virtual sources used in the simulation. Its value must be a positive integer. By default, the reflection order is set to zero in the model, which means that the simulation is not performed. For more information, see the section on environment models, for example the [ISM](/BRT-Documentation/library/environment-models/ism-environment-model/).

#### Syntax

`/environment/setReflectionOrder <string environmentModel_id> <int reflectionOrder>`

`environmentModel_id`: identifier assigned to the model.

`reflectionOrder`: a int value, representing the reflection order to be used by the model simulation. This value must be positive.

#### Return

`/control/actionResult /environment/setReflectionOrder <string environmentModel_id> <bool set> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating if the reflection order has been set or not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/setReflectionOrder ISM 3`

BeRTA sends back to the sender: `/control/actionResult /environment/setReflectionOrder ISM true "Reflection Order updated in the model (ISM) to 3.`

<!----------------------------------------------------------------------------------->

<hr style="border:1px solid gray">

### **/environment/setRoom**

<div style="overflow: auto;">
  <span style="font-size: 0.8em; color: green; font-style: italic; float: left;">Compatible models: SDN, ISM</span>  
  <span style="font-size: 0.8em; color: grey; font-style: italic; float: right; margin-right: 15px;">Available from BeRTA v3.9.0</span>
</div>

Establishes a room in an environment model. The definition of the room and the model are referenced by two identifiers. The room must have been previously loaded using a command of the class /resources/.... After establishing the room, the environment model is reconfigured accordingly, which may require heavy processing and cause rendering to be interrupted. A room can be assigned to more than one environment model at a time. See environment models for more information. Currently, these are the listener models that accept it:

- [**SND Environment Model**](../library/environment-models/sdn-environment-model.md)
- [**ISM Model**](../library/environment-models/ism-environment-model.md)


#### Syntax

`/environment/setRoom <string environmentModel_id> <string room_id>`

`environmentModel_id`: identifier assigned to the model.

`room_id`: indentifier assigned to a room definition.

#### Return

`/control/actionResult /environment/setRoom <string room_id> <bool set> <string description>`

The return confirmation refers to the `room_id`, indicating if the room definition has been set or not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/setRoom SDN room1`

BeRTA sends back to the sender: `/control/actionResult /environment/setRoom SDN false "ERROR setting Room to the environment room1 not found in the Room list'].`

<!----------------------------------------------------------------------------------->