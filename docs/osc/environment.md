This section covers the OSC commands responsible for /controlling the environment. These commands handle parameters related to the characteristics of the space, such as room geometry and the absorption profiles of the walls, particularly in models based on geometrical acoustics. 

<!--These are the models implemented so far:- SDN->


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">


### **/environment/enableModel**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Deprecated since BeRTA v3.6.0, use '/enableModel' instead.</span>

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


### **/environment/setShoeBoxRoom**

Sets up a shoebox room with six walls (four walls, plus floor and ceiling). The six walls are automatically created with the centre of the room at 0, 0, 0.  The following ID are assigned for each wall: 

- `0` front wall (positive x)

- `1` left wall (positive y)

- `2` right wall (negative y)

- `3` back wall (negative x)

- `4` floor (negative z)

- `5` ceiling (positive z)

#### Syntax

`/environment/setShoeBoxRoom <string environmentModel_id> <float length> <float width> <float height>`

`environmentModel_id`: identifier assigned to the model.

`length`: extension of the room in the X axis, expressed in metres.

`width`: extension of the room in the Y axis, expressed in metres.

`heigth`: extension of the room in the Z axis, expressed in metres.

#### Return

`/control/actionResult /environment/setShoeBoxRoom <string environmentModel_id> <bool set> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/setShoeBoxRoom SDN 6 8 3`

BeRTA sends back to the sender: `/control/actionResult /environment/setShoeBoxRoom SDN true "ShoeBox Room set to SDN (Length, Width, Height): [6, 8, 3]"`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">


### **/environment/setWallAbsorption**

Sets the absorption coeficients of a wall. These can be frequency independent, expressed with just one number between 0 and 1, or frequency dependent, expressed in thac case with nine numbers with absorptions corresponding to 62.5Hz, 125Hz, 250Hz, 500Hz, 1KHz, 2KHz, 4KHz, 8KHz and 16KHz.

#### Syntax

`/environment/setWallAbsorption <string environmentModel_id> <int wall_id> <float absorption_fullband>`

`/environment/setWallAbsorption <string environmentModel_id> <int wall_id> <float abs62> <float abs125> <float abs250> <float abs500> <float abs1K> <float abs2K> <float abs4K> <float abs8K> <float abs16K>`

`environmentModel_id`: identifier assigned to the model.

`wall_id`: index of the wall to which the absorption apllies.

`absoption_fulband`: absorption coefficient which is frequency independent.

`abs62`: absorption coefficient of the band centered at 62.5Hz.

`abs125`: absorption coefficient of the band centered at 125Hz.

`abs250`: absorption coefficient of the band centered at 250Hz.

`abs500`: absorption coefficient of the band centered at 500Hz.

`abs1K`: absorption coefficient of the band centered at 1KHz.

`abs2K`: absorption coefficient of the band centered at 2KHz.

`abs4K`: absorption coefficient of the band centered at 4KHz.

`abs8K`: absorption coefficient of the band centered at 8KHz.

`abs16K`: absorption coefficient of the band centered at 16KHz.

#### Return

`/control/actionResult /environment/setWallAbsorption <string environmentModel_id> <bool set> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Examples

BeRTA receives and echoes back to all subscribiers but the sender: `environment/setWallAbsorption SDN 0 0.1 0.2 0.3 0.4 0.5 0.4 0.3 0.2 0.1`

BeRTA sends back to the sender: `/control/actionResult /environment/setWallAbsorption SDN true "Wall absortion set to SDN, wall 0 : [0.1, ...]"`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/environment/enableDirectPath**

This command switches on or off the virtual source coresponding to the direct path, which actually is not virtual, but the real one. Disabling it will produce silence at its output. This feature can be implemented in all environment models. 

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

This command switches on or off all the virtual sources but the one of the direct path, which actually is not virtual. Disabling them will produce silence at all their outputs. This feature can be implemented in all environment models. 

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

This command enables or disables the simulation of propagation delay throughout the entire environment, allowing users to control whether or not the system accounts for delays in signal propagation during audio rendering. 

#### Syntax

`/environment/enablePropagationDelay <string environmentModel_id> <boolean enable>`

`environmentModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the simulation of the propagation delay. If false (0), disable the simulation of the propagation delay.

#### Return

`/control/actionResult /environment/enablePropagationDelay <string environmentModel_id> <bool enabled> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating `enabled=true` if the propagation delay has been enabled and `enabled=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/enablePropagationDelay FreeFieldEnvironmentModel true`

BeRTA sends back to the sender: `/control/actionResult /environment/enablePropagationDelay FreeFieldEnvironmentModel true "Propagation delay in the FreeFieldEnvironmentModel model enabled."`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/environment/enableDistanceAttenuation**

This command enables or disables the simulation of distance throughout the entire environment, which consists of applying a global attenuation, where doubling the distance between the source and the listener reduces the sound level by a predefined attenuation value.

#### Syntax

`/environment/enableDistanceAttenuation <string environmentModel_id> <boolean enable>`

`environmentModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the simulation of the distance attenuation. If false (0), disable the simulation of the distance attenuation.

#### Return

`/control/actionResult /environment/enableDistanceAttenuation <string environmentModel_id> <bool enabled> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating `enabled=true` if the distance attenuation has been enabled and `enabled=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/enableDistanceAttenuation FreeFieldEnvironmentModel true`

BeRTA sends back to the sender: `/control/actionResult /environment/enableDistanceAttenuation FreeFieldEnvironmentModel true "Distance attenuation enabled in the model (FreeFieldEnvironmentModel)."`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/environment/setDistanceAttenuationFactor**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.6.0</span>

This command allows you to change the attenuation factor per distance, in decibels. This is the amount of attenuation applied to the signal each time the distance is doubled from the reference distance. By default, this attenuation has a value of -6.0206 dB and the default reference distance is 1 meter. The value to set must always be negative. For more information, see the [section](/BRT-Documentation/library/environment-models/freefield-environment-model/) on the environment model.

#### Syntax

`/environment/setDistanceAttenuationFactor <string environmentModel_id> <float distanceAttenuationFactor>`

`environmentModel_id`: identifier assigned to the model.

`distanceAttenuationFactor`: a float value, expressed in dB, representing the distance attenuation factor. This value must be negative. 

#### Return

`/control/actionResult /environment/setDistanceAttenuationFactor <string environmentModel_id> <bool setAttenuation> <string description>`

The return confirmation refers to the `environmentModel_id`, indicating `setAttenuation=true` if the attenuation has been set and `setAttenuation=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/environment/setDistanceAttenuationFactor FreeField -3`

BeRTA sends back to the sender: `/control/actionResult /environment/setDistanceAttenuationFactor FreeField true "Distance Attenuation Factor updated in the model (FreeField) to -3".`

<!----------------------------------------------------------------------------------->