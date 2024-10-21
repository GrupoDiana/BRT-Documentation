The OSC control commands allow to establish and manage connections with BeRTA over the network or locally. Below are detailed the basic commands for initiating and closing connections, as well as obtaining system and audio configuration information. Each command is designed to provide immediate feedback.

<!----------------------------------------------------------------------------------->
---

## `/control/connect`

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
---



## `/control/disconnect`

Stops the communication through which BeRTA sent information back. This is equivalent to unsibscribe the sender from updates sent back by BrRTA. When this message is received the same message is sent back.

#### Syntax

`/control/disconnect`. 

#### Return

An echo is sent back to the sender to confirm the reception of the command: `/control/disconnect`.

#### Example

BeRTA receives: `/control/disconnect`.

BeRTA sends back to the sender: `/control/disconnect`. 


<!----------------------------------------------------------------------------------->
---



## `/control/ping`

Responds with the same message to indicate that BeRTA is listening to the external app. 

#### Syntax

`/control/ping`. 

#### Return

An echo is sent back to the sender to confirm the reception of the command: `/control/ping`.

#### Example

BeRTA receives: `/control/ping`.

BeRTA sends back to the sender: `/control/ping`. 


<!----------------------------------------------------------------------------------->
---



## `/control/version`

Responds with a message to indicate the version of BeRTA. 

#### Syntax

`/control/version`. 

#### Return

An echo is sent back to the sender including the version of BeRTA: `/control/version <string version>`.

#### Example

BeRTA receives: `/control/version`.

BeRTA sends back to the sender: `/control/version BeRTA-Renderer v3.0.0`. 


<!----------------------------------------------------------------------------------->
---



## `/control/sampleRate`

Asks for the sample rate which BeRTA is using. This parameter is defined in the used [settings file](/BRT-Documentation/setup/settingsFile).

#### Syntax

`/control/sampleRate`. 

#### Return

An echo is sent back to the sender including the sample rate: `/control/sampleRate <int sample_rate>`.

`sample_rate`: Sample rate used by BeRTA in samples per second.

#### Example

BeRTA receives: `/control/sampleRate`.

BeRTA sends back to the sender: `/control/sampleRate 48000`. 


<!----------------------------------------------------------------------------------->
---



## `/control/frameSize`

Asks for the size of the audio frame which BeRTA is using. This parameter is defined in the used [settings file](/BRT-Documentation/setup/settingsFile).

#### Syntax

`/control/frameSize`. 

#### Return

An echo is sent back to the sender including the frame size: `/control/frameSize <int frame_size>`.

`frame_size`: size of the audio frame in bytes per channel (BeRTA produces a stereo output for each listener. This parameter indicated the size of the frame of only one channel).

#### Example

BeRTA receives: `/control/frameSize`.

BeRTA sends back to the sender: `/control/frameSize 512`. 


<!----------------------------------------------------------------------------------->
---



## `/control/bufferFrames`

Asks for the number of audio frames that are stored in the buffer to prevent audio dropout or system failure due to frame underrun. Since the operating system is not real-time, there can be instances where it prioritizes other tasks, potentially delaying the provision of a new audio frame. By setting an adequate buffer size, the system ensures continuous audio playback, even if the operating system momentarily allocates resources to higher-priority processes. This buffer acts as a safeguard against timing inconsistencies, allowing the audio processing thread to retrieve preloaded frames, maintaining uninterrupted audio output. However, as a trade-off, increasing the number of buffered frames results in higher system latency. To minimize latency, this parameter should be set to 0, which eliminates buffering but requires the system to deliver new audio frames in real time without delay. This parameter is defined in the used [settings file](/BRT-Documentation/setup/settingsFile).

 

#### Syntax

`/control/bufferFrames`. 

#### Return

An echo is sent back to the sender including the number of frames buffered: `/control/bufferFrames <int buffer_frames>`.

`buffer_frames`: number of buffered frames.

#### Example

BeRTA receives: `/control/bufferFrames`.

BeRTA sends back to the sender: `/control/bufferFrames 2`. 


<!----------------------------------------------------------------------------------->
---



