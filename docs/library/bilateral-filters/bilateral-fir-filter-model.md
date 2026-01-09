# Bilateral FIR Filter Model
<div style="overflow: auto;">
  <span style="font-size: 0.8em; color: green; font-style: italic; float: left;">Available from BRT v2.5.0</span>  
  <span style="font-size: 0.8em; color: grey; font-style: italic; float: right; margin-right: 15px;">Last update: 9 Jan 2026</span>
</div>

## Overview

The **Bilateral FIR Filter Model** (class `BRTBilateralFilter::CFIRBilateralFilterModel`, defined in
`FIRBilateralFilterModel.hpp`) implements **binaural filtering through frequency-domain convolution**
using **finite impulse responses (FIR)**. Each ear is processed independently by convolving the input signal with a fixed impulse response
defined in the frequency domain. 

This approach enables applications such as **headphone compensation**,
**device equalization**, or other forms of binaural transfer function processing.

Impulse responses are provided by the dedicated service module
[`GeneralFIR`](../service-modules/service-general-fir.md), typically loaded from **SOFA** files.

---

## Architecture

The internal block diagram of this class is as follows:
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="Bilateral SOS Filter Model Internal Block diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Bilateral SOS Filter Model Internal Block diagram.</p>
</div>

### Inheritance
- `CFIRBilateralFilterModel` derives from `CBilateralFilterModelBase`, which itself inherits from `BRTBase::CModelBase`.
- The model is integrated into the processing graph through the `BRTBase::CBRTManager`.

### Internal components

The main internal elements of the model are:

- `BRTServices::CGeneralFIR` : Service that stores and provides the impulse responses (IRs) in the frequency domain.
- `BRTFilters::CFIRFilter` (Frequency-domain FIR filter processor): Performs FFT-based convolution independently for the left and right ears.
- `Common::CAudioMixer` (base class): Mixes multiple incoming audio streams per ear before convolution is applied.

---

# Configuration Options
The model allows confcan be enabled and disabled dynamically:

- **Enable/Disable Model** :   If disabled, audio samples pass through unchanged.
- **Gain (float)**: Extra gain to be applied to the model output
- **Impulse Responses to be used** : The General FIR service module to be used for rendering. The system supports dynamic, hot-swapping of the service module being used.

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
<li><strong>File</strong>: /include/BilateralFilterModels/FIRBilateralFilterModel.hpp</li>
<li><strong>Class name</strong>: CFIRBilateralFilterModel</li>
<li><strong>Inheritance</strong>: CBilateralFilterModelBase</li>
<li><strong>Namespace</strong>: BRTBilateralFilter</li>
<li><strong>Classes that instance</strong>:
    <ul>
        <li>BRTFilters::CFIRFilter</li>        
    </ul>
</li>
</ul> 

<h2>Class inheritance diagram</h2>
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="FIR Bilateral Filter Model Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">FIR Bilateral Filter Model Internal diagram.</p>
</div>
<br>

<h2>How to instantiate</h2>

```cpp
brtManager.BeginSetup();
std::shared_ptr<BRTBilateralFilter::CFIRBilateralFilterModel> binauralFilter = brtManager.CreateBinauralFilter<BRTBilateralFilter::CFIRBilateralFilterModel>(_binauralFilterID);
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

bool SetFIRTable(std::shared_ptr<BRTServices::CGeneralFIR> _firTable)
std::shared_ptr<BRTServices::CGeneralFIR> GetFIRTable() const 
void RemoveFIRTable()

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