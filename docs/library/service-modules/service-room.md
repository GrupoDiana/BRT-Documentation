# Room Service Module

The Room Service Module provides the geometric and acoustic description of an enclosed space used by the BRT’s simulation engines. It offers a unified representation of room boundaries, wall materials, and spatial relationships required by the **Image-Source Method (ISM)** and the **Scattering Delay Network (SDN)** rendering algorithms.

This module consists of two main classes:

- **`CRoom`** — container of all walls and high-level room operations.
- **`CWall`** — geometric and acoustic description of a single wall element.

Both classes form the core data structure used by the acoustic simulators to evaluate reflections, image sources, occlusion, and room-material interactions.

---

## 1. Purpose and Scope

The Room module describes **the physical enclosure** in which acoustic propagation is simulated. Its responsibilities include:

- Defining the **geometric boundaries** of a room using planar polygonal walls.
- Storing **per-wall acoustic absorption** data across 9 octave bands.
- Ensuring the **convexity** and **validity** of the room geometry.
- Providing a standardized interface used internally by:
    - **ISM (Image-Source Method)** for reflection and image-source generation.
    - **SDN (Scattering Delay Network)** for simulating late reverberation and diffusion.
- Computing geometric relationships such as:
    - Point containment (whether a source or image lies inside the room).
    - Image-room generation for reflective simulation steps.

This module is **not** responsible for rendering, DSP, or file loading. OBJ/MTL importing is described in a separate section (see [OBJ Reader](../readers/obj-reader.md) documentation).

---

## 2. Room Geometry Requirements

### 2.1 Convexity Requirement (Mandatory)

The BRT assumes that all rooms are **strictly convex**.
This is a **strong requirement** of the underlying acoustics algorithms:

* Convexity ensures correct visibility and ordering of reflections in ISM.
* Image-room generation relies on consistent wall normals and enclosure shape.
* Non-convex geometry may lead to invalid or missing reflections.

Users must ensure that custom geometries — whether built manually or imported via OBJ — meet this condition.

### 2.2 Wall Representation

Each wall is represented as a **planar, convex polygon** (typically a quadrilateral). Walls are defined by a list of 3D vertices stored in counter-clockwise order as seen from inside the room.

#### Automatically enforced planarity

If a vertex slightly deviates from the plane defined by the first three vertices, it is automatically projected onto the plane, guaranteeing that all wall polygons remain strictly planar.


#### Wall normals

Wall normals are computed automatically using the corner order. By convention and requirement:

* **The normal must point towards the interior of the room**.
* This direction determines the inward-facing reflection plane used by the ISM engine.

Users creating custom rooms must ensure the correct ordering of corners to preserve this convention.

---

## 3. CWall: Acoustic and Geometric Description of a Wall

A `CWall` models one reflective boundary of the room. It provides:

### 3.1 Geometry

* Convex polygon defined by an ordered list of vertices.
* Automatically enforced planarity.
* Access to geometric properties such as normal vector, center, projection, intersections, and distances.

### 3.2 Acoustic Absorption

Each wall stores **nine absorption coefficients**, representing **absorbed energy over incident energy** at the following octave-band center frequencies: 62.5 Hz, 125 Hz, 250 Hz, 500 Hz, 1 kHz, 2 kHz, 4 kHz, 8 kHz, 16 kHz.

Even when setting a single frequency-independent absorption value, it is internally propagated to all 9 bands.

### 3.3 Active / Inactive Walls

Walls can be **active** (reflective) or **inactive** (transparent). Typical uses include:

* Disabling floor or ceiling reflections for controlled experiments.
* Reducing the number of reflections and virtual sources in specific tests.
* Creating “partially open” spaces while maintaining convexity.

Inactive walls behave as if they do not exist during simulation.

---

## 4. CRoom: Full Room Container and High-Level Operations

The `CRoom` class stores and manages the complete acoustic enclosure. It provides two main construction pathways and several utilities required by the simulators.

---

### 4.1 Room Construction Options

#### **A. Shoebox Rooms**

`SetupShoeBox()` creates a rectangular room defined by: length (X axis), width (Y axis), height (Z axis).

This generates six walls with:

* Proper geometry and orientations.
* Correct inward-facing normals.
* Editable absorption coefficients.
* Optional activation/deactivation of individual walls.

Shoebox walls **cannot be reshaped**, but their absorption and active status can be modified.

---

#### **B. Arbitrary Geometry Rooms**

`SetupRoomGeometry()` accepts a `TRoomGeometry` structure containing:

* A list of **corner vertices**
* A list of **walls**, each defined by a list of vertex indices

This method is the natural endpoint of the OBJ loader, and also allows users to construct custom convex rooms programmatically.

Arbitrary geometry and shoebox rooms are fully equivalent once created.

---

### 4.2 Image Room Generation

`getImageRooms()` generates a set of **image rooms**, one for each active wall. These image rooms are used by the **Image-Source Method** inside the BRT to:

* Generate virtual (reflected) sources
* Compute high-order reflections
* Propagate room geometry recursively

This operation is internal to the acoustic engine, but the method is documented for completeness.

---

### 4.3 Point-In-Room Evaluation

`CheckPointInsideRoom()` determines whether a given point lies inside the room and measures the distance to the nearest wall. This is essential internally to:

* Validate positions of real sources and their image sources
* Handle propagation constraints
* Avoid reflections that occur outside the convex enclosure

This function is exposed for advanced users, although it is primarily used by the BRT internally.

---

## 5. Limitations and Notes

* The room must remain **convex** at all times.
* Walls must be **planar**; slight deviations are corrected automatically.
* Extremely fine or irregular meshes may reduce numerical robustness, although no issues are currently known.
* Vertices defining each wall should remain reasonable in size and spacing to ensure accurate simulation.

---

## 6. Relationship with OBJ Loader

Rooms can be constructed manually or imported from an OBJ/MTL file (see *OBJ Loader* section). The OBJ loader simply creates the internal `TRoomGeometry` structure and passes it to `CRoom`. See [OBJ Reader](../readers/obj-reader.md) section for more information.

---

## 7. Summary

The Room Service Module provides a robust, acoustically meaningful representation of an enclosed environment for binaural rendering. It:

* Defines the boundaries of the room via polygonal walls
* Stores multi-band acoustic absorption
* Guarantees geometric constraints required for ISM and SDN
* Generates image rooms for recursive reflection modeling
* Validates source positions during propagation

Together, `CRoom` and `CWall` form the geometric and acoustic foundation of all room-based simulation within the BRT.