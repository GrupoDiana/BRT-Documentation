# SDN Environment Model  
:warning:*(Ready for review)*:warning:

The **SDN Environment Model** implements an acoustic room simulation using **Scattering Delay Networks (SDNs)**. This approach models the acoustics of a room by employing acoustic reverberators, as described by Enso De Sena et al. in the paper <a href="https://ieeexplore.ieee.org/document/7113826" target="_blank">Efficient Synthesis of Room Acoustics via Scattering Delay Networks</a>.

Scattering Delay Networks simulate the acoustics of an enclosure using a network of delay lines connected by scattering junctions. The parameters of the model are derived from the physical properties of the simulated room, enabling a realistic approximation of the room's acoustic behavior. SDNs accurately model first-order reflections and make progressively coarser approximations for higher-order reflections. The algorithm supports unequal and frequency-dependent wall absorption, directional sound sources, and microphones, making it versatile for various room configurations.  

## Functional overview

The **SDN Environment Model** simulates the acoustics of a shoebox-shaped room by processing input signals from connected source models and generating virtual output sources linked to the corresponding listener models. For each connected source, the model creates seven virtual output sources: six representing the reflections from the walls, ceiling, and floor, and one representing the direct sound path. The user only needs to specify the listener model to which the SDN Environment Model is connected, allowing sources to be freely added or removed as required.  

For each source, the model constructs scattering nodes and waveguides corresponding to the six reflective surfaces of the room. These virtual sources are positioned based on the geometry of the room and the relative positions of the source and the listener, with the delay and attenuation for each source calculated accordingly. The additional virtual source representing the direct sound path ensures the simulation includes both the reflections and the direct propagation.

To configure the simulation accurately, users must define the room’s dimensions and the absorption coefficients for each wall. The model assumes the room’s center is located at the origin of the coordinate system (0, 0, 0), ensuring a consistent spatial framework for the simulation.  The shoebox-shaped room is described by three parameters:  

- **Length**: The dimension of the room along the X-axis.  
- **Width**: The dimension of the room along the Y-axis.  
- **Height**: The dimension of the room along the Z-axis.  

Furthermore, the definition of absorption coefficients is required for each wall, which can be defined as either frequency-independent or frequency-dependent (9 bands).

## Architecture

The internal block diagram of this class is as follows:
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="SDN environment Model Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">SDN environment Model Internal diagram.</p>
</div>

## Configuration Options

This model allows configuration by calling its methods or by BRT internal commands:

- **Model (on/off)**: Silent when off.
- **Gain (float)**: Extra gain to be applied to the model output.
- **Direct path (on/off)**: Direct path silent when off.
- **Reverb path (on/off)**: Reverb path silent when off.
- **Setup ShoeBox Room (dimensions)**: Set the room geometry.
- **Setup Room Wall Absortion (absortion)**: Set walls absortion coefficients.


## Connections
Modules to which it supports connections: 

    - Source models

Modules to which it connects:

    - Listener models    


<details>
<summary>For C++ developer</summary>

<ul>
<li><strong>File</strong>: /include/EnvironmentModels/CSDNEnvironmentModel.hpp</li>
<li><strong>Class name</strong>: CSDNEnvironmentModel</li>
<li><strong>Inheritance</strong>: CEnviromentModelBase</li>
<li><strong>Namespace</strong>: BRTEnvironmentModel</li>
<li><strong>Classes that instance</strong>:
    <ul>
        <li>BRTEnvironmentModel::CSDNEnvironmentProcessor</li>        
        <li>Common::CRoom</li>
    </ul>
</li>
</ul> 

<h2>Class inheritance diagram</h2>
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="Free field Model Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Free field Model Internal diagram.</p>
</div>
<br>

<h2>How to instantiate</h2>

```cpp
// Assuming that the ID of this environment model is contained in _environmentID.
brtManager.BeginSetup();
std::shared_ptr<BRTEnvironmentModel::CSDNEnvironmentModel> environmentModel = brtManager.CreateEnvironment<BRTEnvironmentModel::CSDNEnvironmentModel>(_environmentID);
brtManager.EndSetup();
if (environmentModel == nullptr) {
	// error	
}
```
<h2>How to connect</h2>
Connect it to a listener model.

```cpp
// Assuming that the ID of this environment model is contained in _environmentModelID and 
// that the ID of this listener model is contained in _listenerModelID.
std::shared_ptr<BRTListenerModel::CListenerModelBase> listenerModel = brtManager.GetListenerModel<BRTListenerModel::CListenerModelBase>(_listenerModelID);
if (listenerModel != nullptr) {
	brtManager.BeginSetup();
    bool control = listenerModel->ConnectEnvironmentModel(_environmentModelID);
    brtManager.EndSetup();
}
```

Connect a source model to it.
```cpp
// Assuming that the ID of this source model is contained in _sourceID and 
// that the ID of this environment is contained in _environmentModelID.
std::shared_ptr<BRTEnvironmentModel::CEnviromentModelBase> environmentModel = brtManager->GetEnvironmentModel<BRTEnvironmentModel::CEnviromentModelBase>(_environmentModelID);
if (environmentModel != nullptr) {			
	bool control = environmentModel->ConnectSoundSource(_sourceID);
}
```


<h2>Public methods</h2>

```cpp
void EnableModel() override 
void DisableModel() override

void SetGain(float _gain) override
float GetGain() 

bool SetupShoeBoxRoom(float length, float width, float height)
Common::CRoom GetRoom()

bool SetRoomWallAbsortion(int wallIndex, float absortion)
bool SetRoomAllWallsAbsortion(float _absortion)
bool SetRoomWallAbsortion(int wallIndex, std::vector<float> absortionPerBand)
bool SetRoomAllWallsAbsortion(std::vector<float> absortionPerBand)

void EnableDirectPath() override 
void DisableDirectPath() override 
bool IsDirectPathEnabled() override 

void EnableReverbPath() override 
void DisableReverbPath() override 
bool IsReverbPathEnabled() override 

bool ConnectSoundSource(std::shared_ptr<BRTSourceModel::CSourceModelBase> _source) override
bool ConnectSoundSource(const std::string & _sourceID) override

bool DisconnectSoundSource(std::shared_ptr<BRTSourceModel::CSourceModelBase> _source) override
bool DisconnectSoundSource(const std::string & _sourceID) override

void ResetProcessorBuffers()

void UpdateCommand() override
```


</details>