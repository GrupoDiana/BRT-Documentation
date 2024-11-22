# Listener Acoustic Model based on HRTF direct convolution

:warning:*(In progress)*:warning:

The **Listener HRTF model** module of the library allows the rendering of spatial audio from multiple sound sources. It simulates the direct path between source and listener by taking advantage of *direct convolution* with head impulse responses (HRTF). This module independently processes each of the input sources, taking into account their positions and the position and orientation of the listener.

Subsequently, this model performs a *near-field correction*, independent for each of the sources. This correction is made by filtering the signals corresponding to each ear. The coefficients of these filters are dependent on the distance between source and listener and the interaural azimuth.

Finally, the model mixes all channels, left and right separately, to give a single output per ear.

The convolutions are performed in the frequency domain using the uniformly partitioned convolution algorithm. Filtering is performed in the time domain.
The HRTF service module is responsible for providing the impulse responses in each frame. While the SOS filter module is in charge of providing the coefficients _. Both classes must have been previously configured, read more in the services section.


## Architecture

The internal block diagram of this class is as follows:
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/ListenerHRTFModel_InternalBlockDiagram.png" alt="Listener HRTF Model Internal diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">Listener HRTF Model Internal diagram.</p>
</div>

The operation of the convolucionator as well as its block diagram can be seen in this link (:warning:URL).

## Configuration Options

This model allows configuration by calling its methods or by OSC:

- **Model (on/off)**: Silent when off.
- **Spatialization (on/off)**: Transparent when off.
- **Interpolation (on/off)**: When switched on, HRIRs and delays are calculated at the exact position (relative source-listener position). For this purpose, barycentric interpolation is performed, starting from the three closest points. When it is switched off, the HRIR and delay with the closest position are chosen.
- **NearFieldEffect (on/off)**: 
- **ITD Simulation (on/off)**: When activated, a separate delay is added to each ear to simulate the interaural time difference[^1]. This delay is provided by the HRTF service module and may be provided in the SOFA structure or will be calculated from the head size, for more information see (:warning:URL). When off, it does not simulate interaural time difference. 
- **Parallax Correction (on/off)**
- **HRTF to be used (on/off)**
- **Nearfield filter (SOS filter) to be used (onf/off)**: 

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
<li><strong>File</strong>: \include\ListenerModels\ListenerHRTFModel.hpp</li>
<li>Class name: CListenerHRTFModel</li>
<li>Inheritance: CListenerModelBase</li>
<li>Classes that instance:</li>
</ul> 

<h2>Class inheritance diagram</h2>

<br>
<h2>Public methods</h2>

```cpp

// SET HRTF of listener
bool SetHRTF(std::shared_ptr< BRTServices::CHRTF > _listenerHRTF) override

std::shared_ptr<BRTServices::CHRTF> GetHRTF() const override
void RemoveHRTF() override
bool SetNearFieldCompensationFilters(std::shared_ptr<BRTServices::CSOSFilters> _listenerILD) override

/** \brief Get HRTF of listener
*	\retval HRTF pointer to current listener HRTF
*   \eh On error, an error code is reported to the error handler.
*/
std::shared_ptr<BRTServices::CSOSFilters> GetNearFieldCompensationFilters() const override

```
<h2>How to instantiate</h2>

<h2>How to connect</h2>

</details>






