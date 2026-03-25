# Resources Menu

The **Resources** menu displays all resources currently loaded in the application.

Resources can be loaded in two ways:

* Dynamically during execution via **OSC commands**
* At startup, from the resource lists defined in the `settings.json` configuration file

The content of this menu is updated **in real time** as new resources are loaded.

<div style="border: 1px solid #000; padding: 10px; display: block; max-width: 800px; margin: 0 auto;">
    <img src="/BRT-Documentation/assets/berta_renderer_resources_menu.png" alt="BeRTA Renderer - Resources menu" style="display: block; margin: 0 auto; max-width: 100%;">
    <p style="text-align: center;">BeRTA Renderer - Resources menu</p>
</div>

## Resource Lists

Resources are organized into separate tabs, one for each resource type:

* **HRTFs**
* **Directivities**
* **Filters**
* **BRIRs**
* **Rooms**

Each tab contains a table listing the resources of that type.

## Displayed Information

Each resource is represented as a row in the corresponding table. The following information is shown:

| Column          | Description                                                                                    |
| --------------- | ---------------------------------------------------------------------------------------------- |
| **ID**          | Unique identifier of the resource within the application                                       |
| **Type**        | Describes the nature of the resource. Its meaning depends on the resource category (see below) |
| **Source File** | Path or name of the file from which the resource was loaded                                    |

!!! note
    The **Type** column has a different meaning depending on the resource category.

## Type Column by Resource

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

## Notes

* This menu is **read-only**: it is intended for inspection of the currently loaded resources
* Resources appear automatically when loaded via OSC or configuration
* Future versions may include additional interaction features (e.g., resource removal or inspection)
