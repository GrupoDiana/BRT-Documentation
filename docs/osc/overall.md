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

`type`: indicates the extension of the file: **mat** (MATLAB binary data container) or **wav** (32-bit IEEE floating-point stereo).

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

### **/record**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.7.0</span>

Generates a file of the specified duration with spatialised sound (wav) and other data (mat) by processing all sources from the beginning *without real-time playback*. This allows the operation to be executed significantly faster. If the type is mat, the produced file will follow the structure of the [AnnotatedReceiverAudio SOFA convention](https://www.sofaconventions.org/mediawiki/index.php/AnnotatedReceiverAudio). All sources are stopped and processed from the beginning before the recording starts.

#### Syntax
`/record <String filename> <String type> <float time>`

`filename`: indicates the name of the file and must include the path, either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable. The extension will be added by the application if necessary. If there is a file with the same name, it’ll not be overwritten—an ordinal number will be added to the end of the new file name.

`type`: indicates the extension of the file: **mat** (MATLAB binary data container) or **wav** (32-bit IEEE floating-point stereo).

`time`: indicates the duration of the recording in seconds. **This value must be greater than 0**. Unlike `/playAndRecord`, the special value `-1` is *not* allowed.

#### Return 
`/control/actionResult /record <string filename> <boolean success> <string description>`.

The return confirmation refers to the `filename`, indicating `success=true` if the action has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details.

It sends an echo to all subscribers except the sender: `/record <String filename> <String type> <float time>`

#### Example
BeRTA receives and echoes back to all subscribers except the sender: `/record c:/tmp/file.mat mat 10`

When the processing is finished (in this example after 10 seconds), BeRTA sends back to all subscribers:  
`/control/actionResult /record c:/tmp/file.mat true Recording completed. File saved : c:/tmp/file.mat`  
or  
`/control/actionResult /record c:/tmp/file.mat false ERROR:Recording failed. File could not be created.`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/enableModel**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.6.0</span>

This command enables or disables a model. This function is implemented in all listener, environment models and in all binaural filters. The model to be enabled or disabled is identified by an identifier defined in the [configuration](../applications/settingsFile.md) file used. When a model is deactivated it does not process the input signal and provides: 

- *Silence* on its output in the case of listener models.
- Let the signal *pass through unaltered* in the case of environment models and binaural filters. 


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

`gain`: A floating value, expressed in dB, representing the gain. The model output is multiplied by $10^{gain / 20}$. 


#### Return
`/control/actionResult /modelGain <string model_id> <bool setGain> <string description>`

The return confirmation refers to the `model_id`, indicating `setGain=true` if the action has been successfully performed and setGain=false if not. In both cases a description is added to give more details.

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example
BeRTA receives and echoes back to all subscribiers but the sender:`/modelGain DirectPath -3`

BeRTA sends back to the sender: `/control/actionResult /modelGain DirectPath true "Listener model DirectPath gain updated to -3dB."`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/recordIR**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.13.0</span>

Generates a file of the specified duration containing the **system’s impulse response**, as configured at that moment. The applied impulse can be adjusted using various parameters. Any sound sources present in the simulation will be removed to carry out the measurement. This processing and recording will take place *without real-time playback*.
If the generated file is of the wav type, only the impulse response is saved. If the type is mat, additional data will be saved; the generated file will follow the structure of the [SOFA AnnotatedReceiverAudio convention](https://www.sofaconventions.org/mediawiki/index.php/AnnotatedReceiverAudio). 

#### Syntax
`/recordIR <String filename> <String type> <float time> <int period> <int delay> <float x> <float y> <float z> <String directivity> <bool enable_directivity>`

`filename`: indicates the name of the file and must include the path, either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable. The extension will be added by the application if necessary. If there is a file with the same name, it’ll not be overwritten—an ordinal number will be added to the end of the new file name.

`type`: indicates the extension of the file: **mat** (MATLAB binary data container) or **wav** (32-bit IEEE floating-point stereo).

`time`: indicates the duration of the recording in seconds. *This value must be greater than 0*. The special value `-1` is *not* allowed.

`period`: indicates the number of samples between pulses. If this value is zero, the response to a single pulse is recorded. However, if you wish to record the response to a *impulse train*, this value must indicate the number of samples between impulses. This value must be less than the duration of the recording.

`delay`: indicates the number of samples by which the impulse is delayed relative to the origin. This value must be less than the duration of the recording.

`x`: global coordinate of the impulse’s position on the X-axis, expressed in metres. As a reference, X axis is positive to the front.

`y`: global coordinate of the impulse’s position on the Y-axis. As a reference, Y axis is positive to the left.

`z`: global coordinate of the impulse’s position on the Z-axis, expressed in metres. As a reference, Z axis is positive to up.

`directivity`: indicates the ID of the directivity to be applied to the impulse source. If the specified ID corresponds to a previously loaded directivity, the recording is made using that directivity. If the specified ID is not valid or does not correspond to a loaded directivity, the recording is made without any directivity assigned. If this parameter is left empty, the impulse source is considered omnidirectional.

`enable_directivity`: indicates whether directivity is enabled for the impulse source. If set to true, the impulse source is created with the specified directivity enabled. If set to false, the impulse source is created with the specified directivity disabled. If this parameter is not specified, it defaults to true. If no valid directivity is specified, this parameter has no effect.

#### Return 
`/control/actionResult /recordIR <string filename> <boolean success> <string description>`.

The return confirmation refers to the `filename`, indicating `success=true` if the action has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details.

It sends an echo to all subscribers except the sender: `/recordIR <String filename> <String type> <float time> <int period> <int delay> <float x> <float y> <float z> <String directivity> <bool enable_directivity>`

#### Example
BeRTA receives and echoes back to all subscribers except the sender: `/recordIR "record/sytem_conf_IR.wav" "wav" 2 0 0 1 0 0 Directivity1 true`

When the processing is finished (in this example after 2 seconds), BeRTA sends back to all subscribers:  
`/control/actionResult /recordIR record/system_conf_IR.wav true Recording completed. File saved: record/sytem_conf_IR.wav`  
or  
`/control/actionResult /recordIR record/sytem_conf_IR.wav false ERROR:Recording failed. File could not be created.`
or  
`/control/actionResult /recordIR Directivity1 false ERROR:setting Directivity to the source. Directivity not found into the directivity list.`
