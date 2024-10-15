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
