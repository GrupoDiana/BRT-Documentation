# Safety Limiter

## Importance of Setting a Maximum Signal Level  

In spatial audio applications, ensuring safe listening levels is crucial to preventing hearing damage and avoiding unintended excessive sound levels. Prolonged exposure to high sound pressure levels (SPL) can cause hearing fatigue or even permanent hearing loss. Additionally, sudden loud sounds can be uncomfortable or startling for users, potentially compromising the immersive experience. To mitigate these risks, BeRTA includes a **Safety Limiter** that allows users to define a maximum SPL threshold, preventing output levels from exceeding safe limits.  

## Functionality  

Once the calibration process has been completed, users can configure a **safety control** in BeRTA. This control continuously monitors the signal level in real time. If the signal exceeds the predefined safety threshold, BeRTA automatically limits it to the maximum allowed level, displays a warning in the graphical interface, and sends an alert command to notify connected clients.  

The safety threshold is set using the **setSoundLevelLimit** OSC command, where the user specifies the maximum allowed SPL level. For further details, refer to the [OSC Commands](/BRT-Documentation/osc/control/#safety-limiter) section.  

When the signal surpasses the limit, BeRTA sends an alert message via the **soundLevelAlert** OSC command to all clients that have established an OSC connection with the BeRTA Renderer. This ensures that external applications are immediately informed of potential safety violations.  

By implementing the Safety Limiter, BeRTA helps maintain a controlled and safe listening environment, protecting users while ensuring consistent playback levels across different use cases.  
