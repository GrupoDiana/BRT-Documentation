The OSC control commands allow to establish and manage connections with BeRTA over the network or locally. Below are detailed the basic commands for initiating and closing connections, as well as obtaining system and audio configuration information. Each command is designed to provide immediate feedback.

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/connect**

Open an OSC connection to the indicated IP and port to send the information back. After opening the communicating the following response is sent back: same command with the IP and listening port of BeRTA. Once the connection is stablished, the sender is considered as a suscriber to all the updates which berta sends back.

#### Syntax

`/control/connect <string ip> <int port>`

`ip`: IP address of the sender which BeRTA will use to send back messages. Use `localhost` if it is running in the same machine

`port`: OSC port in which the sender islistening for reply and update messages. BeRTA will send all replies and update messages to this port. 

#### Return

A message is sent back to the sender indicating IP and listening port of BeRTA: `/control/connect <string ip> <int port>`. Although this information is already known by the sender, this is away of acknowdeging that the connection is stablished and the sender is subscribed to updates.


#### Example

BeRTA receives: `/control/connect localhost 10011`.

BeRTA sends back to the sender: `/control/connect localhost 10017`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/disconnect**

Stops the communication through which BeRTA sent information back. This is equivalent to unsibscribe the sender from updates sent back by BrRTA. When this message is received the same message is sent back.

#### Syntax

`/control/disconnect`

#### Return

An echo is sent back to the sender to confirm the reception of the command: `/control/disconnect`.

#### Example

BeRTA receives: `/control/disconnect`.

BeRTA sends back to the sender: `/control/disconnect`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/ping**

Responds with the same message to indicate that BeRTA is listening to the external app. 

#### Syntax

`/control/ping`

#### Return

An echo is sent back to the sender to confirm the reception of the command: `/control/ping`.

#### Example

BeRTA receives: `/control/ping`.

BeRTA sends back to the sender: `/control/ping`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/version**

Responds with a message to indicate the version of BeRTA. 

#### Syntax

`/control/version`

#### Return

An echo is sent back to the sender including the version of BeRTA: `/control/version <string version>`.

#### Example

BeRTA receives: `/control/version`.

BeRTA sends back to the sender: `/control/version BeRTA-Renderer v3.0.0`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/sampleRate**

Asks for the sample rate which BeRTA is using. This parameter is defined in the used [settings file](/BRT-Documentation/setup/settingsFile).

#### Syntax

`/control/sampleRate`

#### Return

An echo is sent back to the sender including the sample rate: `/control/sampleRate <int sample_rate>`.

`sample_rate`: Sample rate used by BeRTA in samples per second.

#### Example

BeRTA receives: `/control/sampleRate`

BeRTA sends back to the sender: `/control/sampleRate 48000`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/frameSize**

Asks for the size of the audio frame which BeRTA is using. This parameter is defined in the used [settings file](/BRT-Documentation/setup/settingsFile).

#### Syntax

`/control/frameSize`

#### Return

An echo is sent back to the sender including the frame size: `/control/frameSize <int frame_size>`.

`frame_size`: size of the audio frame in bytes per channel (BeRTA produces a stereo output for each listener. This parameter indicated the size of the frame of only one channel).

#### Example

BeRTA receives: `/control/frameSize`.

BeRTA sends back to the sender: `/control/frameSize 512`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/bufferFrames**
Asks for the number of audio frames that are stored in the buffer to prevent audio dropout or system failure due to frame underrun. Since the operating system is not real-time, there can be instances where it prioritizes other tasks, potentially delaying the provision of a new audio frame. By setting an adequate buffer size, the system ensures continuous audio playback, even if the operating system momentarily allocates resources to higher-priority processes. This buffer acts as a safeguard against timing inconsistencies, allowing the audio processing thread to retrieve preloaded frames, maintaining uninterrupted audio output. However, as a trade-off, increasing the number of buffered frames results in higher system latency. To minimize latency, this parameter should be set to 0, which eliminates buffering but requires the system to deliver new audio frames in real time without delay. This parameter is defined in the used [settings file](/BRT-Documentation/setup/settingsFile).

#### Syntax

`/control/bufferFrames` 

#### Return

An echo is sent back to the sender including the number of frames buffered: `/control/bufferFrames <int buffer_frames>`.

`buffer_frames`: number of buffered frames.

#### Example

BeRTA receives: `/control/bufferFrames`.

BeRTA sends back to the sender: `/control/bufferFrames 2`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/actionResult**

Command received when an action has been carried out from the application (triggered by a previously received OSC command). It informs about the outcome of the executed action.

#### Syntax

`/control/actionResult <string actionCommand> <string id> <bool success> <string description>`

#### Example

BeRTA receives: `/control/actionResult /resources/removeHRTF HRTF1 true HRTF HRTF1 removed`

<hr style="border:1px solid gray">
<!----------------------------------------------------------------------------------->

## **Calibration**

### **/control/playCalibration**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Start the calibration process by playing an audio file at the dBFS volume specified in the parameter. For more details, refer to the [calibration](/BRT-Documentation/applications/calibration/) section. 


#### Syntax

`/control/playCalibration <float leveldBFS>`

`leveldBFS`: The signal level at which the calibration audio was played, measured in dBFS.

#### Return 

`/control/actionResult /control/playCalibration <string actionName> <boolean success> <string description>`. 

The return confirmation refers to the action `calibration`, indicating `success=true` if the calibration has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details. 

#### Example

BeRTA receives: `/control/playCalibration -40`

BeRTA sends back to the sender: `/control/actionResult /control/playCalibration calibration true "Calibration sound played"`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/setCalibration**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Set the calibration values using the dBFS playback volume and the dBSPL output level measured in the headphones, both specified in the parameters. For more details, refer to the [calibration](/BRT-Documentation/applications/calibration/) section.

#### Syntax

`/control/setCalibration <float leveldBFS> <float leveldBSPL>`

`leveldBFS`: The signal level at which the calibration audio was played, measured in dBFS.

`leveldBSPL`: The output sound level measured at the headphones (expressed in dBSPL) when the system is calibrated and delivering the dBFS specified in the leveldBFS parameter.

#### Return 

`/control/actionResult /control/setCalibration <string actionName> <boolean success> <string description>`. 

The return confirmation refers to the action `calibrationDone`, indicating `success=true` if the calibration has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details. 

#### Example

BeRTA receives: `/control/setCalibration -40 60`

BeRTA sends back to the sender: `/control/actionResult /control/setCalibration calibrationDone true "Calibration done with -40 dB FS and 60 dB SPL."`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/playCalibrationTest**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Play the calibration test sound at a volume adjusted to match the dBSPL specified in the parameter, allowing verification with the sound level meter. See the [calibration](/BRT-Documentation/applications/calibration/) section for more details.

#### Syntax

`/control/playCalibrationTest  <float leveldBSPL>`

`leveldBSPL`: The signal level (in dBSPL) measured at the headphones when reproducing the calibration audio.

#### Return 

`/control/actionResult /control/playCalibrationTest <string actionName> <boolean success> <string description>`. 

The return confirmation refers to the action `calibrationTest`, indicating `success=true` if the calibration has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details. 

#### Example

BeRTA receives: `/control/playCalibrationTest 80`

BeRTA sends back to the sender: `/control/actionResult /control/playCalibrationTest calibrationTest false "ERROR BeRTA has not been calibrated yet"`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/stopCalibrationTest**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Stop the calibration test sound playback.

#### Syntax

`/control/stopCalibrationTest`

#### Return 

`/control/actionResult /control/stopCalibrationTest <string actionName> <boolean success> <string description>`. 

The return confirmation refers to the action `calibrationTest`, indicating `success=true` if the stop of the calibration has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details. 

#### Example

BeRTA receives: `/control/stopCalibrationTest`

BeRTA sends back to the sender: `/control/actionResult /control/stopCalibrationTest calibrationTest true "Calibration test stopped."`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/getSoundLevel**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Get the current sound level in dBSPL for left and right channel. For more details, refer to the [calibration](/BRT-Documentation/applications/calibration/) section.

#### Syntax

`/control/getSoundLevel <string listener_id>`

`listener_id`: identifier assigned to the listener.

#### Return

`/resources/getSoundLevel <string listener_id> <float leftChannelSoundLevel> <float rightChannelSoundLevel>`

The return confirmation refers to the `listener_id`, indicating the sound level for the left (`leftChannelSoundLevel`) and the right channel (`rightChannelSoundLevel`). 

#### Example

BeRTA receives: `/control/getSoundLevel`

BeRTA sends: `/control/getSoundLevel 60 62`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

## **Safety Limiter**

### **/control/setSoundLevelLimit**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Set the sound level limit to the dBSPL value specified in the parameter. See the [safety limitter](/BRT-Documentation/applications/safety-limiter/) for more details.

#### Syntax

`/control/setSoundLevelLimit <float levelLimitdBSPL>`

`levelLimitdBSPL`: The maximum allowable sound level, in dBSPL, used by the limiter to ensure safe listening levels for the user.

#### Return 

`/control/actionResult /control/setSoundLevelLimit <string actionName> <boolean success> <string description>`. 

The return confirmation refers to the action `calibrationTest`, indicating `success=true` if the set has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details. 

#### Example

BeRTA receives: `/control/setSoundLevelLimit 100`

BeRTA sends back to the sender: `/control/actionResult /control/setSoundLevelLimit calibrationTest false "ERROR Sound level limit has not been set becasuse BeRTA has not been calibrated."`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/soundLevelAlert**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Sent when the output sound level exceeds the threshold set in the sound level limiter. This is a send-only command from BeRTA. It does not receive any parameters but triggers an alert when the limit is surpassed. See the [safety limitter](/BRT-Documentation/applications/safety-limiter/) for more details.

#### Syntax

`/control/soundLevelAlert`

#### Example

BeRTA sends: `/control/soundLevelAlert 60 62`
