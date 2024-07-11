## `/source/loadSource`

Add a new source, either loading a sound file (formats supported) or getting it from an audio input channel. Adding a new source does not imply playing it back. When adding the source an ID string is associated. That ID can be used later to control the source using other OSC commands.

### Syntax

`/source/loadSource <string source_id> <string source_path> <string source_model> [<string modelToConnectTo>]`

Specifying a sound file with `source_path`, a new sound file is loaded from the specified path and an identifier `source_id` is assigned to it. If there is already another source with the same identifier, it is substituted by the new one. Otherwise, a new source is created. The `source_model` has to be indicated, possible values are: `SimpleModel`, `DirectivityModel`. 

`modelToConnectTo` is optional and indicates to which listener model this source will be connected. If left blank the source will be connected to all listener models that have been indicated in the application configuration.

### Return

`/source/loadSource <string source_id> <bool loaded> <string description>`

The return message will refer to the `source_id`, indicating `loaded=true`if the source is successfuly loaded and `loaded=false` if not. In both cases a `description`is added to give more details. 

### Example

BeRTA receives: `/source/loadSource source1 speech.wav`
BeRTA sends back: `/source/loadSource source1 true success` or `/source/loadSource source1 false error: Can not find the file`
