# OBJ & MTL File Reader

This section documents how the BRT interprets and loads room geometries defined in **Wavefront OBJ** files together with their associated **MTL** material files.
The loading process is implemented through the `COBJReader` class.

---

## Overview

The OBJ loader provides a way to define room geometry using external modeling tools. The objective is to convert each face in the mesh into a `CWall` object and assemble a complete `CRoom` instance.

The loader is based on the **rapidobj** parser and supports a minimal subset of the OBJ/MTL specification—enough to define walls and their acoustic absorption properties.

---

## 1. OBJ File Requirements and Behaviour

### 1.1 Supported OBJ Directives

The loader processes a standard OBJ file and extracts:

| Directive        | Used?         | Purpose                                                         |
| ---------------- | ------------- | --------------------------------------------------------------- |
| `v`              | ✔️            | Defines vertex positions (treated as 3D coordinates in meters). |
| `f`              | ✔️            | Defines polygonal faces. Each face becomes a `CWall`.           |
| `o`              | ✔️ (optional) | If multiple objects exist, **only the first one** is used.      |
| `usemtl`         | ✔️            | Associates a material with each face (if available).            |
| `g`, `s`, others | ❌ ignored     | Groups, smoothing groups and other OBJ directives are ignored.  |

### 1.2 Coordinate System and Units

* Vertex coordinates map **1:1** to the `CWall` and `CRoom` structures.
* **No scaling, axis reordering, or transformation** is applied.
* Units are assumed to be **meters**.

### 1.3 Geometry Interpretation

Once parsed by rapidobj, the loader converts:

* All vertex positions → internal room vertex list.
* Each face (triangles, quads, or n-gons) → a `CWall` polygon.

#### Important Requirements (from CWall)

Each wall must satisfy the constraints of the `CWall` class:

* Vertices are expected in **counter-clockwise order as seen from inside the room**. 
* Walls must be **planar**; if a polygon is slightly non-planar, the first three vertex define the wall's plane and extra vertices are projected onto the wall’s plane automatically.
* Walls are assumed to be **convex polygons**.

⚠️ **It is the responsibility of the OBJ file author to ensure that the room is geometrically valid** (closed, convex, and properly oriented). Non-convex or open meshes may result in incorrect simulation behaviour.

#### Recommendation on Vertex Count

* Although any number of vertices per face is supported, **we recommend defining walls using exactly four vertices** (like architectural room faces).
* Performance or acoustic correctness with highly tessellated meshes has **not been tested**.

### 1.4 Multiple Meshes and Objects

* If the OBJ file contains multiple meshes/shapes, **only the first one is processed**.
* If no mesh is found, loading fails.

### 1.5 Room Origin and Orientation

* Any coordinate origin is acceptable; the room does **not** need to be centered at (0,0,0).
* No attempt is made to reposition or normalize the geometry.

---

## 2. MTL File Requirements and Acoustic Properties

The loader supports standard MTL file structure but only uses one custom acoustic directive.

### 2.1 Supported MTL Directive

#### Custom Directive: Acoustic Absorption Coefficients

Each material may contain the BRT-specific directive:

```
X-acoustic-coeffs a1 a2 a3 a4 a5 a6 a7 a8 a9
```

Where the nine floating-point values represent octave-band absorption coefficients at:  **62.5 Hz**, **125 Hz**, **250 Hz**, **500 Hz**, **1 kHz**, **2 kHz**, **4 kHz**, **8 kHz**
* **16 kHz**. All coefficients must be within **[0, 1]**.

These values are assigned to the wall via:

```
CRoom::SetWallAbsortion(faceIndex, coefficientsVector)
```

### 2.2 Unsupported Standard MTL Properties

All standard optical or texture-related MTL properties are **ignored** (e.g., colors, reflectivity, texture maps).

### 2.3 Material Assignment Rules

* Materials may be assigned per face using `usemtl`.
* If a face has **no material** or the material does not define `X-acoustic-coeffs`, the wall defaults to **zero absorption in all bands**.

### 2.4 Error Handling

* Missing MTL file: no error, but all walls get default absorption (0).
* Malformed or incomplete `X-acoustic-coeffs`: the directive is ignored and defaults applied.
* Values outside [0,1] are rejected and replaced with default absorption.

---

## 3. Conversion to the Room Model

Once the OBJ and MTL content has been parsed, a room (CRoom) is created and the corresponding absorption coefficients are applied to each wall of the room. All walls are **active** by default, unless they are subsequently deactivated manually.

---

## 4. Summary of Constraints

To ensure correct binaural reverberation simulations:

* OBJ must describe a **closed, convex room**.
* Faces must be oriented **counter-clockwise from the inside**.
* Use simple geometry (preferably four-vertex walls).
* Optional MTL files may specify acoustic absorption per face.
* All walls default to **no absorption** if no materials are specified.
