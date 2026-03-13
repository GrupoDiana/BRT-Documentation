# Spherical SOS Table

## Overview
_SphericalSOSTable_ is a Service Module designed to store **second-order section (SOS) filter coefficients** within the BRT architecture. These coefficients represent parametric filter structures that may either remain constant or vary spatially. The module is particularly useful for representing direction-dependent filters such as **near-field compensation filters** or other parametric acoustic corrections. By organizing SOS filters in a spatial structure, the module enables efficient retrieval of the filter corresponding to a given listener–source direction.

## Role in the Architecture
Within the BRT architecture, _SphericalSOSTable_ acts as a **resource container** accessed by Processing Models during runtime. Processing Models query the module to obtain the appropriate SOS filter coefficients based on the current spatial configuration. The data stored in the module is typically loaded by **Readers**, which parse external formats and convert them into the internal spatial organization used by BRT. This separation allows resource management to remain independent from the signal processing algorithms.

## Data Organization
The data is organized as a **collection of spherical tables, one for each distance**. Each distance defines a *distance bucket*, representing a measurement sphere centered at the listener. Within each bucket, filters are indexed by **azimuth and elevation**, forming a regular directional grid. This structure allows the module to manage datasets containing measurements taken at multiple source–listener distances. During rendering, the system selects the **closest available sphere** to the requested source distance, preserving the physical meaning of distance-dependent measurements while maintaining efficient runtime access.

Inside each distance bucket, the IIR filters are organized according to their **azimuth and elevation coordinates**. A **KD-tree spatial structure** is used to perform efficient nearest-neighbor searches within the table. This allows the system to retrieve the closest available measurement direction whenever an exact directional match is not present.

Although this is the primary design, the structure is flexible enough to support other usage patterns. The module can also store **direction-independent filters** by inserting coefficients with azimuth, elevation, and distance set to zero. In this case, a single filter per ear can be stored and retrieved without any spatial dependency.

Another example of this flexibility can be seen in our implementation of the **near-field effect filters**. In this instance, the coefficients are indexed by interaural azimuth, with elevation fixed at zero. This approach exploits the symmetry of the acoustic response with respect to the interaural axis. Further details about this configuration can be found in the *Near-Field* [section](../processing-modules/nearfield-effect-processor.md).

### Data store hierarchy

```mermaid
graph LR

B[Distance Sphere]
B --> C[Direction: Azimuth / Elevation]
C --> D[IIR coefficients of the left ear]
C --> E[IIR coefficients of the right ear]
```

## Supported Data Types
The module stores **parametric filter representations** expressed as cascades of second-order sections. Each entry typically contains the numerator and denominator coefficients of the SOS structure. This representation is commonly used for efficient and numerically stable implementations of IIR filters. Both **single filters** and **spatially varying collections of filters** can be represented within the same framework.

## Typical Use Cases
A couple of common use cases might be: storing **near-field compensation filters** that vary depending on the distance between the sound source and the listener. The processing models responsible for near-field rendering can query the module to obtain the appropriate SOS filter for the current source direction. Another use of this module is to store filters for simulating direction-independent filters, such as protective headphones or a very simple equalisation device. 

## Related Service Modules
**SphericalFIRTable**

_SphericalFIRTable_ stores **impulse responses (FIR filters)** organized spatially, typically representing HRTFs or BRIRs. In contrast, _SphericalSOSTable_ stores **parametric IIR filters expressed as SOS coefficients**, which are generally more compact and suitable for certain acoustic corrections. 

**SphericalInterpolatedFIRTable**

 _SphericalInterpolatedFIRTable_ performs interpolation between spatial measurements, _SphericalSOSTable_ typically retrieves the filter associated with the closest available spatial position. These modules therefore address different filter representations and rendering needs.

## Summary
_SphericalSOSTable_ provides a structured way to store and access **spatially organized parametric filters** within BRT. By representing filters as SOS coefficients and organizing them over spherical coordinates, the module supports efficient retrieval of direction-dependent filtering resources. It complements FIR-based resource modules and enables Processing Models to integrate parametric acoustic corrections into binaural rendering pipelines.

## For C++ developer
<details>
<summary>For C++ developer</summary>

<ul>
<li><strong>File</strong>: /include/ServiceModules/SphericalSOSTable.hpp</li>
<li><strong>Class name</strong>: CSphericalSOSTable</li>
<li><strong>Inheritance</strong>: CServicesBase</li>
<li><strong>Namespace</strong>: BRTServices</li>
</ul> 

<h2>Class inheritance diagram</h2>
```mermaid
classDiagram
direction TB

class CServicesBase 
    <<interface>> CServicesBase
class CSphericalFIRTable
class CSphericalInterpolatedFIRTable
class CSphericalSOSTable
class CAmbisonicBIR

CServicesBase <|-- CSphericalFIRTable
CServicesBase <|-- CSphericalInterpolatedFIRTable
CServicesBase <|-- CSphericalSOSTable
CServicesBase <|-- CAmbisonicBIR
```

<h2>How to instantiate and load</h2>
```cpp
// Assuming SOFA_FILEPATH contains the SOFA filename including the path
std::shared_ptr<BRTServices::CSphericalSOSTable> sosFilter = std::make_shared<BRTServices::CSphericalSOSTable>();
bool sofaLoaded = LoadSofaFile(SOFA_FILEPATH, sosFilter);        
    if (!sofaLoaded) {
        // ERROR
    }
```

<h2>How to connect it to a listener model </h2>
```cpp
// Assuming that the ID of this listener is contained in _listenerID and 
// that the HRTF is already lsuccessfuly loaded into hrtf.
std::shared_ptr<BRTBase::CListener> listener = brtManager->GetListener(listenerID);
listener->SetNearFieldCompensationFilters(hrtf);
```

<h2>How to connect it to a bilateral filter by the listener ID</h2>
```cpp
std::shared_ptr<BRTBase::CListener> listener = brtManager->GetListener(listenerID);
listener->SetBilateralSOSFilter(filter);
```

<h2>How to connect it directly to a bilateral filter</h2>
```cpp
std::shared_ptr<BBRTBilateralFilter::CBilateralFilterModelBase> filter = brtManager->GetBilateralFilter(bilateralFilterID);
filter->SetSOSFilterCoefficients(filter);
```

<h2>Public Methods of <code>CSphericalSOSTable</code></h2>

<table>
<thead>
<tr>
<th>Category</th>
<th>Method</th>
<th>Description</th>
</tr>
</thead>

<tbody>

<tr>
<td>Constructor</td>
<td><code>CSphericalSOSTable()</code></td>
<td>Creates an empty spherical SOS table service.</td>
</tr>

<tr>
<td>Ear Geometry</td>
<td><code>void SetEarPosition(Common::T_ear _ear, Common::CVector3 _earPosition)</code></td>
<td>Sets the local position of the specified ear relative to the listener head center.</td>
</tr>

<tr>
<td rowspan="3">SOS Table Setup</td>
<td><code>bool BeginSetup()</code></td>
<td>Initializes the SOS table setup process and clears previously loaded data.</td>
</tr>
<tr>
<td><code>void AddCoefficients(float _azimuth, float _elevation, float _distance, Common::CEarPair&lt;CMonoBuffer&lt;float&gt;&gt; &amp;&amp; newCoefs)</code></td>
<td>Adds a new set of SOS coefficients for a given azimuth, elevation, and distance.</td>
</tr>
<tr>
<td><code>bool EndSetup()</code></td>
<td>Finalizes the SOS table, sorts distance buckets, builds search trees, and marks the data as ready.</td>
</tr>

<tr>
<td rowspan="2">SOS Retrieval</td>
<td><code>const std::vector&lt;float&gt; GetSOSCoefficients_SpatiallyOriented(float _azimuth, float _elevation, float _distance, Common::T_ear ear) const</code></td>
<td>Returns the SOS coefficients for one ear using a spatial query defined by azimuth, elevation, and distance.</td>
</tr>
<tr>
<td><code>const Common::CEarPair&lt;CMonoBuffer&lt;float&gt;&gt; GetSOSCoefficients_2Ears() const</code></td>
<td>Returns the SOS coefficients for both ears when the table is not spatially oriented.</td>
</tr>

</tbody>
</table>

</details>