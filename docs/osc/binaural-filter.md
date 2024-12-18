This section covers the OSC commands responsible for controlling the binaural filters using second order section filters.

## `/binauralFilter/setSOSFilter`

This command set the resource (the Second Order Section filters) to the simulation of the binaural filter model. 

#### Syntax

`/binauralFilter/setSOSFilter <string model_id> <string sosfilter_id>`

`model_id`: identifier assigned to the model.

`sosfilter_id`: identifier assigned to the SOS filter resource.


#### Return

`/control/actionResult /binauralFilter/setSOSFilter <string model_id> <bool success> <string description>`

The return confirmation refers to the `model_id`, indicating `success=true` if the set has been done successfully and `enabled=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/binauralFilter/setSOSFilter DefaultListener  Earmuffs`

BeRTA sends back to the sender: `/control/actionResult /binauralFilter/setSOSFilter DefaultListener true "SOS Filters (Earmuffs) has been set to listener (DefaultListener)"`


<!----------------------------------------------------------------------------------->
---

## `/binauralFilter/enableModel`

This command enables or disables the simulation of binaural filter.

#### Syntax

`/binauralFilter/enableModel <string sosfilter_id> <boolean enable>`

`sosfilter_id`: identifier assigned to the model.

`enable`: If true (1), enables the simulation of binaural filter. If false (0), disable the simulation of the binaural filter.

#### Return

`/control/actionResult /binauralFilter/enableModel <string sosfilter_id> <bool enabled> <string description>`

The return confirmation refers to the `sosfilter_id`, indicating `enabled=true` if the binaural filter has been enabled and `enabled=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/binauralFilter/enableModel Earmuffs true`

BeRTA sends back to the sender: `/control/actionResult /binauralFilter/enableModel Earmuffs true "Binaural filter (Earmuffs) enabled"`. 


<!----------------------------------------------------------------------------------->
---