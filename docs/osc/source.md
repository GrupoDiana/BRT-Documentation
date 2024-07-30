
## `/source/loadSource`

Adds a new source loading a sound file (wav, mp3 and aif formats supported). Adding a new source does not imply playing it back. When adding the source an ID string is associated. That ID can be used later to control the source using other OSC commands.

#### Syntax

`/source/loadSource <string source_id> <string source_path> <string source_model> [<string modelToConnectTo>]`

Specifying a sound file with `source_path`, a new sound file is loaded from the specified path and an identifier `source_id` is assigned to it. If there is already another source with the same identifier, it is substituted by the new one. Otherwise, a new source is created. 

`source_model` indicates the source model to be used, possible values are: `SimpleModel`, `DirectivityModel`. See [Source Models](/BRT-Documentation/library/source/models/) for more details.

`modelToConnectTo` is optional and indicates to which listener model this source will be connected. If left blank, the source will be connected to all listener models that have been indicated in the application configuration.

#### Return

`/source/loadSource <string source_id> <bool loaded> <string description>`

The return message will refer to the `source_id`, indicating `loaded=true`if the source is successfuly loaded and `loaded=false` if not. In both cases a `description`is added to give more details. 

#### Example

BeRTA receives: `/source/loadSource source1 speech.wav`
BeRTA sends back: `/source/loadSource source1 true success` or `/source/loadSource source1 false error: Can not find the file`

## `/source/addLineIn`

Adds a new source getting it from an audio input channel. Adding a new source does not imply playing it back. When adding the source an ID string is associated. That ID can be used later to control the source using other OSC commands.

#### Syntax

`/source/addLineIn <string source_id> <int linein_channel> <string source_model> [<string modelToConnectTo>]`

`source_id`: identifier assigned to the sound source for further references to it

`linein_channel`: indicates the input channel of the selected audio device to be linked to this sound source. 

`source_model` indicates the source model to be used, possible values are: `SimpleModel`, `DirectivityModel`. See [Source Models](/BRT-Documentation/library/source/models/) for more details.

`modelToConnectTo` is optional and indicates to which listener model this source will be connected. If left blank, the source will be connected to all listener models that have been indicated in the application configuration.

#### Return

`/source/addLineIn <string source_id> <bool loaded> <string description>`

The return message will refer to the `source_id`, indicating `loaded=true`if the source is successfuly loaded and `loaded=false` if not. In both cases a `description`is added to give more details. 

#### Example

BeRTA receives: `/source/addLineIn source1 1 DirectivityModel`
BeRTA sends back: `/source/addLineIn source1 true success` or `/source/addLineIn source1 false error: not a valid channel`


## `/source/removeSource`

Removes a sound source from the system

#### Syntax

`/source/removeSource <string source_id>`

No command is returned. If the `source_id` does not exist, the command is ignored

#### Example

BeRTA receives: `/source/remove source1`

## `/source/play`

Plays a specific source, starting to send frames to the library from either the audio file or the input channel. In the first case, it starts from the beginning of the file unless the source was previously started and a `/source/pause` command was sent before. In such case it resumes at the point it was paused. If the `source_id` does not exist, the command is ignored

#### Syntax

`/source/play <string source_id>`

#### Return

An echo is returned to all subscribers: `/source/play <string source_id>`

#### Example

BeRTA receives: `/source/play source1`
BeRTA sends back: `/source/play source1`

## `/source/stop`

Stops a specific source. In the case of a source comming from a file, this resets the source so that the next `/source/play` command will start at the beginning of the file. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/stop <string source_id>`

#### Return

An echo is returned to all subscribers: `/source/stop <string source_id>`

#### Example

BeRTA receives: `/source/stop source1`
BeRTA sends back: `/source/stop source1`

## `/source/`

#### Syntax

#### Return

#### Example