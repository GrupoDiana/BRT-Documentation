# Bilateral SOS Filter Model
<div style="overflow: auto;">
  <span style="font-size: 0.8em; color: green; font-style: italic; float: left;">Available from BRT v2.5.0</span>  
  <span style="font-size: 0.8em; color: grey; font-style: italic; float: right; margin-right: 15px;">Last update: 9 Jan 2026</span>
</div>

## Overview

The **Bilateral SOS Filter Model** (class `BRTBilateralFilter::CSOSBilateralFilterModel`, defined in `SOSBilateralFilterModel.hpp`) is a binaural filtering model that applies an **independent chain of second-order sections (SOS / biquads, IIR)** to each ear. 

This approach is well suited for simulating devices or effects that are naturally modeled using IIR filters, such as **hearing protection devices** or other ear-dependent equalization systems.

The SOS coefficients are provided through the dedicated service module
[`SOSCoefficients`](../service-modules/service-sos-coefficients.md), typically loaded from a **SOFA** file.


## Architecture

The internal block diagram of this class is as follows:
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="Bilateral SOS Filter Model Internal Block diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Bilateral SOS Filter Model Internal Block diagram.</p>
</div>

### Inheritance
- `CSOSBilateralFilterModel` derives from `CBilateralFilterModelBase`, which itself inherits from `BRTBase::CModelBase`.
- The model is integrated into the processing graph through the `BRTBase::CBRTManager`.

### Internal components
The main internal elements of the model are:

- `BRTServices::CSOSCoefficients`: Service that stores and provides SOS coefficient sets.
- `BRTFilters::CSOSFilter`: IIR filter implementation that processes the SOS chain for each ear.
- `Common::CAudioMixer`: (base class) Used to mix multiple incoming audio streams per ear before filtering.

---

## Configuration Options
The model allows confcan be enabled and disabled dynamically:

- **Enable/Disable Model** :   If disabled, audio samples pass through unchanged.
- **Gain (float)**: Extra gain to be applied to the model output
- **SOS Filter Coefficients to be used** : The SOS coefficients service module to be used for rendering. The system supports dynamic, hot-swapping of the service module being used.

---

## Connections
Modules to which it supports connections: 

    - Listener models    

Modules to which it connects:

    - Listener
    
---

## For C++ developers

<details>
<summary>For C++ developer</summary>
<ul>
<li><strong>File</strong>: /include/BilateralFilterModels/SOSBilateralFilterModel.hpp</li>
<li><strong>Class name</strong>: CSOSBilateralFilterModel</li>
<li><strong>Inheritance</strong>: CBilateralFilterModelBase</li>
<li><strong>Namespace</strong>: BRTBilateralFilter</li>
<li><strong>Classes that instance</strong>:
    <ul>
        <li>BRTFilters::CSOSFilter</li>        
    </ul>
</li>
</ul> 

<h2>Class inheritance diagram</h2>
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="SOS Bilateral Filter Model Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">SOS Bilateral Filter Model Internal diagram.</p>
</div>
<br>

<h2>How to instantiate</h2>

```cpp
brtManager.BeginSetup();
std::shared_ptr<BRTBilateralFilter::CSOSBilateralFilterModel> binauralFilter = brtManager.CreateBinauralFilter<BRTBilateralFilter::CSOSBilateralFilterModel>(_binauralFilterID);
brtManager.EndSetup();
if (binauralFilter == nullptr) {
    // ERROR
}
```

<h2>How to connect</h2>
Connect it to a listener.

```cpp
std::shared_ptr<BRTBase::CListener> listener = brtManager.GetListener(_listenerID);
if (listener != nullptr) {
    brtManager.BeginSetup();
    bool control = listener->ConnectBinauralFilter(_binauralFilterID);
    brtManager.EndSetup();
}
```

Connect an listener model to it.
```cpp

std::shared_ptr<BRTBilateralFilter::CBilateralFilterModelBase> binauralFilter = brtManager.GetBinauralFilter<BRTBilateralFilter::CBilateralFilterModelBase>(_binauralFilterD);
if (binauralFilter != nullptr) {
    brtManager.BeginSetup();
    bool control = binauralFilter->ConnectListenerModel(_listenerModelID);
    brtManager.EndSetup();
}
```

<h2>Public methods</h2>

```cpp
void EnableModel() 
void DisableModel() 
bool IsModelEnabled() 

void SetGain(float _gain)
float GetGain()

bool SetSOSFilterCoefficients(std::shared_ptr<BRTServices::CSOSCoefficients> _listenerILD)
std::shared_ptr<BRTServices::CSOSCoefficients> GetSOSFilter() const
void RemoveSOSFilter()

bool ConnectListenerModel(const std::string & _listenerModelID, Common::T_ear _ear = Common::T_ear::BOTH)
bool ConnectListenerModel(std::shared_ptr<BRTListenerModel::CListenerModelBase> _listenerModel, Common::T_ear _ear = Common::T_ear::BOTH)

bool DisconnectListenerModel(const std::string & _listenerModelID, Common::T_ear _ear = Common::T_ear::BOTH)
bool DisconnectListenerModel(std::shared_ptr<BRTListenerModel::CListenerModelBase> _listenerModel, Common::T_ear _ear = Common::T_ear::BOTH)

T_BilateralFilterType GeBilateralFilterType() const 

bool IsConnectedToListener()
bool IsConnectedToListenerModel()
void ConnectionToListenerEstablished(const std::string & _listenerID) 

std::vector<std::string> GetInputs() { return inputConnections; }
std::vector<std::string> GetOutputs() { return outputConnections; }
```
</details>
