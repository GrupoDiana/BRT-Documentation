# Omnidirectional Source Model  
:warning:*(Ready for review)*:warning:

The **Omnidirectional Source Model** serves as a straightforward interface for applications to interact with the BRT Library during the rendering process. This model is characterized by its simplicity, as it primarily passes the source's position and the audio samples provided by the application to the connected modules. These modules can include listener models and environment models. Applications must supply monaural audio samples for each frame and update the source's position and orientation when necessary.  

## Functional Overview  

To use the omnidirectional source model, it must first be instantiated and connected to the desired modules, such as listener or environment models. Once connected, the application is responsible for managing the source's position and orientation, which can be updated dynamically as needed using the method ``SetSourceTransform`. During each audio frame, the application must also provide the corresponding monaural audio samples, usind the `SetBuffer` method. 

## Connections

Modules to which it connects:

    - Environment models
    - Listener models    

<details>
<summary>For C++ developer</summary>

<ul>
<li><strong>File</strong>: /include/SourceModels/SourceSimpleModel.hpp</li>
<li><strong>Class name</strong>: CSourceSimpleModel</li>
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
std::shared_ptr<BRTSourceModel::CSourceSimpleModel> brtSoundSource = brtManager->CreateSoundSource<BRTSourceModel::CSourceSimpleModel>(soundSourceID);
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
```


</details>