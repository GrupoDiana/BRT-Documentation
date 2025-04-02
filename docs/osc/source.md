These commands provide a comprehensive set of tools for managing and controlling audio sources within the virtual scene. These commands allow for the addition, manipulation, and control of sound sources, including their playback, position, orientation, and various audio properties.

<hr style="border:1px solid gray">
<!----------------------------------------------------------------------------------->

### **/source/loadSource**

Adds a new source loading a sound file (wav, mp3 and aif formats supported). Adding a new source does not imply playing it back. When adding the source an ID string is associated. That ID can be used later to control the source using other OSC commands.

#### Syntax

`/source/loadSource <string source_id> <string source_path> <string source_model> [<string modelToConnectTo>]`

Specifying a sound file with `source_path`, a new sound file is loaded from the specified path and an identifier `source_id` is assigned to it. If there is already another source with the same identifier, it is substituted by the new one. Otherwise, a new source is created. 

`source_model` indicates the source model to be used, possible values are: `SimpleModel`, `DirectivityModel`. See [Source Models](/BRT-Documentation/library/source/models/) for more details.

`modelToConnectTo` is optional and indicates to which listener model this source will be connected. If left blank, the source will be connected to all listener models that have been indicated in the application configuration.

#### Return

`/control/actionResult /source/loadSource <string source_id> <bool loaded> <string description>` .

The return confirmation refers to the `source_id`, indicating `loaded=true`if the source is successfuly loaded and `loaded=false` if not. In both cases a `description`is added to give more details.

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/loadSource source1 speech.wav`

BeRTA sends back to the sender: `/control/actionResult /source/loadSource source1 true Audio source source1 loop enabled` or `/source/loadSource source1 ERROR: The audio source source1 doesn't exist.`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/addLineIn**

Adds a new source getting it from an audio input channel. Adding a new source does not imply playing it back. When adding the source an ID string is associated. That ID can be used later to control the source using other OSC commands.

#### Syntax

`/source/addLineIn <string source_id> <int linein_channel> <string source_model> [<string modelToConnectTo>]`

`source_id`: identifier assigned to the sound source for further references to it

`linein_channel`: indicates the input channel of the selected audio device to be linked to this sound source. 

`source_model` indicates the source model to be used, possible values are: `SimpleModel`, `DirectivityModel`. See [Source Models](/BRT-Documentation/library/source/models/) for more details.

`modelToConnectTo` is optional and indicates to which listener model this source will be connected. If left blank, the source will be connected to all listener models that have been indicated in the application configuration.

#### Return

`/control/actionResult /source/addLineIn <string source_id> <bool loaded> <string description>`

The return confirmation refers to the `source_id`, indicating `loaded=true`if the source is successfuly loaded and `loaded=false` if not. In both cases a `description`is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/source/addLineIn source1 1 DirectivityModel`

BeRTA sends back to the sender: `/control/actionResult /source/addLineIn source1 true AUDIO LINE-IN source1 configured succesfully` or `/source/addLineIn source1 false error: not a valid channel`
<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/removeSource**

Removes a sound source from the system. 

#### Syntax

`/source/removeSource <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

`/control/actionResult /source/removeSource <string source_id> <bool removed> <string description>`

The return confirmation refers to the `source_id`, indicating `removed=true`if the source is successfuly removed and `removed=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/source/remove source1`

BeRTA sends back to the sender : `/control/actionResult /source/remove source1 true Audio source removed: source1` or `/control/actionResult /source/remove source1 false ERROR: The audio source source1 doesn't exist.`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/play**

Plays a specific source, starting to send frames to the library from either the audio file or the input channel. In the first case, it starts from the beginning of the file unless the source was previously started and a `/source/pause` command was sent before. In such case it resumes at the point it was paused. 

#### Syntax

`/source/play <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

`/control/actionResult /source/play <string source_id> <bool played> <string description>`

The return confirmation refers to the `source_id`, indicating `played=true`if the source is successfuly played and `played=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/play source1`

BeRTA sends back to the sender: `/control/actionResult /source/play source1 true Audio source source1 played back` 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/stop**

Stops a specific source. In the case of a source comming from a file, this resets the source so that the next `/source/play` command will start at the beginning of the file. 

#### Syntax

`/source/stop <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

`/control/actionResult /source/stop <string source_id> <bool stopped> <string description>`

The return confirmation refers to the `source_id`, indicating `stopped=true` if the action has been successfully performed and `stopped=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender:  `/source/stop source1`

BeRTA sends back to the sender: `/control/actionResult /source/stop source1 true Audio source source1 removed.`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/pause**

Pauses a specific source. In the case of a source comming from a file, the next `/source/play` command will resume, sarting at the same point where it was paused. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/pause <string source_id>`

`sourc e_id`: identifier assigned to the sound source for further references to it

#### Return

`/control/actionResult /source/pause <string source_id> <bool paused> <string description>`

The return confirmation refers to the `source_id`,  indicating `paused=true` if the action has been successfully performed and `paused=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/pause source1`

BeRTA sends back to the sender: `/control/actionResult /source/pause source1 true Audio source source1 paused.`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/mute**

Mutes a specific source. In the case of a source comming from a file, the application keeps getting the sound stream from the file at the same rate, but silently. The `/source/unmute` command will make the source sound again. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/mute <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

`/control/actionResult /source/mute <string source_id> <bool muted> <string description>`

The return confirmation refers to the `source_id`, indicating `muted=true` if the action has been successfully performed and `muted=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/mute source1`

BeRTA sends back to the sender: `/control/actionResult /source/mute source1 true Audio source source1 muted.`
<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/unmute**

Unmutes a specific source. In the case of a source comming from a file previously muted, the application has kept getting the sound stream from the file at the same rate, but silently. The `/source/unmute` command will make the source sound again. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/unmute <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

`/control/actionResult /source/unmute <string source_id> <bool unmuted> <string description>`

The return confirmation refers to the `source_id`, indicating `unmuted=true` if the action has been successfully performed and `unmuted=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/unmute source1`

BeRTA sends back to the sender: `/control/actionResult /source/unmute source1 true Audio source source1 unmuted.`
<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/solo**

Mutes all sources but the the one indicated by this command. In the case of sources comming from files, the application keeps getting the sound stream from them at the same rate, but silently. The `/source/unsolo` command will make all the sources sound again. Individual commands `/source/unmute` addressed to those sources muted as a consecuence of an `/source/solo`command will have the same effect making them sound again. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/solo <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

`/control/actionResult /source/solo <string source_id> <bool solo> <string description>`

The return confirmation refers to the `source_id`, indicating `solo=true` if the action has been successfully performed and `solo=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/solo source1`

BeRTA sends back to the sender:  `/control/actionResult /source/solo source1 true Audio source source1 playing solo.`
<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/unsolo**

Unmutes all sources but the the one indicated by this command. If the `source_id` does not exist, the command is ignored.

#### Syntax

`/source/unsolo <string source_id>`

`source_id`: identifier assigned to the sound source for further references to it

#### Return

`/control/actionResult /source/unsolo <string source_id> <bool unsolo> <string description>`

The return confirmation refers to the `source_id`, indicating `unsolo=true` if the action has been successfully performed and `unsolo=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/unsolo source1`

BeRTA sends back to the sender:  `/control/actionResult /source/unsolo source1 true All audio sources unmuted.`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/loop**

Sets loop mode for the identified source. If loop mode is enabled, when reaching the end, the source will start again. If loop mode is disabled, when reaching the end, the source will stop.

#### Syntax

`/source/loop <string source_id> <boolean enable>`

`source_id`: identifier assigned to the sound source for further references to it

`enable`: if true (1) loop mode is enabled. If false (0), loop mode is disabled

#### Return

`/control/actionResult /source/loop <string source_id> <bool loop> <string description>`

The return confirmation refers to the `source_id`, indicating `loop=true` if the action has been successfully performed and `loop=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/loop source1`

BeRTA sends back to the sender: `/control/actionResult /source/loop source1 true Audio source source1 enabled.`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/location**

Set location of source. Position is set in global x,y,z coordinates, expressed in meters. The new location is sent to all remotes except the one has sent the message.

#### Syntax

`/source/location <string source_id> <float x> <float y> <float z>`

`source_id`: identifier assigned to the sound source for further references to it

`x`: X global coordinate, expressed in metres. As a reference, X axis is positive to the front.

`y`: Y global coordinate, expressed in metres. As a reference, Y axis is positive to the left.

`z`: Z global coordinate, expressed in metres. As a reference, Z axis is positive to up.

#### Return

Any confirmation message is sent back to the sender.

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/location source1 2 0 0`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/orientation**

Set orientation of the source. Orientation is set in egocentric coordinates yaw, pitch and roll, expressed in radians and applied in that order (Yaw, Pitch and Roll). An orinetation of (0, 0, 0) corresponds to be oriented towards positive X. The new orientation is sent to all remotes except the one has sent the message.

#### Syntax

`/source/orientation <string source_id> <float yaw> <float pitch> <float roll>`

`source_id`: identifier assigned to the sound source for further references to it

`yaw`: Yaw, expressed in radians. Positive to the right.

`pitch`: Pitch, expressed in radians. Positive upwards.

`roll`: Roll, expressed in radians. Positive to the right.

#### Return

Any confirmation message is sent back to the sender.

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/orientation source1 0 0 0`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/gain**

Sets source gain in dB. 0 dB indicates that the source is kept as it is read from the file or the input channel.

#### Syntax

`/source/gain <string source_id> <float gain>`

`source_id`: identifier assigned to the sound source for further references to it. If the source doesn't exist, the command is ignored.

`gain`: Gain, expressed in dB. the source, as red from the file or the input channel is multiplied by 10^(gain/20)

#### Return

`/control/actionResult /source/gain <string source_id> <bool setGain> <string description>`

The return confirmation refers to the `source_id`, indicating `setGain=true` if the action has been successfully performed and `setGain=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/gain source1 -3`

BeRTA sends back to the sender: `/control/actionResult /source/gain source1 true Audio source source1 gain updated to -3 dB`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/enableDirectivity**

Switch on or off the directivity of the source. For directivity to work, a DirectivityTF must first be loaded from a SOFA file (`/resources/loadDirectivityTF`) and then assigned to the source (`/source/setDirectivity`).

#### Syntax

`/source/enableDirectivity <string source_id> <boolean enable>`

`source_id`: identifier assigned to the sound source. if this the source doesn't exist, the command is ignored

`enable`: if true (1) directivity is enabled. If false (0), directivity is disabled

#### Return

`/control/actionResult /source/enableDirectivity <string source_id> <bool enableDirectivity> <string description>`
 
The return confirmation refers to the `source_id`,  indicating `enableDirectivity=true` if the action has been successfully performed and `enableDirectivity=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/enableDirectivity source1 true`

BeRTA sends back to the sender: `/control/actionResult /source/enableDirectivity source1 true Audio source source1 directivity enabled`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/setDirectivity**

Assigns a directivity which has been previously loaded using the command `/resources/loadDirectivityTF`. 

#### Syntax

`/source/setDirectivity <string source_id> <string DirectivityTF_id>`

`source_id`: identifier assigned to the sound source for further references to it

`DirectivityTF_id`: identifier of the directivity which was assigned when using the command `/resources/loadDirectivityTF`

#### Return

`/control/actionResult /source/setDirectivity <string DirectivityTF_id> <bool setDirectivity> <string description>`

The return confirmation refers to the `source_id`, indicating `setDirectivity=true` if the action has been successfully performed and `setDirectivity=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/setDirectivityTF source1 DirectivityTF1`

BeRTA sends back to the sender: `/control/actionResult /source/setDirectivityTF DirectivityTF1 true DirectivityTF: DirectivityTF1 has been set to SoundSource: source1`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/source/playAndRecord**

Records a file of the specified duration with the delivered binaural sound and all the spatial information of one source and listener (location and orientation). The sound corresponds to the playback of this source from the beginning. The  file includes the structure needed to create a SOFA file with the new [AnnotatedReceiverAudio](https://www.sofaconventions.org/mediawiki/index.php/AnnotatedReceiverAudio) convention


#### Syntax

`/source/playAndRecord <string source_id> <string filename> <string type> <float seconds>`

`source_id`: identifier of the source to be played back and recorded.

`string filename`: indicates the name of the file and must include the path, either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable. The extension will be added by the application if necessary. If there is a file with the same name, it’ll not be overwritten, an ordinal number will be added to the end of the new file name.

`type`: indicates the extension of the file:  wav (not implemented yet) or a mat (matlab binary data container).

`seconds`: indicates the duration of the recording. If seconds is -1 the recording will finish automatically when the source stops.

#### Return

`/control/actionResult /source/playAndRecord <string source_id> <bool recorded> <string description>`
 
The return confirmation refers to the `source_id`, indicating `recorded=true` if the action has been successfully performed and `recorded=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/source/playAndRecord SoundSource1 C:/tmp/testingRecord mat 3`

BeRTA sends back to the sender: `/control/actionResult /source/playAndRecord source1 true "Recording completed. File saved: C:/tmp/testingRecord`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

