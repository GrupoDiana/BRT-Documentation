# Common
:warning:*(Section under construction)*:warning:

- **Ambisonic Encoder**:  
  Encodes sound sources into an ambisonic format for spatial audio processing and rendering.  

- **Binaural Filter**:  


- **Buffer**:  
  Manages temporary storage of audio data during processing, ensuring efficient data flow through the renderer.  

- **Conventions**:  
  Defines and enforces data format standards and spatial audio conventions, such as coordinate systems or naming conventions.  

- **Cranial Geometry**:  
  Models the geometry of the human head to enhance spatial audio accuracy by accounting for head-related effects.  

- **Distance Attenuator**:  
  Implements distance-based attenuation to simulate the natural decrease in sound intensity over distance.  

- **Envelope Detector**:  
  Extracts the amplitude envelope of an audio signal.  

- **Error Handler**:  
  Manages error detection and reporting during audio processing to ensure robustness and debugging support.  

- **FFSG**:  
  Implement the fast Fourier transforms  by Copyright(C) 1996-2001 Takuya OOURA.  

- **Filters Chain**:  
  A modular pipeline that allows chaining multiple audio filters for sequential processing of sound signals.  

- **F Processor**:  
  Audio signal processor that calculates FFTs and IFFTs.  

- **IR Windowing**:  
  Applies a window function to impulse responses (IRs).  

- **Profiler**:  
  Monitors and measures the performance of various components within the renderer, tracking the CPU usage of the rendering process.  

- **Quaternion**:  
  Represents rotational transformations in 3D space, crucial for accurately positioning sound sources and listeners.  

- **[Room](room.md)**:  
Enable the creation and management of 3D room geometries and acoustic properties, supporting predefined and custom shapes, wall configurations, and spatial queries for audio rendering simulations.  

- **Transform**:  
  Handles spatial transformations like translation, rotation, and scaling for positioning sources or listeners.  

- **[Uniform Partitioned Convolution](uniform-partitioned-convolution.md)**:  
  Implements efficient frequency-domain convolution using uniformly partitioned overlap-save methods, optimized for long impulse responses.  

- **Vector3**:  
  Represents 3D vectors, often used for positions, directions, or velocity in spatial audio calculations.  

- **Wall**:  
  Models a physical wall to simulate reflections and obstructions in room acoustics.  

- **Wave Guide**:  
  Simulates wave propagation in 3D space, often used for modeling sound in complex environments.  
