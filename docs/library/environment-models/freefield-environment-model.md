# Free Field Environment Model
  
 The **Free Field Environment Model** simulates sound propagation in a free-field environment, focusing on the direct sound path. Specifically, it models the propagation delay, distance-based attenuation, and filtering effects caused by the medium.  

In a free-field environment, sound propagates without any interaction with obstacles or boundaries, such as walls or objects, allowing it to travel unimpeded from the source to the listener. This type of propagation is characterized by the **inverse-square law**, which states that the sound intensity decreases proportionally to the square of the distance from the source. This phenomenon, known as **distance-based attenuation**, plays a crucial role in how we perceive the loudness of sounds in open spaces.

Additionally, sound waves traveling through a medium, such as air, are subject to **frequency-dependent filtering**. Higher frequencies are more susceptible to attenuation due to energy absorption by the medium, which results in a natural filtering effect as the distance increases. This filtering can significantly impact the timbre of sounds heard at greater distances.

The model also accounts for **propagation delay**, which represents the time it takes for sound to travel from the source to the listener. This delay is determined by the speed of sound in the medium, which is approximately 343 m/s in air under standard atmospheric conditions. By accurately simulating these factors, the Free Field Environment Model provides a realistic representation of sound propagation in open environments.

## Functional overview

The **Free Field Environment Model** simulates sound propagation in open spaces. It processes monaural input signals connected from source models and generates virtual monaural output sources that are connected to listener models. For each source connected to the model, a corresponding virtual output source is created to represent the processed signal. The user only needs to specify the listener model to which this environment model is connected, after which sources can be freely added or removed as needed.  

### Signal Processing Chain  

The model’s signal processing involves three main stages. First, the input signal is attenuated based on the distance between the source and the listener, replicating the natural reduction in sound intensity over distance. Although not yet implemented, a filtering stage will eventually simulate the frequency-dependent attenuation effects caused by propagation through air. Finally, the signal passes through a delay line that accurately models the propagation delay corresponding to the distance between the source and listener. This sequence ensures that the output signals reflect realistic free-field sound propagation.  

### Dynamic Adaptation  

The delay line is dynamically updated to account for changes in the relative positions of the source and the listener, maintaining accurate modeling of propagation effects in real-time. This dynamic behavior makes the Free Field Environment Model a vital component for simulating direct sound paths in spatial audio systems.  


## Architecture

The internal block diagram of this class is as follows:
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="Free field environment Model Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Free field environment Model Internal diagram.</p>
</div>

## Configuration Options

This model allows configuration by calling its methods or by BRT internal commands:

- **Model (on/off)**: Silent when off.
- **Gain (float)**: Extra gain to be applied to the model output.
- **Distance Attenuation (on/off)**: No attenuation is applied when disabled.
- **Propagation Delay (on/off)**: No delay is applied when disabled.

## Connections
Modules to which it supports connections: 

    - Source models

Modules to which it connects:

    - Listener models    


<details>
<summary>For C++ developer</summary>

<ul>
<li><strong>File</strong>: /include/EnvironmentModels/FreeFieldEnvironmentModel.hpp</li>
<li><strong>Class name</strong>: CFreeFieldEnvironmentModel</li>
<li><strong>Inheritance</strong>: CEnviromentModelBase</li>
<li><strong>Namespace</strong>: BRTEnvironmentModel</li>
<li><strong>Classes that instance</strong>:
    <ul>
        <li>BRTEnvironmentModel::CFreeFieldEnvironmentProcessor</li>        
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
std::shared_ptr<BRTEnvironmentModel::CFreeFieldEnvironmentModel> environmentModel = brtManager.CreateEnvironment<BRTEnvironmentModel::CFreeFieldEnvironmentModel>(_environmentID);
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

void EnableDistanceAttenuation() override
void DisableDistanceAttenuation() override
bool IsDistanceAttenuationEnabled() override

void EnablePropagationDelay() override
void DisablePropagationDelay() override
bool IsPropagationDelayEnabled() override

bool ConnectSoundSource(std::shared_ptr<BRTSourceModel::CSourceModelBase> _source) override
bool ConnectSoundSource(const std::string & _sourceID) override

bool DisconnectSoundSource(std::shared_ptr<BRTSourceModel::CSourceModelBase> _source) override
bool DisconnectSoundSource(const std::string & _sourceID) override

void ResetProcessorBuffers()

void UpdateCommand() override
```


</details>