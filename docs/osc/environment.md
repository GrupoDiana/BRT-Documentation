This section covers the OSC commands responsible for controlling the environment. These commands handle parameters related to the characteristics of the space, such as room geometry and the absorption profiles of the walls, particularly in models based on geometrical acoustics. These are the models implemented so far:

- SDN


<!----------------------------------------------------------------------------------->
---


## `/environment/enableModel`

This command switches on or off an environment model. When an environment model is disabled it does not process the input signal and provides silence at its output. this feature must be implemented in all environment models. The environment model to be enabled or disabled is didentified by an identifier defined in the [settings file](/BRT-Documentation/setup/settingsFile).

#### Syntax

`/environment/enableModel <string environmentModel_id> <boolean enable>`

`environmentModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the model. If false (0), the model is disabled and its output will be silent.

#### Return

<!-- An echo is returned to all subscribers: `/environment/enableModel <string environmentModel_id> <boolean enable>`
-->

#### Example

BeRTA receives: `/environment/enableModel SDN true`

<!--BeRTA sends back to all subscribers: `/environment/enableModel SDN true`
-->

<!----------------------------------------------------------------------------------->
---


## `/environment/setShoeBoxRoom`

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

<!-- An echo is returned to all subscribers: `/environment/setShoeBoxRoom <string environmentModel_id> <float length> <float width> <float height>`
-->

#### Example

BeRTA receives: `/environment/setShoeBoxRoom SDN 6 8 3`

<!--BeRTA sends back to all subscribers: `/environment/setShoeBoxRoom SDN 6 8 3`
-->

<!----------------------------------------------------------------------------------->
---


## `/environment/setWallAbsorption`

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

<!-- An echo is returned to all subscribers: `/environment/setShoeBoxRoom <string environmentModel_id> <float length> <float width> <float height>`
-->

#### Examples

BeRTA receives: `environment/setWallAbsorption SDN 0 0.5`

BeRTA receives: `environment/setWallAbsorption SDN 0 0.1 0.2 0.3 0.4 0.5 0.4 0.3 0.2 0.1`

<!--BeRTA sends back to all subscribers: `environment/setWallAbsorption SDN 0 0.5`
-->

<!----------------------------------------------------------------------------------->
---

## `/environment/enableDirectPath`

This command switches on or off the virtual source coresponding to the direct path, which actually is not virtual, but the real one. Disabling it will produce silence at its output. this feature can be implemented in all environment models. 

#### Syntax

`/environment/enableDirectPath <string environmentModel_id> <boolean enable>`

`environmentModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the direct path source. If false (0), mutes the direct path source.

#### Return

<!-- An echo is returned to all subscribers: `/environment/enableDirectPath <string environmentModel_id> <boolean enable>`
-->

#### Example

BeRTA receives: `/environment/enableDirectPath SDN false`

<!--BeRTA sends back to all subscribers: `/environment/enableDirectPath SDN false`
-->

<!----------------------------------------------------------------------------------->
---


## `/environment/enableReverbPath`

This command switches on or off all the virtual sources but the one of the direct path, which actually is not virtual. Disabling them will produce silence at all their outputs. this feature can be implemented in all environment models. 

#### Syntax

`/environment/enableReverbPath <string environmentModel_id> <boolean enable>`

`environmentModel_id`: identifier assigned to the model.

`enable`: If true (1), enables the reverb path sources. If false (0), mutes all reverb path sources.

#### Return

<!-- An echo is returned to all subscribers: `/environment/enableReverbPath <string environmentModel_id> <boolean enable>`
-->

#### Example

BeRTA receives: `/environment/enableReverbPath SDN false`

<!--BeRTA sends back to all subscribers: `/environment/enableReverbPath SDN false`
-->

<!----------------------------------------------------------------------------------->
---
