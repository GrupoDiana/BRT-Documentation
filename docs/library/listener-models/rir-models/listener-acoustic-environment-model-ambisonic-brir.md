# Listener Acoustic Model based on BRIR convolution in the Ambisonics domain

The **Ambisonic BRIR Convolution Model** enables spatial audio rendering from multiple sound sources. It performs convolution in the ambisonic domain using binaural room impulse responses (BRIR[^1]) and the selected ambisonic order (currently up to third order). This process simulates both direct sound and reverberation, providing a complete representation of the acoustic interaction between the source and the listener. If the impulse responses do not include information about the direct path, the simulation is limited to the reverberation of the environment[^2]. The process consists of two main stages: ambisonic encoding and ambisonic convolution/decoding.

[^1]: A BRIR captures the acoustic characteristics of a room from the perspective of a specific listener, as it is recorded using microphones placed in the listener's ears. This is why we refer to this model as both a listener and environment model.
[^2]: This is the standard way we use the library.

In the first stage, a ambisonic encoding is performed for each input sound source. This encoding generates (N) ambisonic channels per ear, where (N) depends on the selected ambisonic order, and per sound source.

In the second stage, two tasks are performed simultaneously: convolution with impulse responses and ambisonic decoding (for more details, see the [AmbisonicBIR](../../service-modules/service-ambisonic-bir.md) section). 
There are two convolution/decoding blocks, one for each ear, allowing independent processing for the left and right channels. Each block begins by separately mixing the ambisonic channels generated during the encoding stage. It then performs convolution in the frequency domain for each channel using precomputed impulse responses stored in the [AmbisonicBIR](../../service-modules/service-ambisonic-bir.md) service module. These impulse responses represent the ambisonic mixture of the virtual loudspeaker responses, enabling both convolution and ambisonic decoding to be executed simultaneously.
Finally, the output is mixed and transformed back into the time domain, resulting in the final signal for the corresponding ear.

This modular approach ensures efficient and precise spatial audio rendering. The linearity of the operations is leveraged to reduce computational overhead by minimizing the number of convolutions required. The result is a highly realistic simulation of the direct sound path in the ambisonic domain, offering support for scalable ambisonic orders up to the third order.

For further details on the functionality of the [Bilateral Ambisonic Encoder](../../processing-modules/bilateral-ambisonic-encoder.md) and the [Ambisonic Domain Convolver](../../processing-modules/ambisonic-domain-convolver.md), refer to their respective sections in the documentation. 

### Distance Attenuation
While not entirely accurate, this model also includes a distance simulation, which is disabled by default. This is a simulation of the distance attenuation due to propagation in space. It is based on the **inverse square law**, which states that the intensity of sound decreases proportionally to the square of the distance to the source. This phenomenon, termed d**distance-based attenuation**, is a critical factor in our perception of sound intensity.

The input signal is **attenuated based on the distance** between the source and the listener, replicating the natural reduction in sound intensity over distance. This means that a fixed amount of attenuation is applied to the signal each time the distance is doubled from a reference distance. By default, this attenuation has a value of -3 dB and the reference distance is 1 metre. Thus, the applied attenuation is calculated using the following expression: 

$attenuation = 10^{(Distance Attenuation Factor/ -3 dB) * log_{10}(ReferenceDistance / distance)}$

## Architecture

The internal block diagram of this class is as follows:
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/ListenerAmbisonicEnvironmentBRIRModel_InternalBlockDiagram.png" alt="Ambisonic BRIR Convolution Model - Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Ambisonic BRIR Convolution Model - Internal diagram.</p>
</div>>


## Configuration Options

This model allows configuration by calling its methods or by BRT internal commands:

- **Model (on/off)**: Silent when off.
- **BRIR to be used**: The BRIR service module to be used for rendering. The system supports dynamic, hot-swapping of the service module being used.
- **Ambisonic Order**: The order of the ambisonic coding to be used. Currently only orders between 1 (default) and 3 are valid.
- **Ambisonic Normalization**: The ambisonic normalization to be used. The available options are: N3D (default), SN3D, maxN 
- **Distance Attenuation (on/off)**: No attenuation is applied when disabled.
- **Distance Attenuation Factor**: Sets a new distance attenuation factor, default is -3dB.


## Connections
Modules to which it supports connections: 

    - Source models    

Modules to which it connects:

    - Listener
    - Binaural Filter



<details>
<summary>For C++ developer</summary>

<ul>
<li><strong>File</strong>: /include/ListenerModels/ListenerAmbisonicEnvironmentBRIRModel.hpp</li>
<li><strong>Class name</strong>: CListenerAmbisonicEnvironmentBRIRModel</li>
<li><strong>Inheritance</strong>: CListenerModelBase</li>
<li><strong>Namespace</strong>: BRTListenerModel</li>
<li><strong>Classes that instance</strong>:
    <ul>
        <li>BRTProcessing::CAmbisonicDomainConvolverProcessor</li>
        <li>BRTProcessing::CBilateralAmbisonicEncoderProcessor</li>
    </ul>
</li>
</ul> 

<h2>Class inheritance diagram</h2>
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="ListenerAmbisonicEnvironmentBRIRModel class diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">ListenerAmbisonicEnvironmentBRIRModel class diagram.</p>
</div>
<br>

<h2>How to instantiate</h2>


```cpp
// Assuming that the ID of this listener model is contained in _listenerModelID.
brtManager.BeginSetup();
std::shared_ptr<BRTListenerModel::CListenerAmbisonicEnvironmentBRIRModel>listenerModel = brtManager.CreateListenerModel<BRTListenerModel::CListenerAmbisonicEnvironmentBRIRModel>(_listenerModelID);
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

bool SetAmbisonicOrder(int _ambisonicOrder) override
int GetAmbisonicOrder() override 

bool SetAmbisonicNormalization(Common::TAmbisonicNormalization _ambisonicNormalization) override 
bool SetAmbisonicNormalization(std::string _ambisonicNormalization) override 
Common::TAmbisonicNormalization GetAmbisonicNormalization() override 

void EnableDistanceAttenuation() override
void DisableDistanceAttenuation() override
bool IsDistanceAttenuationEnabled() override

bool SetDistanceAttenuationFactor(float _distanceAttenuationFactorDB) override
float GetDistanceAttenuationFactor() override

bool SetHRBRIR(std::shared_ptr<BRTServices::CHRBRIR> _listenerBRIR) override
std::shared_ptr<BRTServices::CHRBRIR> GetHRBRIR() const override
void RemoveHRBRIR() override

bool ConnectSoundSource(std::shared_ptr<BRTSourceModel::CSourceModelBase> _source) override
bool ConnectSoundSource(const std::string & _sourceID) override
bool DisconnectSoundSource(std::shared_ptr<BRTSourceModel::CSourceModelBase> _source) override
bool DisconnectSoundSource(const std::string & _sourceID) override 

void ResetProcessorBuffers()
void UpdateCommand() override
```


</details>






