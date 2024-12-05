# Common
:warning:*(Ready for review)*:warning:

- **Biquad filter**: Provides tools for applying second-order IIR filtering to audio signals. It supports filter setup using specified coefficients.
- **Buffer**: Manages temporary storage of audio data during processing, ensuring efficient data flow through the renderer.  
- **Conventions**: Defines and enforces data format standards and spatial audio conventions, such as coordinate systems.  
- **Cranial Geometry**: Models the geometry of the human head to enhance spatial audio accuracy by accounting for head-related effects.  
- **Envelope Detector**: Extracts the amplitude envelope of an audio signal.  
- **Error Handler**: Manages error detection and reporting during audio processing to ensure robustness and debugging support.  
- **FFSG**:  Implement the fast Fourier transforms by Takuya OOURA Copyright(C) 1996-2001.  
- **Filters Chain**: A modular pipeline that allows chaining multiple audio filters for sequential processing of sound signals.  
- **FFT calculator**: Audio signal processor that calculates FFTs and IFFTs.  
- **IR Windowing**: Applies a window function to impulse responses (IRs).  
- **Profiler**: Monitors and measures the performance of various components within the renderer, tracking the CPU usage of the rendering process.  
- **Quaternion**: Represents rotational transformations in 3D space, crucial for accurately positioning sound sources and listeners.  
- **Room**: Enable the creation and management of 3D room geometries and acoustic properties, supporting predefined shapes and wall configurations.  
- **Transform**: Handles spatial transformations like translation, rotation, and scaling for positioning sources or listeners.  
- **Vector3**: Represents 3D vectors, often used for positions, directions, or velocity in spatial audio calculations.  
- **Wall**: Models a physical wall to simulate reflections and obstructions in room acoustics.  
- **Wave Guide**: Simulates wave propagation in 3D space, often used for modeling sound in complex environments.  
