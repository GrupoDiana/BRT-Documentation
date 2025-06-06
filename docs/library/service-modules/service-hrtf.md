# HRTF Service Module

The HRTF service module is responsible for handling the reading, storage, and processing of HRTFs. These functions are essential for enabling the listener model to render spatial audio [(read more)](../listener-models/hrtf-models/listener-acoustic-model-hrtf.md). This module optimizes the accessibility and organization of the HRTF data, ensuring efficient processing and integration within the overall rendering pipeline.

An HRTF consists of a collection of HRIRs, each associated with specific measurement directions, including azimuth (left-right) and elevation (up-down) and distance. The BRT is designed to handle arbitrary distributions of HRIR measurement directions, meaning it does not rely on a regular or complete directional grid and imposes no minimum density requirements. The HRTF Service Module is in charge of estimating HRIRs for the exact direction and distance of the source if these are not explicitly included in the HRTF data. For this estimation, the algorithm selects the three nearest points at which the HRTF was measured, and performs a barycentric interpolation among the HRIRs corresponding to these three locations. In this way, this module stores the HRTF in the form of a regular grid, enabling more efficient use of resources during binaural rendering. For more details about this grid, click [here](../../assets/technical-report/SONICOM_TR3.1_BRT%20REGULAR%20GRID%20DISTRIBUTION%20OF%20POINTS%20IN%20THE%20SPHERE%20USED%20BY%20THE%20BRT.pdf). 


Interpolating between HRIRs with varying ITDs can lead to issues, such as audible artifacts and reduced rendering quality. To address this, the HRTF Service Module handles ITDs independently from the interpolation and convolution processes. To do so, user-imported HRIRs should be provided with ITDs stored separately. The ITDs to be added after interpolation can be either estimated by interpolating among those corresponding to the three closest HRIRs, or synthesised using data about the location of the sound source (specifically the interaural azimuth) and the head circumference of the listener.

Each HRIR is partitioned in chunks to match the input buffer length. This is done in order to use the  [UPOLS convolution](../processing-modules/uniform-partitioned-convolution.md). In addition, an FFT is applied to each of the HRIR partitions and stored in memory by the Service Module, since the convolution is done in the frequency domain.

The following diagram illustrates the processing performed on the HRTF table before it is stored. This processing is performed offline, ensuring that, in real-time, we have a regular table that is faster to access.

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/HRTF_offlineProcess.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">HRTF offline process.</p>
</div>

<!--
## Architecture

<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="HRTF offline process" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">HRTF class diagram.</p>
</div>
-->

## Functional Overview

The methods in the **HRTF class** are designed to facilitate the loading and management of HRTFs within the renderer. This service allows loading multiple HRTFs as needed, enabling flexible spatial audio configurations. To load an HRTF file, a separate [Reader](../readers/index.md) class is required to parse the file and add HRIRs to the HRTF class one by one. The process begins by calling the **Begin Setup** method to initialize the configuration. HRIRs are then added incrementally using the **Add HRIR** method. Once all the data has been loaded, the **End Setup** method finalizes the setup, generating a complete HRTF table for use. Additionally, the class provides various configuration options, such as setting the sampling rate, adjusting the angular grid resolution, enabling the Woodworth ITD model, and defining head and ear properties, as detailed in the methods below. These features ensure precise and efficient integration of HRTFs into the rendering pipeline.


## Configuration Options

The methods provided by this service are as follows.

- **Begin Setup**: Prepares the HRTF object for initializing and starts the configuration.
- **End Setup**: Finalizes the setup process and store in the configured HRTF data.
- **Set Sampling Rate**:  Specifies the sampling rate for the HRTF data
- **Add HRIR**: Adds a Head-Related Impulse Response (HRIR) to the HRTF dataset for a specific position.
- **Set/Get file name**: Assigns or retrieves the file name of the HRTF data to be used.
- **Set/Get sampling step for the grid**: Defines or retrieves the angular step size used to sample the HRTF grid.
- **Enable/Disable Woodworth ITD**: Toggles the application of the Woodworth ITD formula for HRTF processing.
- **Get HRIR Partitioned**: Retrieves  the interpolated and partitioned stored HRTF for an specific direction.
- **Get HRIR Delay**: Calculates and returns the delay associated with the HRIR for a specific position.
- **Set/Get Head Radius**: Configures or retrieves the radius of the listener's head model.
- **Set ear position**: Sets the position of the ears relative to the head model.

<details>
<summary>For C++ developer</summary>

<ul>
<li><strong>File</strong>: /include/LServiceModules/HRTF.hpp</li>
<li><strong>Class name</strong>: CHRTF</li>
<li><strong>Inheritance</strong>: CServicesBase</li>
<li><strong>Namespace</strong>: BRTServices</li>
</ul> 

<h2>Class inheritance diagram</h2>
<div style="border: 1px solid #000; padding: 10px; display: inline-block;">
    <img src="/BRT-Documentation/assets/sysmldiagrams/none.png" alt="HRTF Class diagram" style="display: block; margin: 0 auto;">
    <p style="text-align: center;">HRTF Class diagram.</p>
</div>
<br>

<h2>How to instantiate and load</h2>


```cpp
// Assuming SOFA_FILEPATH contains the SOFA filename including the path
std::shared_ptr<BRTServices::CHRTF> hrtf = std::make_shared<BRTServices::CHRTF>();
bool hrtfSofaLoaded = AppUtils::LoadSofaFile(SOFA_FILEPATH, hrtf);        
    if (!hrtfSofaLoaded) {
        // ERROR
    }
```

<h2>How to connect it to a listener</h2>

```cpp
// Assuming that the ID of this listener is contained in _listenerID and 
// that the HRTF is already lsuccessfuly loaded into hrtf.
std::shared_ptr<BRTBase::CListener> listener = brtManager->GetListener(listenerID);
listener->SetHRTF(hrtf);
```


<h2>Public methods</h2>

```cpp
int32_t GetHRIRLength() const
void SetGridSamplingStep(int _samplingStep) 
int GetGridSamplingStep() 

bool BeginSetup(int32_t _HRIRLength, BRTServices::TEXTRAPOLATION_METHOD _extrapolationMethod) override 
void AddHRTFTable(T_HRTFTable&& newTable)
void AddHRIR(double _azimuth, double _elevation, double _distance, Common::CVector3 listenerPosition, THRIRStruct&& newHRIR) override 
bool EndSetup() override 

void EnableWoodworthITD() 
void DisableWoodworthITD() 
bool IsWoodworthITDEnabled() 

const std::vector<CMonoBuffer<float>> GetHRIRPartitioned(Common::T_ear ear, float _azimuth, float _elevation, bool runTimeInterpolation, const Common::CTransform& /* _listenerLocation*/ ) const override
THRIRPartitionedStruct GetHRIRDelay(Common::T_ear ear, float _azimuthCenter, float _elevationCenter, bool runTimeInterpolation,	Common::CTransform& _listenerLocation) 
const int32_t GetHRIRNumberOfSubfilters() const 
const int32_t GetHRIRSubfilterLength() const override 
bool IsHRTFLoaded()
float GetHRTFDistanceOfMeasurement() override 

void SetTitle(std::string _title) override 
void SetDatabaseName(std::string _databaseName) override 
void SetListenerShortName(std::string _listenerShortName) override 

void SetFilename(std::string _fileName) override 
std::string GetFilename() override 

void SetHeadRadius(float _headRadius) override
float GetHeadRadius() override 

void SetEarPosition(Common::T_ear _ear, Common::CVector3 _earPosition) override 
void SetCranialGeometryAsDefault() override 
Common::CVector3 GetEarLocalPosition(Common::T_ear _ear) 

void SetSamplingRate(int _samplingRate) 

```

</details>



