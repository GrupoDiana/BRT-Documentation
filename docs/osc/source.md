<!----------------------------------------------------------------------------------->

## `/source/loadSource`

Adds a new source loading a sound file (wav, mp3 and aif formats supported). Adding a new source does not imply playing it back. When adding the source an ID string is associated. That ID can be used later to control the source using other OSC commands.

#### Syntax

`/source/loadSource <string source_id> <string source_path> <string source_model> [<string modelToConnectTo>]`

Specifying a sound file with `source_path`, a new sound file is loaded from the specified path and an identifier `source_id` is assigned to it. If there is already another source with the same identifier, it is substituted by the new one. Otherwise, a new source is created. 

`source_model` indicates the source model to be used, possible values are: `SimpleModel`, `DirectivityModel`. See [Source Models](/BRT-Documentation/library/source/models/) for more details.

`modelToConnectTo` is optional and indicates to which listener model this source will be connected. If left blank, the source will be connected to all listener models that have been indicated in the application configuration.

#### Return

`/source/loadSource <string source_id> <bool loaded>`

The return message will refer to the `source_id`, indicating `loaded=true`if the source is successfuly loaded <!--and `loaded=false` if not. In both cases a `description`is added to give more details. -->

#### Example

BeRTA receives: `/source/loadSource source1 speech.wav`
BeRTA sends back: `/source/loadSource source1 true` <!--or `/source/loadSource source1 false error: Can not find the file`-->

<!----------------------------------------------------------------------------------->
---



## `/source/addLineIn`

Adds a new source getting it from an audio input channel. Adding a new source does not imply playing it back. When adding the source an ID string is associated. That ID can be used later to control the source using other OSC commands.

#### Syntax

`/source/addLineIn <string source_id> <int linein_channel> <string source_model> [<string modelToConnectTo>]`

`source_id`: identifier assigned to the sound source for further references to it

`linein_channel`: indicates the input channel of the selected audio device to be linked to this sound source. 

`source_model` indicates the source model to be used, possible values are: `SimpleModel`, `DirectivityModel`. See [Source Models](/BRT-Documentation/library/source/models/) for more details.

`modelToConnectTo` is optional and indicates to which listener model this source will be connected. If left blank, the source will be connected to all listener models that have been indicated in the application configuration.

#### Return

A confirmation is sent back to the sender: `/source/addLineIn <string source_id> <bool loaded> <string description>` 
The return message will refer to the `source_id`, indicating `loaded=true`if the source is successfuly loaded <!--and `loaded=false` if not. In both cases a `description`is added to give more details. 

#### Example

BeRTA receives: `/source/addLineIn source1 1 DirectivityModel`

BeRTA sends back to the sender: `/source/addLineIn source1 true` <!--or `/source/addLineIn source1 false error: not a valid channel`

<!----------------------------------------------------------------------------------->
---


## `/source/removeSource`

Removes a sound source from the system

#### Syntax

`/source/removeSource <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

A confirmation is sent back to all subscribers: `/source/removeSource <string source_id> <bool removed>` 
The return message will refer to the `source_id`, indicating `removed=true`if the source is successfuly removed. If the source does not exist, the command is ignored

#### Example

BeRTA receives: `/source/remove source1`

BeRTA sends back to all subscribiers: `/source/remove source1 true`

<!----------------------------------------------------------------------------------->
---


## `/source/play`

Plays a specific source, starting to send frames to the library from either the audio file or the input channel. In the first case, it starts from the beginning of the file unless the source was previously started and a `/source/pause` command was sent before. In such case it resumes at the point it was paused. If the `source_id` does not exist, the command is ignored

#### Syntax

`/source/play <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

An echo is returned to all subscribers: `/source/play <string source_id>`

#### Example

BeRTA receives: `/source/play source1`

BeRTA sends back to all subscribers: `/source/play source1`

<!----------------------------------------------------------------------------------->
---


## `/source/stop`

Stops a specific source. In the case of a source comming from a file, this resets the source so that the next `/source/play` command will start at the beginning of the file. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/stop <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

An echo is returned to all subscribers: `/source/stop <string source_id>`

#### Example

BeRTA receives: `/source/stop source1`

BeRTA sends back to all subscribers: `/source/stop source1`

<!----------------------------------------------------------------------------------->
---


## `/source/pause`

Pauses a specific source. In the case of a source comming from a file, the next `/source/play` command will resume, sarting at the same point where it was paused. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/pause <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

An echo is returned to all subscribers: `/source/pause <string source_id>`

#### Example

BeRTA receives: `/source/pause source1`

BeRTA sends back to all subscribers: `/source/pause source1`

<!----------------------------------------------------------------------------------->
---


## `/source/mute`

Mutes a specific source. In the case of a source comming from a file, the application keeps getting the sound stream from the file at the same rate, but silently. The `/source/unmute` command will make the source sound again. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/mute <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

No return is sent back.
<!--An echo is returned to all subscribers: `/source/mute <string source_id>`
-->
#### Example

BeRTA receives: `/source/mute source1`

<!--BeRTA sends back to all subscribers: `/source/mute source1`
-->
<!----------------------------------------------------------------------------------->
---


## `/source/unmute`

Unmutes a specific source. In the case of a source comming from a file previously muted, the application has kept getting the sound stream from the file at the same rate, but silently. The `/source/unmute` command will make the source sound again. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/unmute <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

No return is sent back.
<!--An echo is returned to all subscribers: `/source/unmute <string source_id>`
-->
#### Example

BeRTA receives: `/source/unmute source1`

<!--BeRTA sends back to all subscribers: `/source/unmute source1`
-->
<!----------------------------------------------------------------------------------->
---


## `/source/solo`

Mutes all sources but the the one indicated by this command. In the case of sources comming from files, the application keeps getting the sound stream from them at the same rate, but silently. The `/source/unsolo` command will make all the sources sound again. Individual commands `/source/unmute` addressed to those sources muted as a consecuence of an `/source/solo`command will have the same effect making them sound again. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/solo <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

No return is sent back.
<!--An echo is returned to all subscribers: `/source/solo <string source_id>`
-->
#### Example

BeRTA receives: `/source/solo source1`

<!--BeRTA sends back to all subscribers: `/source/solo source1`
-->
<!----------------------------------------------------------------------------------->
---


## `/source/unsolo`

Unmutes all sources but the the one indicated by this command. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/unsolo <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

No return is sent back.
<!--An echo is returned to all subscribers: `/source/unsolo <string source_id>`
-->
#### Example

BeRTA receives: `/source/unsolo source1`

<!--BeRTA sends back to all subscribers: `/source/unsolo source1`
-->
<!----------------------------------------------------------------------------------->
---


## `/source/loop`

Sets loop mode for the identified source. If loop mode is enabled, when reaching the end, the source will start again. If loop mode is disabled, when reaching the end, the source will stop.

#### Syntax

`/source/loop <string source_id> <boolean enable>`

`source_id`: identifier assigned to the sound source for further references to it

`enable`: if true (1) loop mode is enabled. If false (0), loop mode is disabled

#### Return

No return is sent back.
<!--An echo is returned to all subscribers: `/source/loop <string source_id> <boolean enable>`
-->
#### Example

BeRTA receives: `/source/loop source1 false`

<!--BeRTA sends back to all subscribers: `/source/loop source1 false`
-->
<!----------------------------------------------------------------------------------->
---



## `/source/location`

Set location of source. Position is set in global x,y,z coordinates, expressed in meters. The new location is sent to all remotes except the one has sent the message.

#### Syntax

`/source/location <string source_id> <float x> <float y> <float z>`

`source_id`: identifier assigned to the sound source for further references to it

`x`: X global coordinate, expressed in metres. As a reference, X axis is positive to the front.

`y`: Y global coordinate, expressed in metres. As a reference, Y axis is positive to the left.

`z`: Z global coordinate, expressed in metres. As a reference, Z axis is positive to up.

#### Return

An echo is returned to all subscribers excepting the sender: `/source/location <string source_id> <float x> <float y> <float z>`

#### Example

BeRTA receives: `/source/location source1 2 0 0`

BeRTA sends back to all subscribers excepting the sender: `/source/location source1 2 0 0`

<!----------------------------------------------------------------------------------->
---


## `/source/orientation`

Set orientation of the source. Orientation is set in egocentric coordinates yaw, pitch and roll, expressed in radians and applied in that order (Yaw, Pitch and Roll). An orinetation of (0, 0, 0) corresponds to be oriented towards positive X. The new orientation is sent to all remotes except the one has sent the message.

#### Syntax

`/source/orientation <string source_id> <float yaw> <float pitch> <float roll>`

`source_id`: identifier assigned to the sound source for further references to it

`yaw`: Yaw, expressed in radians. Positive to the right.

`pitch`: Pitch, expressed in radians. Positive upwards.

`roll`: Roll, expressed in radians. Positive to the right.

#### Return

An echo is returned to all subscribers excepting the sender: `/source/orientation <string source_id> <float yaw> <float pitch> <float roll>`

#### Example

BeRTA receives: `/source/orientation source1 0 0 0`

BeRTA sends back to all subscribers excepting the sender: `/source/orientation source1 0 0 0`

<!----------------------------------------------------------------------------------->
---


## `/source/gain`

Sets source gain in dB. 0 dB indicates that the source is kept as it is read from the file or the input channel.

#### Syntax

`/source/gain <string source_id> <float gain>`

`source_id`: identifier assigned to the sound source for further references to it. If the source doesn't exist, the command is ignored.

`gain`: Gain, expressed in dB. the source, as red from the file or the input channel is multiplied by 10^(gain/20)

#### Return

An echo is returned to all subscribers: `/source/gain <string source_id> <float gain>`

#### Example

BeRTA receives: `/source/gain source1 -3`

BeRTA sends back to all subscribers: `/source/gain source1 -3`

<!----------------------------------------------------------------------------------->
---


## `/source/enableDirectivity`

Switch on or off the directivity of the source. For directivity to work, a DirectivityTF must first be loaded from a SOFA file (`/resources/loadDirectivityTF`) and then assigned to the source (`/source/setDirectivity`).

#### Syntax

`/source/enableDirectivity <string source_id> <boolean enable>`

`source_id`: identifier assigned to the sound source. if this the source doesn't exist, the command is ignored

`enable`: if true (1) directivity is enabled. If false (0), directivity is disabled

#### Return

An echo is returned to all subscribers: `/source/enableDirectivity <string source_id> <boolean enable>`

#### Example

BeRTA receives: `/source/enableDirectivity source1 true`

BeRTA sends back to all subscribers: `/source/enableDirectivity source1 true`

<!----------------------------------------------------------------------------------->
---


## `/source/setDirectivity`

Assigns a directivity which has been previously loaded using the command `/resources/loadDirectivityTF`. 

#### Syntax

`/source/setDirectivity <string source_id> <string DirectivityTF_id>`

`source_id`: identifier assigned to the sound source for further references to it

`DirectivityTF_id`: identifier of the directivity which was assigned when using the command `/resources/loadDirectivityTF`

#### Return

In case of success, an echo is returned to all subscribers: `/source/setDirectivity <string source_id> <string DirectivityTF_id>`

#### Example

BeRTA receives: `/source/setDirectivityTF source1 DirectivityTF1`

BeRTA sends back to all subscribers: `/source/setDirectivityTF source1 DirectivityTF1`

<!----------------------------------------------------------------------------------->
---


## `/source/playAndRecord`

Records a file of the specified duration with the delivered binaural sound and all the spatial information of one source and listener (location and orientation). The sound corresponds to the playback of this source from the beginning. The  file includes the structure needed to create a SOFA file with the new [AnnotatedReceiverAudio](https://www.sofaconventions.org/mediawiki/index.php/AnnotatedReceiverAudio) convention


#### Syntax

`/source/playAndRecord <string source_id> <string filename> <string type> <float seconds>`

`source_id`: identifier of the source to be played back and recorded.

`string filename`: indicates the name of the file and must include the path, either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable. The extension will be added by the application if necessary. If there is a file with the same name, it’ll not be overwritten, an ordinal number will be added to the end of the new file name.

`type`: indicates the extension of the file:  wav (not implemented yet) or a mat (matlab binary data container).

`seconds`: indicates the duration of the recording. If seconds is -1 the recording will finish automatically when the source stops.

#### Return

BeRTA will send back to all subscribers the applied stop and play commands for all sources

Once the recording is completed, `/source/playAndRecord <string filename> true` is sent back to the sender. In case the recording is not possible, `/source/playAndRecord <string filename> false` is sent back to the sender.

#### Example

BeRTA receives: `/source/playAndRecord SoundSource1 C:/tmp/testingRecord mat 3`

BeRTA immediately sends back to all subscribers: `/source/stop SoundSource1`, `/source/stop SoundSource2` and `/source/play SoundSource1`, 

After 3 seconds, BeRTA sends back to the sender: `/source/playAndRecord c:/tmp/file.mat true`

<!----------------------------------------------------------------------------------->
---

