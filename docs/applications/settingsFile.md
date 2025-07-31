# Configuration File Setup

This section explains how to define a configuration file in JSON format that is read when the application starts. If the program is launched without arguments, it will look for a file called `settings.json` in the same folder. Alternatively, a custom configuration file can be provided as an argument, for example, `mysettings.json`.  This file defines the audio interface settings, the model architecture, and, optionally, a set of resources to be re-loaded at the begining and a complete scenario with sources and listeners.

## General Structure

The JSON configuration file is organized into several key sections that define settings such as general parameters, model architecture, resources (HRTFs, BRIRs, etc.), and scene configuration. Below is a guide on how to construct each section.

### 1. General Settings

These settings control general application parameters.

- **SampleRate**: The sample rate in Hz. Example: `48000`.
- **BufferSize**: The size of the buffer in samples. Example: `512`.
- **BufferFrames**: Controls stream latency. Larger values may provide better performance but increase latency.
- **OSCListenPort**: The port on which the OSC server listens for messages.
- **ExtrapolationMethod**: The method of extrapolation. Available options: `ZeroInsertion`, `NearestPoint`.
- **LogFile** : Selects whether to save a log file or not. `true`, `false`.
- **LogFilePath** : Folder where the log file is stored. By default the application saves the log files in the subfolder ‘data’ inside the installation folder. On Windows, if the installation folder is ‘Program Files’, you will need to run the program with administrator permissions.

Example: 

```
"GeneralSettings": {
  "SampleRate": 48000,
  "BufferSize": 512,
  "BufferFrames": 4,
  "OSCListenPort": 10017,
  "ExtrapolationMethod": "NearestPoint",
  "LogFile": true,
  "LogFilePath" : "",
},
```

### 2. Models Architecture

This section defines the models used for sound rendering, including listeners and environment models.

- **Listeners**: Defines the physical listener(s) by ID. Example: `["DefaultListener"]`.
- **ListenerModels**: Specifies the listener models, identified by an ID and model type. The available types include `ListenerDirectHRTFConvolution`, `ListenerDirectBRIRConvolutionModel`, `ListenerAmbisonicVirtualLoudspeakersModel`, and `ListenerAmbisonicReverberantVirtualLoudspeakersModel`.
- **EnvironmentModels**: Specifies the environment models, such as the `SDNEnvironmentModel` and the `FreeFieldEnvironmentModel`.
- **Binaural Filters**: Specifies the binaural filter used. The available binaural filter model is the `SOSBinauralFilter`.
- **ConnectSourcesTo**: Defines which models the audio sources connect to when loaded. Sources can be connected to either listener models or environment models.
- **Model2ModelConnections**: Defines how are the interconnections between the different models (environment, listening and binaural filters).  The outputs of the `OriginID` models will be connected to the inputs of the `DestinationID` models.
- **ConnectToListener**: Specifies how the listener models are connected to the listener. The output of the listener models is connected to the listener for mixing.

Example:

```
	"ModelsArchitecture": {	
		"Listeners": ["DefaultListener"],		
		"ListenerModels": [
			{"ID": "DirectPath", "Model": "ListenerDirectHRTFConvolution"},							
			{"ID": "ReverbPath", "Model": "ListenerAmbisonicReverberantVirtualLoudspeakersModel"}	
		],
		"EnvironmentModels": [{"ID": "FreeField", "Model": "FreeFieldEnvironmentModel"}],
		"BinauralFilters":[],
		"ConnectSourcesTo":["FreeField", "ReverbPath"],				
		"Model2ModelConnections": [{"OriginID":"FreeField", "DestinationID":"DirectPath"}],		
		"ConnectToListener":[
      {"ModelID":"DirectPath", "ListenerID":"DefaultListener"},
			{"ModelID":"ReverbPath", "ListenerID":"DefaultListener"}
		],
  },
```

### 3. Resources

This section lists the resources such as HRTFs, BRIRs, NearField Filters, and DirectivityTFs that are loaded when the application starts.

- **HRTFs**: A list of HRTFs (Head-Related Transfer Functions) to load. If no HRTFs should be loaded, leave the list empty (`[]`).
- **BRIRs**: A list of BRIRs (Binaural Room Impulse Responses) to load. BRIRs can provide spatial audio information specific to a particular room. If none should be loaded, leave this list empty (`[]`).
- **SOSFilters**: Defines the SOS filters to load. If no SOS Filter should be loaded, leave the list empty (`[]`)
- **DirectivityTFs**: A list of Directivity Transfer Functions (TFs) to apply to sound sources. These describe how sound is emitted in different directions from a sound source.

Example: 

```
"Resources" : {
		"HRTFs": [{"ID": "HRTF1", "fileName": "resources//HRTF//3DTI_HRTF_SADIE_II_D2_256s_48000Hz_resampled5.sofa", "spatialResolution": 5}],		
		"BRIRs": [{"ID": "BRIR1", "fileName": "resources//BRIR//3DTI_BRIR_Trapezoid_48000Hz_3D.sofa", "spatialResolution": 15, "fadeInWindowThreshold" : 0, "fadeInWindowRiseTime": 0, "fadeOutWindowThreshold" : 0, "fadeOutWindowRiseTime": 0}],		
		"SOSFilters": [
			{"ID": "DefaultNFFilters", "fileName": "resources//SOSFilters//NearFieldCompensation_ILD_1.2m_48Khz.sofa"},
			],
		"DirectivityTFs": [], 
	},
```

### 4. Sound Sources

This section defines the sound sources to be loaded at startup. Each source must specify the full filename and the model used for rendering. The available source model options are `OmnidirectionalModel` or `DirectivityModel`.

- **ID**: A unique identifier for each sound source.
- **fileName**: The full path to the sound file to be used.
- **sourceModel**: Specifies the rendering model for the sound source. The two available models are `OmnidirectionalModel` and `DirectivityModel`.

Example: 

```
	"SoundSources": [
		{"ID": "SoundSource1", "fileName": "resources//MusArch_Sample_48kHz_Anechoic_FemaleSpeech.wav", "sourceModel":"OmnidirectionalModel"},
		{"ID": "SoundSource2", "fileName": "resources//MusArch_Sample_48kHz_Anechoic_MaleSpeech.wav", "sourceModel":"OmnidirectionalModel"}
	],
```

### 5. Scene Configuration

This section allows you to define the scene by specifying commands in OSC (Open Sound Control) format. These commands configure various parameters like looping sound sources, setting HRTFs, and configuring environmental properties such as room dimensions and wall absorption coefficients.

- **command**: The OSC command to execute.
- **parameters**: The parameters that accompany each OSC command.

Example:

```
	"SceneConfiguration": [
		{"command": "/source/loop", "parameters": ["SoundSource1", "true"]},		
		{"command": "/listener/setHRTF", "parameters": ["DefaultListener", "HRTF1"]},
		{"command": "/listener/setSOSFilters", "parameters": ["DefaultListener", "DefaultNFFilters"]},		
		{"command": "/listener/setBRIR", "parameters": ["DefaultListener", "BRIR1"]}
	],
```
