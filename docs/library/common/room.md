# Room

The Room class provides tools to define, configure, and manipulate the geometry and properties of a 3D room for audio rendering simulations. It supports both predefined shapes, like shoebox rooms, and custom geometries by defining individual walls and their properties. The class also includes functionality to manage wall properties, such as absorption coefficients, and to check spatial relationships within the room. Below is a summary of its key capabilities:

- **Room Initialization**:
The class can initialize a room as a shoebox by specifying its length, width, and height, or as a custom geometry using a list of vertices and wall definitions.

- **Wall Management**:
Individual walls can be added, retrieved, activated, deactivated (transparent), or modified in terms of their absorption properties (frequency-dependent or independent).

- **Acoustic Properties**:
Walls' absorption coefficients can be adjusted, and the entire room's walls can be configured simultaneously for consistent acoustic behavior.

- **Geometric Queries**:
The class supports querying whether a point is inside the room, finding the center of the room, and creating "image rooms" for reflections based on active walls.

- **Room Size and Shape**:
If the room is a shoebox, its dimensions can be retrieved. Additionally, the center of the room can be calculated as an average of its wall centers.

This class is integral to defining the environment for spatial audio rendering by allowing precise control over room geometry and acoustics.