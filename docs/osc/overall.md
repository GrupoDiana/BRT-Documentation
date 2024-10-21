These commands affects all sound sources and therefore, they don't need the `/source`prefix.

<!----------------------------------------------------------------------------------->
---

## `/play`

Starts playing back all sources comming from audio files and starts streaming all sources comming form input channels.

#### Syntax

`/play`

#### Return

An echo is returned to all subscribers for each of the sources being played: `/source/play <string source_id>`


#### Example

BeRTA receives: `/play`

BeRTA sends back to all subscribers: `/source/play source1`, `/source/play source2`, `/source/play source3`. 


<!----------------------------------------------------------------------------------->
---



## `/stop`

Stops playing back all sources comming from audio files and also stops streaming all sources comming form input channels.

#### Syntax

`/stop`

#### Return

An echo is returned to all subscribers for each of the sources being stopped: `/source/stop <string source_id>`


#### Example

BeRTA receives: `/stop`

BeRTA sends back to all subscribers: `/source/stop source1`, `/source/stop source2`, `/source/stop source3`. 


<!----------------------------------------------------------------------------------->
---



## `/pause`

Pauses all sources comming from audio files and stops streaming all sources comming form input channels. While file sources will resume with a later play at the same point they were paused, for sources connected to an input channel the effect of `/pause` is identical to the effect of `/stop`. 

#### Syntax

`/pause`

#### Return

<!--An echo is returned to all subscribers for each of the sources being paused: `/source/pause <string source_id>`
-->

#### Example

BeRTA receives: `/pause`

<!--BeRTA sends back to all subscribers: `/source/pause source1`, `/source/pause source2`, `/source/pause source3`. 
-->

<!----------------------------------------------------------------------------------->
---



## `/removeAllSources`

Removes all sources. 

#### Syntax

`/removeAllSources`

#### Return

<!--An echo is returned to all subscribers for each of the sources being paused: `/source/remove <string source_id>`
-->

#### Example

BeRTA receives: `/removeAllSources`

<!--BeRTA sends back to all subscribers: `/source/remove source1`, `/source/remove source2`, `/source/remove source3`. 
-->

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

BeRTA sends back to all subscribers messaves of stop and play for every source: `/source/stop <string source_id>`, `/source/play <string source_id>`. 

After the recording is finished, a message is sent back to the sender indicating it: `/playAndRecord <String filename> <boolean success>`. If the recording was successfully completed (either because it reached the given time or because a stop was recevied while recording), `success` will be true in the confirmation message. If the recording cannot be completed for any reason, this response message will set `success` to false. 


#### Example

BeRTA receives: `/playAndRecord c:/tmp/file.mat mat 10`

Berta sends back to all subscribers:  `/source/stop source1`, `/source/stop source2`, `/source/stop source3`, `/source/play source1`, `/source/play source2`, `/source/play source3`.

After 10 seconds, BeRTA sends back to the sender: `/playAndRecord c:/tmp/file.mat true`. 


<!----------------------------------------------------------------------------------->
---



