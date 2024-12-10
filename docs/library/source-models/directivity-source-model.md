# Directivity Source Model  
:warning:*(Ready for review)*:warning:

The **Directivity Source Model** provides an interface for applications to interact with the BRT Library while implementing frequency-dependent directivity for sound sources. This model takes into account the direction of the listener relative to the source and applies a corresponding filter to transform the audio signal as it would be radiated in that direction. Once processed, the model transmits the filtered audio samples to the connected modules, such as listener or environment models, while also providing real-time updates on the source's position and orientation. Applications must supply monaural audio samples for each frame and dynamically update the source's position and orientation as needed.  

## Functional Overview  

After instantiating the Directivity Source Model and connecting it to the desired modules (listener and/or environment models), the application manages the source’s position and orientation using the `SetSourceTransform` method. This allows for dynamic updates to the source's state. For each audio frame, the application must also provide the corresponding monaural audio samples via the `SetBuffer` method.

When the model receives the audio samples, it performs a convolution with the directivity data to account for the directional characteristics of the source. This data is supplied by the [DirectivityTF service module](../service-modules/service-directivity-tf.md), which provides frequency responses based on the source's orientation and the relative position between the source and the listener. The convolutions are executed in the frequency domain using the [uniformly partitioned convolution](../common/uniform-partitioned-convolution.md) algorithm, ensuring efficient and accurate processing.  


## Architecture

The internal block diagram of this class is as follows:
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="Directivity Sound Source Model Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Directivity Sound Source Model Internal diagram.</p>
</div>

## Configuration Options

This model allows configuration by calling its methods or by BRT internal commands:

- **Directivity (on/off)**: Omnidirectional sound source when off.
- **DirectivityTF to be used**: The DirectivityTF service module to be used for rendering. The system supports dynamic, hot-swapping of the service module being used.

## Connections

Modules to which it connects:

    - Environment models
    - Listener models    

<details>
<summary>For C++ developer</summary>

<ul>
<li><strong>File</strong>: /include/SourceModels/SourceDirectivityModel.hpp</li>
<li><strong>Class name</strong>: CSourceDirectivityModel</li>
<li><strong>Inheritance</strong>: CSourceModelBase</li>
<li><strong>Namespace</strong>: BRTSourceModel</li>
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
std::shared_ptr<BRTSourceModel::CSourceDirectivityModel> brtSoundSource = brtManager->CreateSoundSource<BRTSourceModel::CSourceDirectivityModel>(soundSourceID);
brtManager.EndSetup();
if (brtSoundSource == nullptr) {
	// error	
}
```
<h2>How to connect</h2>
Connect it to a listener model.

```cpp
// Assuming that the soundSource could be a ID(string) or a std::shared_ptr<BRTSourceModel::CSourceModelBase>;
std::shared_ptr<BRTListenerModel::CListenerModelBase> listenerModel = brtManager->GetListenerModel<BRTListenerModel::CListenerModelBase>(_listenerModelID);
if (listenerModel != nullptr) {			
	bool control = listenerModel->ConnectSoundSource(soundSource);
}
```

Connect it to a environment model.
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

void SetBuffer(const CMonoBuffer<float>& _buffer)
CMonoBuffer<float> GetBuffer()

void SetSourceTransform(Common::CTransform _transform)
const Common::CTransform& GetSourceTransform() const

std::string GetID()

TSourceType GetSourceType()

bool SetDirectivityTF(std::shared_ptr< BRTServices::CDirectivityTF > _sourceDirectivityTF) override
std::shared_ptr<BRTServices::CDirectivityTF> GetDirectivityTF() override
void RemoveDirectivityTF() override

void SetDirectivityEnable(bool _enabled) override

```


</details>