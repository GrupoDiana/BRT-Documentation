# Listener Acoustic model based on HRTF convolution in the Ambisonics domain

:warning:*(In progress)*:warning:

The **Listener ....l** bla bla.




## Architecture

The internal block diagram of this class is as follows:
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="Listener HRTF Model Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Listener HRTF Model Internal diagram.</p>
</div>

The operation of the convolucionator as well as its block diagram can be seen in this link (:warning:URL).

## Configuration Options

This model allows configuration by calling its methods or by OSC:

- **Model (on/off)**: Silent when off.
- blabla

## Connections
Modules to which it supports connections: 

    - Bla models
    - blat models

Modules to which it connects:

    - Bla
    - Bla


<details>
<summary>For C++ developer</summary>

<ul>
<li><strong>File</strong>: \include\ListenerModels\ListenerAmbisonicHRTFModel.hpp</li>
<li><strong>Class name</strong>: CListenerAmbisonicHRTFModel</li>
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
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="Listener HRTF Model Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Listener HRTF Model Internal diagram.</p>
</div>
<br>

<h2>How to instantiate</h2>


```cpp
// Assuming that the ID of this listener model is contained in _listenerModelID.
brtManager.BeginSetup();
std::shared_ptr<BRTListenerModel::CListenerAmbisonicHRTFModel>listenerModel = brtManager.CreateListenerModel<BRTListenerModel::CListenerAmbisonicHRTFModel>(_listenerModelID);
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
bool SetHRTF(std::shared_ptr< BRTServices::CHRTF > _listenerHRTF) override
std::shared_ptr<BRTServices::CHRTF> GetHRTF() const override
void RemoveHRTF() override

bool SetNearFieldCompensationFilters(std::shared_ptr<BRTServices::CSOSFilters> _listenerILD) override
std::shared_ptr<BRTServices::CSOSFilters> GetNearFieldCompensationFilters() const override
void RemoveNearFierldCompensationFilters() override

bool SetAmbisonicOrder(int _ambisonicOrder) override
int GetAmbisonicOrder() override 
bool SetAmbisonicNormalization(Common::TAmbisonicNormalization _ambisonicNormalization) override 

bool SetAmbisonicNormalization(std::string _ambisonicNormalization) override 
Common::TAmbisonicNormalization GetAmbisonicNormalization() override 

void EnableNearFieldEffect() override
void DisableNearFieldEffect() override
bool IsNearFieldEffectEnabled() override

void EnableITDSimulation() override
void DisableITDSimulation() override
bool IsITDSimulationEnabled() override

void EnableParallaxCorrection() override
void DisableParallaxCorrection() override 
bool IsParallaxCorrectionEnabled() override

void EnableModel() override 
void DisableModel() override

void ResetProcessorBuffers()

bool ConnectSoundSource(std::shared_ptr<BRTSourceModel::CSourceModelBase> _source) override
bool ConnectSoundSource(const std::string & _sourceID) override
bool DisconnectSoundSource(std::shared_ptr<BRTSourceModel::CSourceModelBase> _source) override
bool DisconnectSoundSource(const std::string & _sourceID) override 

bool ConnectEnvironmentModel(const std::string & _environmentModelID) override 
bool DisconnectEnvironmentModel(const std::string & _environmentModelID) override

void UpdateCommand() override
```


</details>






