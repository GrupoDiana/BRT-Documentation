# Listener Acoustic Model based on HRTF direct convolution

The **Listener HRTF model** module of the library allows the rendering of spatial audio from multiple sound sources. It simulates the direct path between source and listener by taking advantage of *direct convolution* with head impulse responses (HRTF). This module independently processes each of the input sources, taking into account their positions and the position and orientation of the listener.

Subsequently, this model performs a *near-field correction*, independent for each of the sources. This correction is made by filtering the signals corresponding to each ear. The coefficients of these filters are dependent on the distance between source and listener and the interaural azimuth.

Finally, the model mixes all channels, left and right separately, to give a single output per ear.

The convolutions are performed in the frequency domain using the uniformly partitioned convolution algorithm. Filtering is performed in the time domain.
The HRTF service module is responsible for providing the impulse responses in each frame. While the SOS filter module is in charge of providing the filters coefficients. Both classes must have been previously configured, read more in the [services modules section](../../service-modules/index.md).


## Architecture

The internal block diagram of this class is as follows:
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/ListenerHRTFModel_InternalBlockDiagram.png" alt="Listener HRTF Model Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Listener HRTF Model Internal diagram.</p>
</div>

The operation of the convolucionator as well as its block diagram can be seen in this link (:warning:URL).

## Configuration Options

This model allows configuration by calling its methods or by BRT internal commands:

- **Model (on/off)**: Silent when off.
- **Gain (float)**: Extra gain to be applied to the model output.
- **Spatialization (on/off)**: Transparent when off.
- **Interpolation (on/off)**: When switched on, HRIRs and delays are calculated at the exact position (relative source-listener position). For this purpose, barycentric interpolation is performed, starting from the three closest points. When it is switched off, the HRIR and delay with the closest position are chosen.
- **Near Field Compensation (on/off)**: The near field correction is applied when on. 
- **ITD Simulation (on/off)**: When activated, a separate delay is added to each ear to simulate the interaural time difference[^1]. This delay is provided by the HRTF service module and may be provided in the SOFA structure or will be calculated from the head size, for more information see (:warning:URL). When off, it does not simulate interaural time difference. 
- **Parallax Correction (on/off)**: When it is switched on, a cross-ear parallax correction is apllied. This correction is based on calculating the projection of the vector from the ear to the source on the HRTF sphere (i.e. the sphere on the surface of which the HRTF was measured), giving a more accurate rendering, especially for near-field and far-field sound sources. When deactivated, the calculation is based on the centre of the listener's head.
- **HRTF to be used**: The HRTF service module to be used for rendering. The system supports dynamic, hot-swapping of the service module being used.
- **Nearfield filter (SOS filter) to be used**: The SOS filter service module to be used for rendering. The system supports dynamic, hot-swapping of the service module being used.

[^1]: To perform the convolution task correctly, avoiding comb filters, it is necessary that the delays of the impulse responses have been removed, for more information see (:warning:URL).
## Connections
Modules to which it supports connections: 

    - Source models
    - Environment models

Modules to which it connects:

    - Listener
    - Binaural Filter


<details>
<summary>For C++ developer</summary>

<ul>
<li><strong>File</strong>: /include/ListenerModels/ListenerHRTFModel.hpp</li>
<li><strong>Class name</strong>: CListenerHRTFModel</li>
<li><strong>Inheritance</strong>: CListenerModelBase</li>
<li><strong>Namespace</strong>: BRTListenerModel</li>
<li><strong>Classes that instance</strong>:
    <ul>
        <li>BRTProcessing::CHRTFConvolverProcessor</li>
        <li>BRTProcessing::CNearFieldEffectProcessor</li>
    </ul>
</li>
</ul> 

<h2>Class inheritance diagram</h2>
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/HRTFListenerModel.png" alt="Listener HRTF Model Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Listener HRTF Model Internal diagram.</p>
</div>
<br>

<h2>How to instantiate</h2>


```cpp
// Assuming that the ID of this listener model is contained in _listenerModelID.
brtManager.BeginSetup();
std::shared_ptr<BRTListenerModel::CListenerHRTFModel>listenerModel = brtManager.CreateListenerModel<BRTListenerModel::CListenerHRTFModel>(_listenerModelID);
brtManager.EndSetup();
if (listenerModel == nullptr) {
    // ERROR
}
```
<h2>How to connect</h2>
Connect it to a listener.

```cpp
// Assuming that the ID of this listener is contained in _listenerID and 
// that the ID of this listener model is contained in _listenerModelID.
std::shared_ptr<BRTBase::CListener> listener = brtManager.GetListener(_listenerID);
if (listener != nullptr) {
    brtManager.BeginSetup();
    bool control = listener->ConnectListenerModel(_listenerModelID);
    brtManager.EndSetup();
}
```

Connect an environment model to it.
```cpp
// Assuming that the ID of this listener model is contained in _listenerModelID.
// that the ID of this environment is contained in _environmentModelID.
std::shared_ptr<BRTListenerModel::CListenerModelBase> listenerModel = brtManager.GetListenerModel<BRTListenerModel::CListenerModelBase>(_listenerModelID);
if (listenerModel != nullptr) {
    brtManager.BeginSetup();
    bool control = listenerModel->ConnectEnvironmentModel(_environmentModelID);
    brtManager.EndSetup();
}
```

Connect a source model to it.

```cpp
// Assuming that the soundSource could be a ID(string) or a std::shared_ptr<BRTSourceModel::CSourceModelBase>;
std::shared_ptr<BRTListenerModel::CListenerModelBase> listenerModel = brtManager->GetListenerModel<BRTListenerModel::CListenerModelBase>(_listenerModelID);
if (listenerModel != nullptr) {			
	bool control = listenerModel->ConnectSoundSource(soundSource);
}

```


<h2>Public methods</h2>

```cpp
void EnableModel() override 
void DisableModel() override

void EnableSpatialization() override 
void DisableSpatialization() override
bool IsSpatializationEnabled() override

void EnableInterpolation() override 
void DisableInterpolation() override 
bool IsInterpolationEnabled() override

void EnableNearFieldEffect() override
void DisableNearFieldEffect() override
bool IsNearFieldEffectEnabled() override

void EnableITDSimulation() override
void DisableITDSimulation() override
bool IsITDSimulationEnabled() override

void EnableParallaxCorrection() override
void DisableParallaxCorrection() override 
bool IsParallaxCorrectionEnabled() override

bool SetHRTF(std::shared_ptr< BRTServices::CHRTF > _listenerHRTF) override
std::shared_ptr<BRTServices::CHRTF> GetHRTF() const override
void RemoveHRTF() override

bool SetNearFieldCompensationFilters(std::shared_ptr<BRTServices::CSOSFilters> _listenerILD) override
std::shared_ptr<BRTServices::CSOSFilters> GetNearFieldCompensationFilters() const override
void RemoveNearFierldCompensationFilters() override

bool ConnectSoundSource(std::shared_ptr<BRTSourceModel::CSourceModelBase> _source) override
bool ConnectSoundSource(const std::string & _sourceID) override
bool DisconnectSoundSource(std::shared_ptr<BRTSourceModel::CSourceModelBase> _source) override
bool DisconnectSoundSource(const std::string & _sourceID) override 

bool ConnectEnvironmentModel(const std::string & _environmentModelID) override 
bool DisconnectEnvironmentModel(const std::string & _environmentModelID) override

void ResetProcessorBuffers()
void UpdateCommand() override
```


</details>






