These commands affects all sound sources and therefore, they don't need the `/source`prefix.

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/play**
Starts playing back all sources comming from audio files and starts streaming all sources comming form input channels.

#### Syntax
`/play`

#### Return
In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example
BeRTA receives and echoes back to all subscribiers but the sender:  `/play`.

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/stop**
Stops playing back all sources comming from audio files and also stops streaming all sources comming form input channels.

#### Syntax
`/stop`

#### Return
In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example
BeRTA receives and echoes back to all subscribiers but the sender: `/stop`.


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/pause**
Pauses all sources comming from audio files and stops streaming all sources comming form input channels. While file sources will resume with a later play at the same point they were paused, for sources connected to an input channel the effect of `/pause` is identical to the effect of `/stop`. 

#### Syntax
`/pause`

#### Return
In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example
BeRTA receives and echoes back to all subscribiers but the sender: `/pause`.

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/removeAllSources**
Removes all sources. 

#### Syntax
`/removeAllSources`

#### Return
In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example
BeRTA receives and echoes back to all subscribiers but the sender:`/removeAllSources`.

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/playAndRecord**
Records a file of the specified duration with spatialised sound (wav) and other data (mat) corresponding to the playback of all sources from the beginning. If the type is mat, the produced file will follow the structure of the [AnnotatedReceiverAudio SOFA convention](https://www.sofaconventions.org/mediawiki/index.php/AnnotatedReceiverAudio). Before starting the recording, all sources are stopped and played back since the begining. 

#### Syntax
`/playAndRecord <String filename> <String type> <float time>`

`filename`: indicates the name of the file and must include the path, either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable. The extension will be added by the application if necessary. If there is a file with the same name, it’ll not be overwritten, an ordinal number will be added to the end of the new file name.

`type`: indicates the extension of the file: wav (not implemented yet)  or a mat (matlab binary data container).

`time`: indicates the duration of the recording in seconds. If time is -1 the recording will finish automatically when the source stops.

#### Return 
`/control/actionResult /playAndRecord <string filename> <boolean success> <string description>`. 

The return confirmation refers to the `filename`, indicating `success=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

It sends an echo to all subscribers excepting the sender: `/playAndRecord <String filename> <String type> <float time>`

#### Example
BeRTA receives and echoes back to all subscribiers but the sender: `/playAndRecord c:/tmp/file.mat mat 10`

When the recording is finished (in this example after 10 seconds), BeRTA sends back to all subscribers: `/control/actionResult /playAndRecord c:/tmp/file.mat true Recording completed. File saved : c:/tmp/file.mat` or `/control/actionResult  /playAndRecord c:/tmp/file.mat false ERROR:Recording failed. File could not be created.`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/enableModel**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.6.0</span>

This command switches on or off a model. When a model is disabled it does not process the input signal and provides ~~silence~~ at its output. This feature must be implemented in all listener, enviroment models and in all the Binaural Filters. The model to be enabled or disabled is identified by an identifier defined in the used [settings file](../applications/settingsFile.md).

#### Syntax
`/enableModel <string model_id> <boolean enable>`

`model_id`: identifier assigned to the model.

`enable`: If true (1), enables the model. If false (0), the model is disabled and its output will be silent.

#### Return
`/control/actionResult /enableModel <string model_id> <bool enable> <string description>`

The return confirmation refers to the `model_id`, indicating `enable=true` if the model has been enabled and `enable=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example
BeRTA receives and echoes back to all subscribiers but the sender:`/enableModel DirectPath true`

BeRTA sends back to the sender: `/control/actionResult /enableModel DirectPath true "Listener model DirectPath enabled."`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/modelGain**

<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.6.0</span>

This command sets the output gain of the model in dB. This is a gain that is applied to the model output after processing. A gain of 0 dB, default value, indicates that no gain is applied to the output samples after processing. The model to be enabled or disabled is identified by an identifier defined in the used [settings file](../applications/settingsFile.md).

#### Syntax
`/modelGain <string model_id> <float gain>`

`model_id`: identifier assigned to the model.

`gain`: Gain, expressed in dB. the source, as red from the file or the input channel is multiplied by $10^{gain / 20}$. 


#### Return
`/control/actionResult /modelGain <string model_id> <bool setGain> <string description>`

The return confirmation refers to the `model_id`, indicating `setGain=true` if the action has been successfully performed and setGain=false if not. In both cases a description is added to give more details.

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example
BeRTA receives and echoes back to all subscribiers but the sender:`/modelGain DirectPath -3`

BeRTA sends back to the sender: `/control/actionResult /modelGain DirectPath true "Listener model DirectPath gain updated to -3dB."`