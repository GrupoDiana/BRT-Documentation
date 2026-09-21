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


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

## **Listener output configuration**

### **/control/setListenerOutput**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.15.0</span>

Configure an existing listener's output assignment, Active/Standby state and calibration reference. 

This command provides the output-routing configuration available in the Listeners tab of the Settings GUI. It does not create a listener. The listener type is determined by its output assignment: a non-negative channel selects a physical output pair, while `-1` makes the listener virtual.

#### Syntax

`/control/setListenerOutput <string listener_id> <int leftOutputChannel> <bool outputActive> <string calibrationReferenceListenerID>`

All four arguments are required, including an empty string when no calibration reference is used.

`listener_id`: Identifier of the existing listener to configure.

`leftOutputChannel`: Zero-based left channel of an available physical stereo output pair. It must be a non-negative even number, such as `0` for channels 0–1 or `2` for channels 2–3. Use `-1` for a virtual listener.

`outputActive`: For a listener with a physical output assignment, `true` selects Active and `false` selects Standby. Virtual listeners must use `false`. This controls physical output routing, not whether the listener's signal levels are calculated. Send this argument as an OSC boolean.

`calibrationReferenceListenerID`: For a virtual listener, the ID of another listener with a direct output assignment, or `""` for no reference. A listener with a physical output assignment must use `""`.

#### Behaviour

- Activating a listener automatically puts other listeners assigned to the same output pair into Standby.
- Listeners assigned to different output pairs may be active simultaneously.
- A virtual listener may reference a routed listener in either Active or Standby.
- A virtual listener cannot reference itself or another virtual listener.
- Calibration references follow the referenced listener's current output assignment.
- Converting a routed listener into a virtual listener automatically clears references from other listeners that depended on it.
- An output pair does not need to be calibrated to configure its routing. Calibration is required to obtain a listener's theoretical dBSPL level.
- The resulting routing configuration is validated before it is applied. If validation fails, the current routing configuration is preserved.

#### Return

`/control/actionResult /control/setListenerOutput <string listener_id> <bool success> <string description>`

`listener_id`: Listener being configured. If the request cannot provide a usable listener ID, the response uses `listenerOutput`.

`success`: `true` if the configuration was applied; otherwise, `false`.

`description`: Description of the applied configuration or the reason for failure.

#### Examples

- Assign Listener A to channels 0–1 and activate its output:

    BeRTA receives: `/control/setListenerOutput A 0 true ""`
    
    BeRTA sends: `/control/actionResult /control/setListenerOutput A true "Listener A routed to output channels 0-1 (Active)."`

- Keep the assignment but put Listener A into Standby:

    BeRTA receives: `/control/setListenerOutput A 0 false ""`
    
    BeRTA sends: `/control/actionResult /control/setListenerOutput A true "Listener A routed to output channels 0-1 (Standby)."`

- Configure Listener B as virtual, using Listener A as its calibration reference:

    BeRTA receives: `/control/setListenerOutput B -1 false A`

    BeRTA sends: `/control/actionResult /control/setListenerOutput B true "Listener B configured as Virtual with A as calibration reference."`

- Remove Listener B's calibration reference while keeping it virtual:

    BeRTA receives: `/control/setListenerOutput B -1 false ""`

    BeRTA sends: `/control/actionResult /control/setListenerOutput B true "Listener B configured as Virtual without a calibration reference."`

- Attempt to activate a virtual listener:

    BeRTA receives: `/control/setListenerOutput B -1 true A`

    BeRTA sends: `/control/actionResult /control/setListenerOutput B false "ERROR: Virtual listener B cannot have an active physical output."`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

## **Calibration**

### **/control/playCalibration**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Start the calibration process by playing an audio file at the dBFS volume specified in the parameter. For more details, refer to the [calibration](/BRT-Documentation/applications/calibration/) section. 


#### Syntax

`/control/playCalibration <float leveldBFS> <int channelNumber>`

`leveldBFS`: The signal level at which the calibration audio was played, measured in dBFS.

`channelNumber`: <span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.7.0</span> This indicates the output channels of the audio interface to which the command applies. This value must always be an even number. For example, if channel 0 is selected, channels 0–1 are affected; if 2 is selected, channels 2–3 are affected. This argument is required. The command applies to the specified output pair independently of the listener selected in the GUI. 

#### Return 

`/control/actionResult /control/playCalibration <string actionName> <boolean success> <string description>`. 

The return confirmation refers to the action `calibration`, indicating `success=true` if the calibration has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details. 

#### Example

BeRTA receives: `/control/playCalibration -40 0`

BeRTA sends back to the sender: `/control/actionResult /control/playCalibration calibration true "Calibration sound played"`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/setCalibration**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Set the calibration values using the dBFS playback volume and the dBSPL output level measured in the headphones, both specified in the parameters. For more details, refer to the [calibration](/BRT-Documentation/applications/calibration/) section.

#### Syntax

`/control/setCalibration <float leveldBFS> <float leveldBSPL> <int channelNumber>`

`leveldBFS`: The signal level at which the calibration audio was played, measured in dBFS.

`leveldBSPL`: The output sound level measured at the headphones (expressed in dBSPL) when the system is calibrated and delivering the dBFS specified in the leveldBFS parameter.

`channelNumber`: <span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.7.0</span> This indicates the output channels of the audio interface to which the command applies. This value must always be an even number. For example, if channel 0 is selected, channels 0–1 are affected; if 2 is selected, channels 2–3 are affected. This argument is required. The command applies to the specified output pair independently of the listener selected in the GUI.

#### Return 

`/control/actionResult /control/setCalibration <string actionName> <boolean success> <string description>`. 

The return confirmation refers to the action `calibrationDone`, indicating `success=true` if the calibration has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details. 

#### Example

BeRTA receives: `/control/setCalibration -40 60 0`

BeRTA sends back to the sender: `/control/actionResult /control/setCalibration calibrationDone true "Calibration done with -40 dB FS and 60 dB SPL."`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/playCalibrationTest**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Play the calibration test sound at a volume adjusted to match the dBSPL specified in the parameter, allowing verification with the sound level meter. See the [calibration](/BRT-Documentation/applications/calibration/) section for more details.

#### Syntax

`/control/playCalibrationTest  <float leveldBSPL> <int channelNumber>`

`leveldBSPL`: The signal level (in dBSPL) measured at the headphones when reproducing the calibration audio.

`channelNumber`: <span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.7.0</span> This indicates the output channels of the audio interface to which the command applies. This value must always be an even number. For example, if channel 0 is selected, channels 0–1 are affected; if 2 is selected, channels 2–3 are affected. This argument is required. The command applies to the specified output pair independently of the listener selected in the GUI.

#### Return 

`/control/actionResult /control/playCalibrationTest <string actionName> <boolean success> <string description>`. 

The return confirmation refers to the action `calibrationTest`, indicating `success=true` if the calibration has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details. 

#### Example

BeRTA receives: `/control/playCalibrationTest 80 2`

BeRTA sends back to the sender: `/control/actionResult /control/playCalibrationTest calibrationTest false "ERROR BeRTA has not been calibrated yet"`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/stopCalibrationTest**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Stop the calibration test sound playback.

#### Syntax

`/control/stopCalibrationTest <int channelNumber>`

`channelNumber`: <span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.7.0</span> This indicates the output channels of the audio interface to which the command applies. This value must always be an even number. For example, if channel 0 is selected, channels 0–1 are affected; if 2 is selected, channels 2–3 are affected. This argument is required. The command applies to the specified output pair independently of the listener selected in the GUI.

#### Return 

`/control/actionResult /control/stopCalibrationTest <string actionName> <boolean success> <string description>`. 

The return confirmation refers to the action `calibrationTest`, indicating `success=true` if the stop of the calibration has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details. 

#### Example

BeRTA receives: `/control/stopCalibrationTest 0`

BeRTA sends back to the sender: `/control/actionResult /control/stopCalibrationTest calibrationTest true "Calibration test stopped."`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

## **Sound levels**

The following commands distinguish between the estimated physical output level after safety limiting, the theoretical calibrated level of a listener, and the digital signal level of a listener.

They replace the removed `/control/getSoundLevel` command.

Each query returns the requested output channel or listener ID, followed by `valid`, the left and right levels, and an `error` string.

A valid silent signal is represented by `valid=true` and negative infinity for the corresponding level. An unavailable measurement is represented by `valid=false`, negative infinity for both levels, and an explanatory error message.

In the examples below, `-inf` represents a floating-point negative infinity value, not an OSC string. `""` represents an empty OSC string.

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/getOutputSoundLeveldBSPL**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.15.0</span>

Get the latest estimated sound level in dBSPL for a physical stereo output pair, after applying the safety limiter. The estimate uses the output calibration and the processed digital signal. It is not a live acoustic measurement from a sound level meter.

This command queries the output pair independently of the listener selected in the GUI.

#### Syntax

`/control/getOutputSoundLeveldBSPL <int leftOutputChannel>`

`leftOutputChannel`: Zero-based index of the left output channel. It must be a non-negative even number identifying an available stereo pair in the selected audio interface. For example, `0` selects channels 0–1 and `2` selects channels 2–3.

#### Return

`/control/getOutputSoundLeveldBSPL <int leftOutputChannel> <bool valid> <float leftChannelSoundLevel> <float rightChannelSoundLevel> <string error>`

`leftOutputChannel`: Output channel requested.

`valid`: `true` if a current output measurement is available; otherwise, `false`.

`leftChannelSoundLevel`, `rightChannelSoundLevel`: Estimated left and right output levels in dBSPL, including the effect of the safety limiter.

`error`: Empty when `valid=true`; otherwise, describes why the measurement is unavailable.

When a valid silence measurement has been published for the output pair, both levels are `-inf`. This includes output pairs receiving no signal, and does not require calibration. If a signal is being processed but the output pair is not calibrated, the measurement is invalid. Invalid output pairs and unavailable current measurements also return `valid=false`.

#### Examples

Assuming that BeRTA receives: `/control/getOutputSoundLeveldBSPL 0`. Then: 

- For an uncalibrated output pair processing a signal:
    
    BeRTA sends: `/control/getOutputSoundLeveldBSPL 0 false -inf -inf "Output channels 0-1 have an active signal but are not calibrated."`

- If channels 0-1 are calibrated, they have a calibration offset of 100 dB and their digital levels are –24.6 dBFS and –18.7 dBFS. And that the safety limiter is not configured or is not working: 

    BeRTA sends: `/control/getOutputSoundLeveldBSPL 0 true 75.4 81.3""`

- If channels 0-1 are calibrated, they have a calibration offset of 100 dB and their digital levels are –24.6 dBFS and –18.7 dBFS. And that the safety limiter is reducing their output to 80 dBSPL: 
    
    BeRTA sends: `/control/getOutputSoundLeveldBSPL 0 true 75.4 80 ""`

- For a valid silent output pair:    
    
    BeRTA sends: `/control/getOutputSoundLeveldBSPL 0 true -inf -inf ""`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/getListenerSoundLeveldBSPL**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.15.0</span>

Get the latest theoretical sound level in dBSPL for a listener. The result is calculated from the listener's own digital signal level in dBFS and the calibration offset of its resolved output pair:

`listener level in dBSPL = listener level in dBFS + calibration offset in dB`

For a listener with a *direct output assignment*, its assigned pair provides the calibration. For a *virtual listener*, its calibration reference identifies another listener with a direct output assignment. The current output pair of that referenced listener provides the calibration. The reference follows the listener if its output assignment changes.

The safety limiter is *not applied* to this result. The Active/Standby state of either listener does not affect the calculation. Consequently, the theoretical level may exceed the physical safety limit.

#### Syntax

`/control/getListenerSoundLeveldBSPL <string listener_id>`

`listener_id`: Identifier of an existing listener.

#### Return

`/control/getListenerSoundLeveldBSPL <string listener_id> <bool valid> <float leftChannelSoundLevel> <float rightChannelSoundLevel> <string error>`

`listener_id`: Listener requested.

`valid`: `true` if a current listener measurement and a valid calibration are available; otherwise, `false`.

`leftChannelSoundLevel`, `rightChannelSoundLevel`: Theoretical left and right levels in dBSPL, before physical output limiting.

`error`: Empty when `valid=true`; otherwise, describes why the measurement is unavailable.

The result is invalid if the listener does not exist, has no current digital measurement, has no direct output or valid calibration reference, or resolves to an unavailable or uncalibrated output pair.

A silent listener returns `valid=true` and `-inf` levels only when the required measurement and calibration are available.

#### Example

- Assuming:
    - that Listener A is assigned to channels 0–1 in Active.
    - that Channels 0–1 have a calibration offset of 100 dB. 
    - that Listener A has digital levels of -29 dBFS and -27 dBFS.

    BeRTA receives: `/control/getListenerSoundLeveldBSPL B`

    BeRTA sends: `/control/getListenerSoundLeveldBSPL B true 71 73 ""`

- Assuming:
    - that Listener A is assigned to channels 0–1 in Standby
    - that Listener B is virtual and references Listener A. 
    - that Channels 0–1 have a calibration offset of 100 dB. 
    - that Listener B has digital levels of -30 dBFS and -28 dBFS.

    BeRTA receives: `/control/getListenerSoundLeveldBSPL B`

    BeRTA sends: `/control/getListenerSoundLeveldBSPL B true 70 72 ""`

    The result uses Listener B's signal and the calibration of channels 0–1. Listener A being in Standby does not affect it.

    If Listener B has no calibration reference:

    BeRTA sends: `/control/getListenerSoundLeveldBSPL B false -inf -inf "Listener B has no direct output or valid calibration reference."`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/control/getListenerSoundLeveldBFS**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.15.0</span>

Get the latest digital signal level in dBFS for a listener's left and right channels, before physical output limiting. This command applies to both routed and virtual listeners. It does not require an output assignment or calibration, and the listener's Active/Standby state does not affect the result.

#### Syntax

`/control/getListenerSoundLeveldBFS <string listener_id>`

`listener_id`: Identifier of an existing listener.

#### Return

`/control/getListenerSoundLeveldBFS <string listener_id> <bool valid> <float leftChannelSoundLevel> <float rightChannelSoundLevel> <string error>`

`listener_id`: Listener requested.

`valid`: `true` if a current digital measurement is available; otherwise, `false`.

`leftChannelSoundLevel`, `rightChannelSoundLevel`: Left and right digital signal levels in dBFS.

`error`: Empty when `valid=true`; otherwise, describes why the measurement is unavailable.

A valid silent signal returns `-inf` for the corresponding channel. A missing listener or an unavailable current measurement returns `valid=false`.

#### Example

- BeRTA receives: `/control/getListenerSoundLeveldBFS B`

    BeRTA sends: `/control/getListenerSoundLeveldBFS B true -30 -28 ""`

- For a valid silent listener:

    BeRTA sends: `/control/getListenerSoundLeveldBFS B true -inf -inf ""`

- If the requested listener does not exist:

    BeRTA receives: `/control/getListenerSoundLeveldBFS Unknown`

    BeRTA sends: `/control/getListenerSoundLeveldBFS Unknown false -inf -inf "Listener Unknown does not exist."`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

## **Safety Limiter**

### **/control/setSoundLevelLimit**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.4.0</span>

Set the sound level limit to the dBSPL value specified in the parameter. See the [safety limitter](/BRT-Documentation/applications/safety-limiter/) for more details.

#### Syntax

`/control/setSoundLevelLimit <float levelLimitdBSPL> <int channelNumber>`

`levelLimitdBSPL`: The maximum allowable sound level, in dBSPL, used by the limiter to ensure safe listening levels for the user.

`channelNumber`: <span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.7.0</span> This indicates the output channels of the audio interface to which the command applies. This value must always be an even number. For example, if channel 0 is selected, channels 0–1 are affected; if 2 is selected, channels 2–3 are affected. This argument is required. The command applies to the specified output pair independently of the listener selected in the GUI.

#### Return 

`/control/actionResult /control/setSoundLevelLimit <string actionName> <boolean success> <string description>`. 

The return confirmation refers to the action `calibrationTest`, indicating `success=true` if the set has been successfully performed and `success=false` if not. In both cases a `description` is added to give more details. 

#### Example

BeRTA receives: `/control/setSoundLevelLimit 100 0`

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
