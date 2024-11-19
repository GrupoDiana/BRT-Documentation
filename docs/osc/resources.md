<!----------------------------------------------------------------------------------->

## `/resources/loadHRTF`

Loads a new HRTF in a SOFA file from the specified path and assign an identifier to it. If there is already another HRTF with the same identifier, it is substituted by the new one. The SOFA file must use FIR or FIR-E as data type but can follow any convention using these data types. When loading, the HRTF is spatially resampled to fit the [HRTF grids](). This way the real time processor can get fast enough the three HRIRs to be interpolated and convolved.

#### Syntax

`/resources/loadHRTF <string HRTF_id> <string HRTF_SOFAFile_path> <float spatialResolution>`

`HRTF_id`: Identifier to be assigned to the HRTF for later references to it. If there is already another HRTF with the same identifier, it is substituted by the new one. Otherwise, a new HRTF is created.

`HRTF_SOFAFile_path`: Specifies a SOFA file containing an HRTF. it can be either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable.  

`spatialResolution`: This parameter indicates the interpolation step in the horizontal plane of zero elevation. To learn more about this grid and the interpolation mechanism, please refer to the [HRTF grids]().

#### Return

A return message is sent back to the sender `control/actionResult /resources/loadHRTF <string HRTF_id> <boolean loaded> <string description>`, indicating `loaded=true`if the HTYF is successfuly loaded and `loaded=false` if not. In both cases a `description` is added to give more details. 

In case of success, it sends an echo to all subscribers excepting the sender: `/resources/loadHRTF <string HRTF_id> <string HRTF_SOFAFile_path> <float spatialResolution>`

#### Example

BeRTA receives: `/resources/loadHRTF HRTF1 c:/tmp/hrtf.sofa 5`

BeRTA sends back to the sender: `control actionResult /resources/loadHRTF HRTF1 true HRTF HRTF1 loaded successfully, with a spatial resolution of 5, from the file: c:/tmp/hrtf.sofa` or `control actionResult /resources/loadHRTF HRTF1 false HRTF sofa file couldn't be loaded from HRTF1;`. 

BeRTA sends an echo to all subscribiers excepting the sender: `/resources/loadHRTF HRTF1 c:/tmp/hrtf.sofa 5`

<!----------------------------------------------------------------------------------->
---


## `/resources/removeHRTF`

Removes an HRTF from the loaded resources. 

#### Syntax

`/resources/removeHRTF <string HRTF_id>`

`HRTF_id`: Identifier of the HRTF to be removed.

#### Return

A return message is sent back to the sender `control/actionResult /resources/loadHRTF <string HRTF_id> <bool removed> <string description>`, indicating `removed=true`if the HRTF is successfuly removed and `removed=false` if not. In both cases a `description` is added to give more details. 

In case of success, it sends an echo to all subscribers excepting the sender: `/resources/removeHRTF <string HRTF_id>`

#### Example

BeRTA receives: `/resources/removeHRTF HRTF1`

BeRTA sends back to all subscribers: `/resources/removeHRTF HRTF1 true HRTF HRTF1 removed` or just to the sender: `/resources/removeHRTF HRTF1 false error: HRTF not found in the list`

BeRTA sends an echo to all subscribiers excepting the sender: `/resources/removeHRTF HRTF1`
<!----------------------------------------------------------------------------------->
---


## `/resources/getHRTFInfo`

Gets some information about one of the loaded HRTFs. this information consists of the filename and the spatial resolution used to resample it in the [HRTF grid]().
#### Syntax

`/resources/getHRTFInfo <string HRTF_id>`

`HRTF_id`: Identifier of the HRTF of which we are requesting information.

#### Return

`/resources/getHRTFInfo <string HRTF_id> <string HRTF_SOFAFile> <float spatialResolution>`

The return message is sent back to the sender and refers to the `HRTF_id`, indicating the name of the SOFA file (`HRTF_SOFAFile`) from which the HRTF was loaded, as well as the `spatialResolution` used to resample it when loaded. 

#### Example

BeRTA receives: `/resources/getHRTFInfo HRTF1`

BeRTA sends back: `/resources/getHRTFInfo HRTF1 hrtf.sofa 5` 

<!----------------------------------------------------------------------------------->
---


## `/resources/setHRTFHeadRadius`

Sets the head radius to be stored into the HRTF. This value affects: (1) the position of the ears used in the parallax correction when getting HRIRs; and (2) the calculation of the ITD when using the simulation based on the Woodworth formula whcih is activated or deactivated by the `/resources/enableWoodworthITD` command. If this command is not received, the position of receivers in the HRTF SOFA file is used as the default value. See `/resources/restoreHRTFHeadRadius` as a related command.

#### Syntax

`/resources/setHRTFHeadRadius <string HRTF_id> <float headRadius>`

`HRTF_id`: Identifier of the HRTF of which we are setting the new head radius.

`headRadius`: New head radius in meters. Only positives values are accepted.

#### Return

A return message is sent back to the sender `control/actionResult /resources/setHRTFHeadRadius <string HRTF_id> <bool set> <string description>`, indicating `set=true`if the HRTF is successfuly set and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, it sends an echo to all subscribers but the sender: `/resources/setHRTFHeadRadius <string HRTF_id> <float headRadius>`


#### Example

BeRTA receives: `/resources/setHRTFHeadRadius HRTF1 0.09`

BeRTA sends back to the sender: `control/actionResult /resources/setHRTFHeadRadius HRTF1 true success` 

BeRTA sends back to all subscribers but the sender: `/resources/setHRTFHeadRadius HRTF1 0.09` 

<!----------------------------------------------------------------------------------->
---


## `/resources/getHRTFHeadRadius`

Gets the head radius of one of the HRTFs loaded as a resource. 

#### Syntax

`/resources/getHRTFHeadRadius <string HRTF_id>`

`HRTF_id`: Identifier of the HRTF of which we are requesting the head radius.

#### Return

A message is sent back to the sender `/resources/getHRTFHeadRadius <string HRTF_id> <float headRadius>`, indicating the `headRadius` of the HRTF `HRTF_id`.

#### Example

BeRTA receives: `/resources/getHRTFHeadRadius HRTF1`

BeRTA sends back to the sender: `/resources/getHRTFHeadRadius HRTF1 0.09` 

<!----------------------------------------------------------------------------------->
---


## `/resources/restoreHRTFHeadRadius`

Sets the head radius of the HRTF to the one stored in the SOFA file as the position of receivers. This value affects: (1) the position of the ears used in the parallax correction when getting HRIRs; and (2) the calculation of the ITD when using the simulation based on the Woodworth formula whcih is activated or deactivated by the `/resources/enableWoodworthITD` command. 

#### Syntax

`/resources/restoreHRTFHeadRadius <string HRTF_id>`

`HRTF_id`: Identifier of the HRTF for which we are setting the head radius.

#### Return

A return message is sent back to the sender `control/actionResult /resources/restoreHRTFHeadRadius <string HRTF_id> <bool restored> <string description>`, indicating `restored=true`if the HRTF is successfuly set and `restored=false` if not. In both cases a `description` is added to give more details. 

In case of success, it sends an echo to all subscribers but the sender: `/resources/restoreHRTFHeadRadius <string HRTF_id>`


#### Example

BeRTA receives: `/resources/restoreHRTFHeadRadius HRTF1`

BeRTA sends back to the sender: `control/actionResult /resources/restoreHRTFHeadRadius HRTF1 true success` 

BeRTA sends back to all subscribers but the sender: `/resources/restoreHRTFHeadRadius HRTF1`

<!----------------------------------------------------------------------------------->
---


## `/resources/enableWoodworthITD`

Enables or disables the calculation of the ITD based on the Woodworth formula, considering head radius, or it is extracted from the delays in the HRTF SOFA file. The Woodworth formula needs the head radius set by commands `/resources/setHRTFHeadRadius` and `/resources/restoreHRTFHeadRadius`. By default, the use of the Woodworth formula is disabled.
#### Syntax

`/resources/enableWoodworthITD <string HRTF_id> <boolean enable>`

`HRTF_id`: Identifier of the HRTF for which we are setting the head radius.

`enable`: If enable is true (1), the ITD calculation is performed using the Woodworth formula. If enable is false (0), the ITD is simulated from the delays read from the SOFA file.


#### Return

A return message is sent back to the sender `control/actionResult /resources/enableWoodworthITD <string HRTF_id> <bool enabled> <string description>`, indicating `enabled=true`if the HRTF is successfuly set and `enabled=false` if not. In both cases a `description` is added to give more details. 

In case of success, it sends an echo to all subscribers but the sender: `/resources/enableWoodworthITD <string HRTF_id> <boolean enable>`

#### Example

BeRTA receives: `/resources/enableWoodworthITD HRTF1 true`

BeRTA sends back to the sender: `control/actionResult /resources/enableWoodworthITD HRTF1 true success` 

BeRTA sends back to all subscribers but the sender: `/resources/enableWoodworthITD HRTF1 true`

<!----------------------------------------------------------------------------------->
---


## `/resources/loadDirectivityTF`

Loads a new Directivity Transfer Function in a SOFA file from the specified path and assign an identifier to it. If there is already another Directivity with the same identifier, it is substituted by the new one. The SOFA file must use TF as data type with the same number of frequency bins as the frame size configured in BeRTA. When loading, the DirectivityTF is spatially resampled to fit the [BRT grids](). This way the real time processor can get fast enough the three Directivities to be interpolated and convolved.

#### Syntax

`/resources/loadDirectivityTF <string directivityTF_id> <string directivityTF_SOFAFile_path> <float spatialResolution>`

`directivityTF_id`: Identifier to be assigned to the directivityTF for later references to it. If there is already another directivityTF with the same identifier, it is substituted by the new one. Otherwise, a new directivityTF is created.

`directivityTF_SOFAFile_path`: Specifies a SOFA file containing a directivityTF. it can be either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable.  

`spatialResolution`: This parameter indicates the interpolation step in the horizontal plane of zero elevation. To learn more about this grid and the interpolation mechanism, please refer to the [BRT grids]().

#### Return

A return message is sent back to the sender:  `control/actionResult /resources/loadDirectivityTF <string directivityTF_id> <boolean loaded> <string description>`, indicating `loaded=true`if the directivityTF is successfuly loaded and `loaded=false` if not. In both cases a `description` is added to give more details. 

In case of success, it sends an echo to all subscribers excepting the sender:  `/resources/loadDirectivityTF <string directivityTF_id> <string directivityTF_SOFAFile_path> <float spatialResolution>`


#### Example

BeRTA receives: `/resources/loadDirectivityTF DirectivityTF1 c:/tmp/directivityTF.sofa 15`

BeRTA sends back to the sender: `control/actionResult /resources/loadDirectivityTF DirectivityTF1 true Directivity directivityTF1 loaded` or `/resources/loadDirectivityTF DirectivityTF1 false Directivity sofa file couldn't be loaded from c:/tmp/directivityTF.sofa`. 

BeRTA sends an echo to all subscribiers excepting the sender:  `/resources/loadDirectivityTF DirectivityTF1 c:/tmp/directivityTF.sofa 15`

<!----------------------------------------------------------------------------------->
---


## `/resources/removeDirectivityTF`

Removes a Directivity Transfer Function from the loaded resources. 

#### Syntax

`/resources/removeDirectivityTF <string directivityTF_id>`

`directivityTF_id`: Identifier of the directivityTF to be removed.

#### Return

A return message is sent back to the sender: `control/actionResult /resources/removeDirectivityTF <string directivityTF_id> <bool removed>`, indicating `removed=true`if the directivityTF is successfuly removed and `removed=false` if not. In both cases a `description` is added to give more details.

In case of success, it sends an echo to all subscribers excepting the sender: `/resources/removeDirectivityTF <string directivityTF_id>`

#### Example

BeRTA receives: `/resources/removeDirectivityTF directivityTF1`

BeRTA sends back to the sender: ` control/ actionResult /resources/removeDirectivityTF directivityTF1 true DirectivityTF directivityTF1 removed` or `control actionResult /resources/removeDirectivityTF directivityTF1 false ERROR deleting DirectivityTF.  directivityTF1 not found in the list`

BeRTA sends an echo to all subscribiers excepting the sender: `/resources/removeDirectivityTF directivityTF1`
<!----------------------------------------------------------------------------------->
---


## `/resources/getDirectivityTFInfo`

Gets some information about one of the loaded Directivity Transfer Functions. This information consists of the filename and the spatial resolution used to resample it in the [BRT grid]().
#### Syntax

`/resources/getDirectivityTFInfo <string directivityTF_id>`

`directivityTF_id`: Identifier of the DirectivityTF of which we are requesting information.

#### Return

`/resources/getDirectivityTFInfo <string directivityTF_id> <string directivityTF_SOFAFile> <float spatialResolution>`

The return message is sent back to the sender and refers to the `directivityTF_id`, indicating the name of the SOFA file (`directivityTF_SOFAFile`) from which the directivity was loaded, as well as the `spatialResolution` used to resample it when loaded. 

#### Example

BeRTA receives: `/resources/getDirectivityTFInfo directivityTF1`

BeRTA sends back: `/resources/getDirectivityTFInfo directivityTF1 directivityTF.sofa 15` 

<!----------------------------------------------------------------------------------->
---


## `/resources/loadSOSFilters`

Loads a new set of Second Order Section (SOS) filters in a SOFA file from the specified path and assign an identifier to it. If there is already another set of SOS filters with the same identifier, it is substituted by the new one. The SOFA file must use SOS as data type. 

#### Syntax

`/resources/loadSOSFilters <string SOSFilters_id> <string SOSFilters_SOFAFile_path>`

`SOSFilters_id`: Identifier to be assigned to the NFC Filters for later references to it. If there is already another set of NFC Filters with the same identifier, it is substituted by the new one. Otherwise, a new set of NFC Filters is created.

`SOSFilters_SOFAFile_path`: Specifies a SOFA file containing a set of NFC Filters. It can be either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable.  

#### Return

A return message is sent back to the sender:  `control/actionResult /resources/loadSOSFilters <string SOSFilters_id> <boolean loaded>`, indicating `loaded=true`if SOS NFC Filters are successfuly loaded and `loaded=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers but the sender: `/resources/loadSOSFilters <string NSOSilters_id> <string SOSFilters_SOFAFile_path>`

#### Example

BeRTA receives: `/resources/loadSOSFilters SOSF1 c:/tmp/SOSilters.sofa`

BeRTA sends back to the sender: `control/actionResult /resources/loadSOSFilters NFCF1 true SOS filter SOSF1 loaded successfully, from the file: c:/tmp/SOSilters.sofa` or `/resources/loadSOSFilters SOSF1 false ERROR deleting SOS filter.  SOSF1 not found in the list`. 

BeRTA sends an echo to all subscribiers but the sender: `/resources/loadSOSFilters SOSF1 c:/tmp/SOSilters.sofa`

<!----------------------------------------------------------------------------------->
---


## `/resources/removeSOSFilters`

Removes a set of Near Field Compensation (NFC) filters from the loaded resources. 

#### Syntax

`/resources/removeSOSFilters <string SOSFilters_id>`

`SOSFilters_id`: Identifier of the NFC Filters to be removed.

#### Return

A return message is sent back to the sender:  `control/actionResult/resources/removeSOSFilters <string SOSFilters_id> <bool removed> <string description>`, indicating `removed=true`if the NFC Filters are successfuly removed and `removed=false` if not. In both cases a `description` is added to give more details.

In case of success, an echo is sent to all subscribers but the sender: `/resources/removeSOSFilters <string SOSFilters_id>`

#### Example

BeRTA receives: `/resources/removeSOSFilters SOSF1`

BeRTA sends back to the sender: `control/actionResult /resources/removeSOSFilters SOSF1 true SOSF1 removed from the list` or `control/actionResult /resources/removeSOSFilters SOSF1 false ERROR deleting SOS Filters.  SOSF1 not found in the SOS Filters list`

BeRTA sends an echo to all subscribiers but the sender: `/resources/removeSOSFilters SOSF1`
<!----------------------------------------------------------------------------------->
---


## `/resources/getSOSFiltersInfo`

Gets some information about one of the loaded set of Near Field Compensation (NFC) filters. This information consists of the name of the file from which it was loaded.
#### Syntax

`/resources/getSOSFiltersInfo <string SOSFilters_id>`

`SOSFilters_id`: Identifier of the NFC Filters of which we are requesting information.

#### Return

`/resources/getSOSFiltersInfo <string SOSFilters_id> <string SOSFilters_SOFAFile>`

The return message is sent back to the sender and refers to the `SOSFilters_id`, indicating the name of the SOFA file (`SOSFilters_SOFAFile`) from which the filters were loaded. 

#### Example

BeRTA receives: `/resources/getSOSFiltersInfo NFCF1`

BeRTA sends back: `/resources/getSOSFiltersInfo NFCF1 NFCFilters.sofa` 

<!----------------------------------------------------------------------------------->
---


## `/resources/loadBRIR`

Loads a new Binaural Room Impulse Response (BRIR) from a SOFA file at the specified path and assign an identifier to it. If there is already another BRIR with the same identifier, it is substituted by the new one. The SOFA file must use FIR or FIR-E as data type but can follow any convention using these data types. When loading, the HRTF is spatially resampled to fit the [BRT grids](). 

#### Syntax

`/resources/loadBRIR <string BRIR_id> <string BRIR_SOFAFile_path> <float spatialResolution> <float fadeInWindowThreshold> <float fadeInWindowRiseTime> <float fadeOutWindowThreshold> <float fadeOutWindowFallTime>`

`BRIR_id`: Identifier to be assigned to the BRIR for later references to it. If there is already another BRIR with the same identifier, it is substituted by the new one. Otherwise, a new BRIR is created.

`BRIR_SOFAFile_path`: Specifies a SOFA file containing a BRIR. It can be either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable.  

`spatialResolution`: This parameter indicates the interpolation step in the horizontal plane of zero elevation. To learn more about the interpolation mechanism, please refer to [BRT grids]().

The loaded BRIR can be windowed. For this purpose, a fade-in and fade-out mechanism has been enabled using raised-cosines. The parameter `fadeInWindowThreshold` indicates the midpoint (50%) of the rising slope of the window, in seconds, the `fadeInWindowRisetime` indicates the rising time, also in seconds. Analogously, the parameter `fadeOutWindowThreshold` indicates the midpoint (50%) of the falling slope of the window, and `fadeOutWindowFalltime` indicates the falling time, all in seconds.

#### Return

A return message is sent back to the sender:  `control/actionResult /resources/loadBRIR <string BRIR_id> <boolean loaded>`, indicating `loaded=true`if the BRIR is successfuly loaded and `loaded=false` if not. In both cases a `description` is added to give more details.

In case of success, an echo is sent to all subscribers excepting the sender: `/resources/loadBRIR <string BRIR_id> <string BRIR_SOFAFile_path> <float spatialResolution> <float fadeInWindowThreshold> <float fadeInWindowRiseTime> <float fadeOutWindowThreshold> <float fadeOutWindowFallTime>`

#### Example

BeRTA receives: `/resources/loadBRIR BRIR1 c:/tmp/BRIR.sofa 15 0.005 0.001 0.5 0.01`

BeRTA sends back to the sender: `/resources/loadBRIR BRIR1 true BRIR BRIR1 loaded successfully, with a spatial resolution of 15, from the file: c:/tmp/BRIR.sofa` or `/resources/loadBRIR BRIR1 false BRIR sofa file couldn't be loaded from c:/tmp/BRIR.sofa`. 

BeRTA sends an echo to all subscribiers but the sender: `/resources/loadBRIR BRIR1 c:/tmp/BRIR.sofa 15 0.005 0.001 0.5 0.01`

<!----------------------------------------------------------------------------------->
---


## `/resources/removeBRIR`

Removes a Binaural Room Impulse Response (BRIR) from the loaded resources. 

#### Syntax

`/resources/removeBRIR <string BRIR_id>`

`BRIR_id`: Identifier of the BRIR to be removed.

#### Return

A return message is sent back to the sender: `control/actionResult /resources/removeBRIR <string BRIR_id> <bool removed> <string description>`, indicating `removed=true`if the BRIR successfuly removed and `removed=false` if not. In both cases a `description` is added to give more details.

In case of success, an echo is sent to all subscribers but the sender: `/resources/removeBRIR <string BRIR_id>`

#### Example

BeRTA receives: `/resources/removeBRIR BRIR1`

BeRTA sends back to all subscribers: `/resources/removeBRIR BRIR1 true BRIR BRIR1 removed` or just to the sender: `/resources/removeBRIR BRIR1 false ERROR deleting BRIR.  BRIR1 not found in the list`

BeRTA sends an echo to all subscribiers but the sender: `/resources/removeBRIR BRIR1`
<!----------------------------------------------------------------------------------->
---


## `/resources/getBRIRInfo`

Gets some information about one of the loaded Binaural Room Impulse Response (BRIR). This information consists of the name of the file from which it was loaded, the spatial resolution and all the parameters of the window with which the BRIR is windowed.

#### Syntax

`/resources/getBRIRInfo <string BRIR_id>`

`BRIR_id`: Identifier of the BRIR of which we are requesting information.

#### Return

`/resources/getBRIRInfo <string BRIR_id> <string BRIR_SOFAFile> <float spatialResolution> <float fadeInWindowThreshold> <float fadeInWindowRiseTime> <float fadeOutWindowThreshold> <float fadeOutWindowFallTime>`

The return message is sent back to the sender and refers to the `BRIR_id`, indicating the name of the SOFA file (`BRIR_SOFAFile`) from which the filters were loaded, the spatial resolution at which it is resampled and all the parameters defining the windowing: `fadeInWindowThreshold`and `fadeInWindowRiseTime` for the fade-in rising slope; and `fadeOutWindowThreshold` and `fadeOutWindowFallTime>` for the fade-out falling slope. 

#### Example

BeRTA receives: `/resources/getBRIRInfo BRIR1`

BeRTA sends back: `/resources/getBRIRInfo BRIR1 BRIR.sofa 15 0.005 0.001 0.5 0.01` 

<!----------------------------------------------------------------------------------->
---

