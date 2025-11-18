These commands are responsible for managing the resources used in the virtual audio scene.

<hr style="border:1px solid gray">
<!----------------------------------------------------------------------------------->

## **HRTF**

### **/resources/loadHRTF**

Loads a new HRTF in a SOFA file from the specified path and assign an identifier to it. If there is already another HRTF with the same identifier, it is substituted by the new one. The SOFA file must use FIR or FIR-E as data type but can follow any convention using these data types. When loading, the HRTF is spatially resampled to fit the [HRTF grids](). This way the real time processor can get fast enough the three HRIRs to be interpolated and convolved.

#### Syntax

`/resources/loadHRTF <string HRTF_id> <string HRTF_SOFAFile_path> <float spatialResolution>`

`HRTF_id`: Identifier to be assigned to the HRTF for later references to it. If there is already another HRTF with the same identifier, it is substituted by the new one. Otherwise, a new HRTF is created.

`HRTF_SOFAFile_path`: Specifies a SOFA file containing an HRTF. it can be either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable.  

`spatialResolution`: This parameter indicates the interpolation step in the horizontal plane of zero elevation. To learn more about this grid and the interpolation mechanism, please refer to the [HRTF grids]().

#### Return

`/control/actionResult /resources/loadHRTF <string HRTF_id> <boolean loaded> <string description>`

The return confirmation refers to the `HRTF_id`,indicating `loaded=true`if the HTYF is successfuly loaded and `loaded=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/loadHRTF HRTF1 c:/tmp/hrtf.sofa 5`

BeRTA sends back to the sender: `/control/actionResult /resources/loadHRTF HRTF1 true HRTF HRTF1 loaded successfully, with a spatial resolution of 5, from the file: c:/tmp/hrtf.sofa` or `/control/actionResult /resources/loadHRTF HRTF1 false HRTF sofa file couldn't be loaded from HRTF1;`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/removeHRTF**

Removes an HRTF from the loaded resources. 

#### Syntax

`/resources/removeHRTF <string HRTF_id>`

`HRTF_id`: Identifier of the HRTF to be removed.

#### Return

`/control/actionResult /resources/loadHRTF <string HRTF_id> <bool removed> <string description>`

The return confirmation refers to the `HRTF_id`,indicating `removed=true`if the HRTF is successfuly removed and `removed=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/removeHRTF HRTF1`

BeRTA sends back to all subscribers: `/control/actionResult /resources/removeHRTF HRTF1 true HRTF HRTF1 removed` or `/control/actionResult /resources/removeHRTF HRTF1 false error: HRTF not found in the list`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/getHRTFInfo**

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
<hr style="border:1px solid gray">

### **/resources/setHRTFHeadRadius**

Sets the head radius to be stored into the HRTF. This value affects: (1) the position of the ears used in the parallax correction when getting HRIRs; and (2) the calculation of the ITD when using the simulation based on the Woodworth formula whcih is activated or deactivated by the `/resources/enableWoodworthITD` command. If this command is not received, the position of receivers in the HRTF SOFA file is used as the default value. See `/resources/restoreHRTFHeadRadius` as a related command.

#### Syntax

`/resources/setHRTFHeadRadius <string HRTF_id> <float headRadius>`

`HRTF_id`: Identifier of the HRTF of which we are setting the new head radius.

`headRadius`: New head radius in meters. Only positives values are accepted.

#### Return

`/control/actionResult /resources/setHRTFHeadRadius <string HRTF_id> <bool set> <string description>`

The return confirmation refers to the `HRTF_id`, indicating `set=true`if the HRTF is successfuly set and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/setHRTFHeadRadius HRTF1 0.09`

BeRTA sends back to the sender: `/control/actionResult /resources/setHRTFHeadRadius HRTF1 true success` 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/getHRTFHeadRadius**

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
<hr style="border:1px solid gray">


### **/resources/restoreHRTFHeadRadius**

Sets the head radius of the HRTF to the one stored in the SOFA file as the position of receivers. This value affects: (1) the position of the ears used in the parallax correction when getting HRIRs; and (2) the calculation of the ITD when using the simulation based on the Woodworth formula whcih is activated or deactivated by the `/resources/enableWoodworthITD` command. 

#### Syntax

`/resources/restoreHRTFHeadRadius <string HRTF_id>`

`HRTF_id`: Identifier of the HRTF for which we are setting the head radius.

#### Return

`/control/actionResult /resources/restoreHRTFHeadRadius <string HRTF_id> <bool restored> <string description>`

The return confirmation refers to the `HRTF_id`, indicating `restored=true`if the HRTF is successfuly set and `restored=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/restoreHRTFHeadRadius HRTF1`

BeRTA sends back to the sender: `/control/actionResult /resources/restoreHRTFHeadRadius HRTF1 true success` 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/enableWoodworthITD**

Enables or disables the calculation of the ITD based on the Woodworth formula, considering head radius, or it is extracted from the delays in the HRTF SOFA file. The Woodworth formula needs the head radius set by commands `/resources/setHRTFHeadRadius` and `/resources/restoreHRTFHeadRadius`. By default, the use of the Woodworth formula is disabled.
#### Syntax

`/resources/enableWoodworthITD <string HRTF_id> <boolean enable>`

`HRTF_id`: Identifier of the HRTF for which we are setting the head radius.

`enable`: If enable is true (1), the ITD calculation is performed using the Woodworth formula. If enable is false (0), the ITD is simulated from the delays read from the SOFA file.


#### Return

`/control/actionResult /resources/enableWoodworthITD <string HRTF_id> <bool enabled> <string description>`

The return confirmation refers to the `HRTF_id`, indicating `enabled=true`if the HRTF is successfuly set and `enabled=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/enableWoodworthITD HRTF1 true`

BeRTA sends back to the sender: `/control/actionResult /resources/enableWoodworthITD HRTF1 true success` 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

## **BRIR**

### **/resources/loadBRIR**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Parameters renamed from BeRTA v3.6.0</span>

Loads a new Binaural Room Impulse Response (BRIR) from a SOFA file at the specified path and assign an identifier to it. If there is already another BRIR with the same identifier, it is substituted by the new one. The SOFA file must use FIR or FIR-E as data type but can follow any convention using these data types. When loading, the HRTF is spatially resampled to fit the [BRT grids](). 

#### Syntax

`/resources/loadBRIR <string BRIR_id> <string BRIR_SOFAFile_path> <float spatialResolution> <float fadeInBegin> <float riseTime> <float fadeOutCutoff> <float fallTime>`

`BRIR_id`: Identifier to be assigned to the BRIR for later references to it. If there is already another BRIR with the same identifier, it is substituted by the new one. Otherwise, a new BRIR is created.

`BRIR_SOFAFile_path`: Specifies a SOFA file containing a BRIR. It can be either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable.  

`spatialResolution`: This parameter indicates the interpolation step in the horizontal plane of zero elevation. To learn more about the interpolation mechanism, please refer to [BRT grids]().

The loaded BRIR can be windowed. For this purpose, a fade-in and fade-out mechanism has been enabled using raised-cosines. The parameter `fadeInBegin` indicates the midpoint (50%) of the rising slope of the window, in seconds, the `risetime` indicates the rising time, also in seconds. Analogously, the parameter `fadeOutCutoff` indicates the midpoint (50%) of the falling slope of the window, and `fallTime` indicates the falling time, all in seconds.

#### Return

`/control/actionResult /resources/loadBRIR <string BRIR_id> <boolean loaded>`

The return confirmation refers to the `BRIR_id`, indicating `loaded=true`if the BRIR is successfuly loaded and `loaded=false` if not. In both cases a `description` is added to give more details.

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/loadBRIR BRIR1 c:/tmp/BRIR.sofa 15 0.005 0.001 0.5 0.01`

BeRTA sends back to the sender: `/control/actionResult /resources/loadBRIR BRIR1 true BRIR BRIR1 loaded successfully, with a spatial resolution of 15, from the file: c:/tmp/BRIR.sofa` or `/control/actionResult /resources/loadBRIR BRIR1 false BRIR sofa file couldn't be loaded from c:/tmp/BRIR.sofa`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/removeBRIR**

Removes a Binaural Room Impulse Response (BRIR) from the loaded resources. 

#### Syntax

`/resources/removeBRIR <string BRIR_id>`

`BRIR_id`: Identifier of the BRIR to be removed.

#### Return

`/control/actionResult /resources/removeBRIR <string BRIR_id> <bool removed> <string description>`

The return confirmation refers to the `BRIR_id`, indicating `removed=true`if the BRIR successfuly removed and `removed=false` if not. In both cases a `description` is added to give more details.

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/resources/removeBRIR BRIR1`

BeRTA sends back to all subscribers: `/control/actionResult /resources/removeBRIR BRIR1 true BRIR BRIR1 removed` or just to the sender: `/control/actionResult /resources/removeBRIR BRIR1 false ERROR deleting BRIR.  BRIR1 not found in the list`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/getBRIRInfo**

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
<hr style="border:1px solid gray">

## **Directivity**

### **/resources/loadDirectivityTF**

Loads a new Directivity Transfer Function in a SOFA file from the specified path and assign an identifier to it. If there is already another Directivity with the same identifier, it is substituted by the new one. The SOFA file must use TF as data type with the same number of frequency bins as the frame size configured in BeRTA. When loading, the DirectivityTF is spatially resampled to fit the [BRT grids](). This way the real time processor can get fast enough the three Directivities to be interpolated and convolved.

#### Syntax

`/resources/loadDirectivityTF <string directivityTF_id> <string directivityTF_SOFAFile_path> <float spatialResolution>`

`directivityTF_id`: Identifier to be assigned to the directivityTF for later references to it. If there is already another directivityTF with the same identifier, it is substituted by the new one. Otherwise, a new directivityTF is created.

`directivityTF_SOFAFile_path`: Specifies a SOFA file containing a directivityTF. it can be either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable.  

`spatialResolution`: This parameter indicates the interpolation step in the horizontal plane of zero elevation. To learn more about this grid and the interpolation mechanism, please refer to the [BRT grids]().

#### Return

`/control/actionResult /resources/loadDirectivityTF <string directivityTF_id> <boolean loaded> <string description>`

The return confirmation refers to the `directivityTF_id`, indicating `loaded=true`if the directivityTF is successfuly loaded and `loaded=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/resources/loadDirectivityTF DirectivityTF1 c:/tmp/directivityTF.sofa 15`

BeRTA sends back to the sender: `/control/actionResult /resources/loadDirectivityTF DirectivityTF1 true Directivity directivityTF1 loaded` or `/control/actionResult /resources/loadDirectivityTF DirectivityTF1 false Directivity sofa file couldn't be loaded from c:/tmp/directivityTF.sofa`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/removeDirectivityTF**

Removes a Directivity Transfer Function from the loaded resources. 

#### Syntax

`/resources/removeDirectivityTF <string directivityTF_id>`

`directivityTF_id`: Identifier of the directivityTF to be removed.

#### Return

`/control/actionResult /resources/removeDirectivityTF <string directivityTF_id> <bool removed>`

The return confirmation refers to the `directivityTF_id`, indicating `removed=true`if the directivityTF is successfuly removed and `removed=false` if not. In both cases a `description` is added to give more details.

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/removeDirectivityTF directivityTF1`

BeRTA sends back to the sender: ` /control/actionResult /resources/removeDirectivityTF directivityTF1 true DirectivityTF directivityTF1 removed` or `/control/actionResult /resources/removeDirectivityTF directivityTF1 false ERROR deleting DirectivityTF.  directivityTF1 not found in the list`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/getDirectivityTFInfo**

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
<hr style="border:1px solid gray">

## **SOS Filters**

### **/resources/loadSOSFilters**

Loads a new set of Second Order Section (SOS) filters in a SOFA file from the specified path and assign an identifier to it. If there is already another set of SOS filters with the same identifier, it is substituted by the new one. The SOFA file must use SOS as data type. 

#### Syntax

`/resources/loadSOSFilters <string SOSFilters_id> <string SOSFilters_SOFAFile_path>`

`SOSFilters_id`: Identifier to be assigned to the NFC Filters for later references to it. If there is already another set of NFC Filters with the same identifier, it is substituted by the new one. Otherwise, a new set of NFC Filters is created.

`SOSFilters_SOFAFile_path`: Specifies a SOFA file containing a set of NFC Filters. It can be either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable.  

#### Return

 `/control/actionResult /resources/loadSOSFilters <string SOSFilters_id> <boolean loaded>`
 
 The return confirmation refers to the `SOSFilters_id`, indicating `loaded=true`if SOS NFC Filters are successfuly loaded and `loaded=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/resources/loadSOSFilters SOSF1 c:/tmp/SOSilters.sofa`

BeRTA sends back to the sender: `/control/actionResult /resources/loadSOSFilters NFCF1 true SOS filter SOSF1 loaded successfully, from the file: c:/tmp/SOSilters.sofa` or `/resources/loadSOSFilters SOSF1 false ERROR deleting SOS filter.  SOSF1 not found in the list`. 


<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/removeSOSFilters**

Removes a set of Near Field Compensation (NFC) filters from the loaded resources. 

#### Syntax

`/resources/removeSOSFilters <string SOSFilters_id>`

`SOSFilters_id`: Identifier of the NFC Filters to be removed.

#### Return

 `/control/actionResult /resources/removeSOSFilters <string SOSFilters_id> <bool removed> <string description>`
 
 The return confirmation refers to the `SOSFilters_id`, indicating `removed=true`if the NFC Filters are successfuly removed and `removed=false` if not. In both cases a `description` is added to give more details.

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/removeSOSFilters SOSF1`

BeRTA sends back to the sender: `/control/actionResult /resources/removeSOSFilters SOSF1 true SOSF1 removed from the list` or `/control/actionResult /resources/removeSOSFilters SOSF1 false ERROR deleting SOS Filters.  SOSF1 not found in the SOS Filters list`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">


### **/resources/getSOSFiltersInfo**

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
<hr style="border:1px solid gray">

## **Room**

### **/resources/loadRoom**

Loads a new room from a OBJ file from the specified path and assign an identifier to it. If there is already another set of room with the same identifier, it is substituted by the new one. Please note the considerations regarding OBJ files specified in the OBJ services section.

#### Syntax

`/resources/loadRoom <string Room_id> <string OBJ_File_path>`

`Room_id`: Identifier to be assigned to the Room for later references to it. If there is already another room with the same identifier, it is substituted by the new one. 

`OBJ_File_path`: Specifies an OBJ file containing a room (see the OBJ Services section for more information). It can be either relative or absolute. If a relative path is used it will be calculated from the data folder that can be found in the same folder as the BeRTA executable. 

#### Return

 `/control/actionResult /resources/loadRoom <string Room_id> <boolean loaded>`
 
 The return confirmation refers to the `Room_id`, indicating `loaded=true`if room are successfuly loaded and `loaded=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender:`/resources/loadRoom room1 c:/tmp/Room1.obj`

BeRTA sends back to the sender: `/control/actionResult /resources/loadRoom room1 true Room (room1) loaded successfully, from the file: c:/tmp/Room1.obj` or `/resources/loadRoom room1 false ERROR File path could not be found: c:/tmp/Room1.obj`. 

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">


### **/resources/loadShoeBoxRoom**

Sets up a shoebox room with six walls (four walls, plus floor and ceiling). The six walls are automatically created with the centre of the room at (0, 0, 0).  The following ID are assigned for each wall: 

- `0` front wall (positive x)

- `1` left wall (positive y)

- `2` right wall (negative y)

- `3` back wall (negative x)

- `4` floor (negative z)

- `5` ceiling (positive z)

#### Syntax

`/resources/setShoeBoxRoom <string Room_id> <float length> <float width> <float height>`

`Room_id`: identifier to be assigned to the Room for later references to it. If there is already another room with the same identifier, it is substituted by the new one. .

`length`: floating value expressing the length of the room on the X axis, expressed in metres.

`width`: Floating value expressing the length of the room on the Y axis, expressed in metres.

`heigth`: Floating value expressing the length of the room on the Z axis, expressed in metres.

#### Return

`/control/actionResult /resources/setShoeBoxRoom <string Room_id> <bool set> <string description>`

The return confirmation refers to the `Room_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/setShoeBoxRoom room1 6 8 3`

BeRTA sends back to the sender: `/control/actionResult /environment/setShoeBoxRoom room1 true "ShoeBox Room (room1) created succesfully with dimensions Length 6m, Width 8m, Height 3m`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/removeRoom**

Removes a room from the loaded resources. 

#### Syntax

`/resources/removeRoom <string Room_id>`

`Room_id`: Identifier of the room to be removed.

#### Return

 `/control/actionResult /resources/removeRoom <string Room_id> <bool removed> <string description>`
 
 The return confirmation refers to the `Room_id`, indicating `removed=true`if the room are successfuly removed and `removed=false` if not. In both cases a `description` is added to give more details.

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/removeRoom room1`

BeRTA sends back to the sender: `/control/actionResult /resources/removeRoom room1 true Room (room1) removed from the resources list` or `/control/actionResult /resources/removeRoom room1 false ERROR deleting Room. Room1 not found in list.`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/setRoomWallAbsorption**
<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.X.X, previously called /environment/setWallAbsorption.</span>

Sets the absorption coefficients of a room wall. It supports two modes of operation: 

- Specify a frequency-independent absorption coefficient. That is, using a single number between 0 and 1, which is used for all bands. 
- Specify absorption coefficients for each of the 9 simulated bands. In other words, in this case, nine numbers between 0 and 1 are used, with absorptions corresponding to 62.5 Hz, 125 Hz, 250 Hz, 500 Hz, 1 kHz, 2 kHz, 4 kHz, 8 kHz and 16 kHz.

#### Syntax

Two options available:

- `/resources/setRoomWallAbsorption <string Room_id> <int wall_id> <float absorption_fullband>`
- `/resources/setRoomWallAbsorption <string Room_id> <int wall_id> <float abs62> <float abs125> <float abs250> <float abs500> <float abs1K> <float abs2K> <float abs4K> <float abs8K> <float abs16K>`

`Room_id`: identifier of the room.

`wall_id`: index of the wall to which the absorption apllies.

`absoption_fulband`: absorption coefficient which is frequency independent.

`abs62`: absorption coefficient of the band centered at 62.5Hz.

`abs125`: absorption coefficient of the band centered at 125Hz.

`abs250`: absorption coefficient of the band centered at 250Hz.

`abs500`: absorption coefficient of the band centered at 500Hz.

`abs1K`: absorption coefficient of the band centered at 1KHz.

`abs2K`: absorption coefficient of the band centered at 2KHz.

`abs4K`: absorption coefficient of the band centered at 4KHz.

`abs8K`: absorption coefficient of the band centered at 8KHz.

`abs16K`: absorption coefficient of the band centered at 16KHz.

#### Return

`/control/actionResult /resources/setRoomWallAbsorption <string Room_id> <bool set> <string description>`

The return confirmation refers to the `Room_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.

#### Examples

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/setRoomWallAbsorption Room1 0 0.1 0.2 0.3 0.4 0.5 0.4 0.3 0.2 0.1`

BeRTA sends back to the sender: `/control/actionResult /resources/setRoomWallAbsorption Room1 true "The absortion coefficients of the wall (0) of the room (Room1) have been updated: [0.100000, ...]"`

<!----------------------------------------------------------------------------------->
<hr style="border:1px solid gray">

### **/resources/enableRoomWall**

<span style="font-size: 0.8em; color: grey; font-style: italic;">Available from BeRTA v3.X.X.</span>

Enables or disables a wall in the room. Disabling it has the same effect as not having defined it. By default, all walls are activated after defining the geometry of a room.

#### Syntax

`/resources/enableRoomWall <string Room_id> <int wall_id> <boolean enable>`


`Room_id`: identifier assigned to the model.

`wall_id`: index of the wall to which the absorption apllies.

`enable`: If false (0), disable the simulation of the indicated room wall. If true (1), enables the simulation of the indicated room wall. By default, all walls are enabled.


#### Return

`/control/actionResult /resources/enableRoomWall <string Room_id> <bool set> <string description>`

The return confirmation refers to the `Room_id`, indicating `set=true` if the action has been successfully performed and `set=false` if not. In both cases a `description` is added to give more details. 

In case of success, an echo is sent to all subscribers except the sender, using the same syntax as the received message.


#### Example

BeRTA receives and echoes back to all subscribiers but the sender: `/resources/enableRoomWall Room1 1 false`

BeRTA sends back to the sender: `/control/actionResult /resources/enableRoomWall Room1 true "The wall (1) of the room (Room1) has been disabled."`. 
