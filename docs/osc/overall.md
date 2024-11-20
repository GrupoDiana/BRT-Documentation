These commands affects all sound sources and therefore, they don't need the `/source`prefix.

<!----------------------------------------------------------------------------------->
---

## `/play`

Starts playing back all sources comming from audio files and starts streaming all sources comming form input channels.

#### Syntax

`/play`

#### Return

An echo is returned to all subscribers but the sender: `/play`.


#### Example

BeRTA receives: `/play`.

BeRTA sends back to all subscribers but the sender: `/play`. 


<!----------------------------------------------------------------------------------->
---



## `/stop`

Stops playing back all sources comming from audio files and also stops streaming all sources comming form input channels.

#### Syntax

`/stop`

#### Return

An echo is returned to all subscribers but the sender: `/stop`.


#### Example

BeRTA receives: `/stop`.

BeRTA sends back to all subscribers but the sender: `/stop`. 


<!----------------------------------------------------------------------------------->
---



## `/pause`

Pauses all sources comming from audio files and stops streaming all sources comming form input channels. While file sources will resume with a later play at the same point they were paused, for sources connected to an input channel the effect of `/pause` is identical to the effect of `/stop`. 

#### Syntax

`/pause`

#### Return

An echo is returned to all subscribers but the sender: `/pause`.

#### Example

BeRTA receives: `/pause`.

BeRTA sends back to all subscribers but the sender: `/pause`. 

<!----------------------------------------------------------------------------------->
---



## `/removeAllSources`

Removes all sources. 

#### Syntax

`/removeAllSources`

#### Return

An echo is returned to all subscribers but the sender: `/removeAllSources`.

#### Example

BeRTA receives: `/removeAllSources`.

BeRTA sends back to all subscribers but the sender: `/removeAllSources`. 

<!----------------------------------------------------------------------------------->
---

## `/playAndRecord`

Records a file of the specified duration with spatialised sound (wav) and other data (mat) corresponding to the playback of all sources from the beginning. If the type is mat, the produced file will follow the structure of the [AnnotatedReceiverAudio SOFA convention](https://www.sofaconventions.org/mediawiki/index.php/AnnotatedReceiverAudio). Before starting the recording, all sources are stopped and played back since the begining. 

#### Syntax

`/playAndRecord <String filename> <String type> <float time>`

`filename`: indicates the name of the file and must include the path, either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable. The extension will be added by the application if necessary. If there is a file with the same name, it’ll not be overwritten, an ordinal number will be added to the end of the new file name.

`type`: indicates the extension of the file: wav (not implemented yet)  or a mat (matlab binary data container).

`time`: indicates the duration of the recording in seconds. If time is -1 the recording will finish automatically when the source stops.


#### Return 

A return message is sent back to the sender `control/actionResult /playAndRecord <string filename> <boolean success> <string description>`. If the recording was successfully completed (either because it reached the given time or because a stop was recevied while recording), `success` will be true in the confirmation message. If the recording cannot be completed for any reason, this response message will set `success` to false.

It sends an echo to all subscribers excepting the sender: `/playAndRecord <String filename> <String type> <float time>`

#### Example

BeRTA receives: `/playAndRecord c:/tmp/file.mat mat 10`

When the recording is finished (in this example after 10 seconds), BeRTA sends back to all subscribers: `control/actionResult /playAndRecord c:/tmp/file.mat true Recording completed. File saved : c:/tmp/file.mat` or `control/actionResult  /playAndRecord c:/tmp/file.mat false ERROR:Recording failed. File could not be created.`

BeRTA sends to all subscribiers but the sender: `/playAndRecord c:/tmp/file.mat mat 10`


<!----------------------------------------------------------------------------------->
---



