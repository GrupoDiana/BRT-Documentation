# Resources Menu

The **Resources** menu displays all resources currently loaded in the application.

Resources can be loaded in three ways:

* Dynamically during execution via **OSC commands**
* At startup, from the resource lists defined in the `settings.json` configuration file
* Using the ‘Add’ buttons in this menu of the user interface

The content of this menu is updated **in real time** as new resources are loaded.

<div style="border: 1px solid #000; padding: 10px; display: block; max-width: 900px; margin: 0 auto;">
    <img src="/BRT-Documentation/assets/berta_renderer_resources_menu.png" alt="BeRTA Renderer - Resources menu" style="display: block; margin: 0 auto; max-width: 100%;">
    <p style="text-align: center;">BeRTA Renderer - Resources menu</p>
</div>

## Resource Lists
Resources are organized into separate tabs, one for each resource type:

* **Sound Sources**
* **HRTFs**
* **Directivities**
* **Filters**
* **BRIRs**
* **Rooms**

Each tab contains a table listing the resources of that type.

### Sound Sources

#### Displayed Information
Each resource is represented as a row in the corresponding table. The following information is shown:

| Column          | Description                                                                                    |
| --------------- | ---------------------------------------------------------------------------------------------- |
| **Source ID**   | Unique identifier of the sound source                                                          |
| **Source Model**| Specify the type of microphone used: directional or omnidirectional.                           |
| **Input type**  | Indicate whether the source has been loaded from an audio file or is being read in real time from an input channel on the interface.|
| **WAV File / Input Channel**| Displays the audio file or input channel currently in use.                         |
| **Actions**     | Button to delete the sound source from the scene                                               |



### SOFA Resources

#### Displayed Information

Each resource is represented as a row in the corresponding table. The following information is shown:

| Column          | Description                                                                                    |
| --------------- | ---------------------------------------------------------------------------------------------- |
| **ID**          | Unique identifier of the resource within the application                                       |
| **Type**        | Describes the nature of the resource. Its meaning depends on the resource category (see below) |
| **Source File** | Path or name of the file from which the resource was loaded                                    |
| **Actions**     | Buttons to delete or set that resource on a compatible model.                                  |

!!! note
    The **Type** column has a different meaning depending on the resource category.

#### Type Column by Resource

The interpretation of the **Type** column varies depending on the selected tab:

- **HRTFs / Directivities**: Indicates whether the dataset has been **interpolated** or loaded as **raw data**

- **Filters**: Indicates the filter structure.
    - **FIR** (Finite Impulse Response)
    - **IIR** (Infinite Impulse Response)
- **Rooms**: Indicates the room model type.
    - **Shoebox** (parametric rectangular room)
    - **Free-form** (geometry loaded from an external `.obj` file)
- **BRIRs**
    - This category does **not include a Type column**
    - Only **ID** and **Source File** are displayed


## Actions Buttons
- **Add _X_**: Opens a pop-up menu to load a new resource of type X.
- **Set**: Set that resource to a compatible model from those loaded in the scene.
- **Delete**: Remove that resource.

## Notes
- The same resource file can be loaded several times using different IDs.