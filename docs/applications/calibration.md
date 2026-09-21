# Calibration

BeRTA provides a calibration procedure that establishes a relationship between digital signal levels in dBFS and measured sound pressure levels in dBSPL.

Calibration is associated with a **physical stereo output pair** of the selected audio interface. Each pair stores its own calibration reference, independently of the listener selected in the GUI. Once a pair is calibrated, BeRTA can use this reference to estimate sound pressure levels from digital signal levels. These values are estimates based on the playback chain used during calibration, not continuous acoustic measurements.

Calibration does not automatically adjust the playback hardware or guarantee a particular perceived loudness.

## Output pairs and calibration references

Output channels are numbered from **0** and grouped into stereo pairs:

| Left channel argument | Output pair |
|-----------------------|-------------|
| `0` | 0–1 |
| `2` | 2–3 |
| `4` | 4–5 |

Calibration commands require the left channel of an available pair. The value must be a non-negative even number, and both channels must be enabled in **Settings → Audio Interfaces**.

The requested pair is used independently of the listener selected in the GUI. A pair can be calibrated even when no listener is assigned to it.

BeRTA currently stores **one calibration relationship per stereo pair**, shared by its left and right channels. It does not store independent calibration offsets for the two sides.

## Calibration relationship

A calibration reference contains:

- The digital RMS level used to reproduce the reference signal, in dBFS.
- The corresponding sound pressure level measured externally, in dBSPL.

BeRTA calculates the calibration offset as:

```text
calibration offset = reference dBSPL − reference dBFS
```

During normal operation, the estimated sound pressure level is:

```text
estimated dBSPL = signal dBFS + calibration offset
```

For example, if a reference reproduced at −40 dBFS produces a measured level of 60 dBSPL:

```text
calibration offset = 60 − (−40) = 100 dB
```

A subsequent signal at −30 dBFS therefore corresponds to an estimated level of 70 dBSPL, before any physical output limiting is applied.

## Calibration process in BeRTA

The procedure consists of reproducing a reference signal, measuring its acoustic output, storing the relationship and optionally verifying it.

Choose the output device, enabled channels and playback hardware before starting. Keep the hardware gain and volume settings unchanged throughout calibration and subsequent use.

### 1. Play the reference signal

Send `/control/playCalibration` with the desired digital RMS level and the left channel of the output pair.

For example:

```text
/control/playCalibration -40 0
```

This starts the reference signal at **−40 dBFS** on output channels **0–1**.

BeRTA plays `resources/Calibration/CalibrationPinkNoise.wav` in a continuous loop. It reads the file's left channel and sends identical samples to both channels of the selected output pair. Other output channels remain silent.

When loading the reference file, BeRTA:

- Checks that its sample rate matches the application's configured sample rate.
- Calculates the RMS of its left channel to determine the required playback gain.
- Checks that the requested gain will not produce digital clipping.

If these checks fail, the playback request is rejected.

!!! note "Reference signal"
    The reference is reproduced directly through the selected output pair. It does not pass through listener spatialisation or the normal scene-processing chain.


### 2. Measure the acoustic output

Measure the reference signal using a sound level meter and a measurement arrangement appropriate to the playback transducer. 

Record the measured sound pressure level and the digital level used in the previous step. Keep the measurement settings and physical arrangement consistent when verifying the calibration. For headphones, the microphone placement and coupling to the earpiece affect the result. The calibration reference represents the conditions under which that measurement was made.

!!! note "Shared calibration for both channels"
    Both channels receive identical digital samples, but the corresponding acoustic levels may differ because of the playback hardware.

    BeRTA applies one calibration offset to both channels of the pair. A measurement taken from one side does not independently calibrate or compensate the other side. Check both sides when their agreement matters to the application.


### 3. Store the calibration

Send `/control/setCalibration` with:

1. The digital RMS level used for the reference.
2. The measured sound pressure level.
3. The left channel of the same output pair.

For example, if the reference played at −40 dBFS was measured at 60 dBSPL:

```text
/control/setCalibration -40 60 0
```

BeRTA stores this relationship for channels **0–1**, stops calibration playback and exits calibration mode.

Starting or stopping reference playback does not replace an existing calibration. The stored values change when `/control/setCalibration` is applied.

### 4. Verify the calibration

Once a calibration is available, use `/control/playCalibrationTest` to request a reference level in dBSPL:

```text
/control/playCalibrationTest 60 0
```

BeRTA converts the requested level to dBFS using the stored calibration and reproduces the reference signal on channels **0–1**.

Measure the acoustic output externally and compare it with the requested level.

The command requires an existing calibration for the selected pair. A requested level that would cause digital clipping is rejected.

### 5. Stop the test

To stop reference or verification playback, send the left channel of the pair currently being tested:

```text
/control/stopCalibrationTest 0
```

The GUI Stop control and the space bar also stop calibration playback.

Stopping the test preserves the stored calibration. Normal scene playback does not resume automatically.

For argument types and response formats, see the [OSC calibration commands](/BRT-Documentation/osc/control/#calibration).

!!! info "Command acknowledgement"
    A successful OSC response confirms that BeRTA accepted and performed the software operation. It does not confirm the accuracy of the external acoustic measurement.

## Behaviour during calibration

Calibration playback is an exclusive operating mode:

- Only one output pair can be tested at a time.
- Starting reference or verification playback stops normal scene playback.
- New playback and recording operations are blocked until calibration mode ends.
- Calibration playback cannot start while a recording is in progress.
- A new calibration playback request interrupts the previous test. If the new request fails, the previous test is not resumed.
- Applying `/control/setCalibration` ends calibration playback.
- Stopping calibration does not automatically resume the scene.


### Safety limiter and measurements

The normal scene-processing path is bypassed during both reference playback and verification playback.

Consequently:

- The physical safety limiter does not act on the calibration signal.
- Normal output-level measurements are paused.
- The output VUmeters do not display the calibration signal.
- Listener measurements do not represent the calibration signal.

The reference-file RMS calculation performed when loading the WAV is used to set playback gain. It is separate from the normal live measurements, which remain paused during calibration.

!!! warning "Calibration playback bypasses the safety limiter"
    The sound-level limit configured for normal scene playback does not constrain reference or verification playback.

    Choose the requested test level deliberately. BeRTA's digital clipping check does not establish an acoustic exposure limit.

The existing calibration data remain stored while a test is running. They are replaced only when new calibration values are applied.


## How listeners use calibration

Calibration belongs to an output pair. Listeners obtain the calibration used for their theoretical dBSPL measurements through their output configuration.

| Listener configuration | Calibration used |
|------------------------|------------------|
| Routed, Active | Its assigned output pair |
| Routed, Standby | Its assigned output pair |
| Virtual with a valid reference | The current output pair of the referenced Routed listener |
| Virtual without a reference | No calibration available |

For example:

```text
Virtual listener B → Routed listener A → output pair 0–1 → calibration
```

Listener B uses **its own digital signal level** with the calibration of channels 0–1. It does not use Listener A's signal level.

The reference follows Listener A. If A is reassigned to channels 2–3, B uses the calibration of channels 2–3 after the routing change is applied.

A may be in Active or Standby: its routing state does not affect its ability to provide a calibration reference.

Virtual listeners without a valid calibration reference still provide digital RMS measurements in dBFS. Their theoretical dBSPL measurements are unavailable.

These references are configured in [Settings → Listeners](/BRT-Documentation/applications/berta-renderer/settings-menu/#listeners).

## Monitoring sound levels

BeRTA distinguishes between digital output metering, estimated physical output levels and listener signal levels.

### Output VUmeters

The multichannel output VUmeters display the digital envelope level of each enabled output channel, in dBFS.

- The measurement is taken after the physical safety limiter.
- Digital metering does not require calibration.
- The meters are independent of the listener selected in the GUI.
- The envelope detector provides the meter's attack and release behaviour; it is not the same measurement as listener RMS.

During calibration playback, the meters show a paused indication instead of measuring the reference signal.

### Output Status

The **Output pairs - dBSPL** section displays one row per physical output pair:

| Column | Meaning |
|--------|---------|
| **Pair** | Physical output pair, such as `0–1` |
| **Cal** | `OK` when calibration is available, or `No` otherwise |
| **Level** | Estimated left / right output levels in dBSPL, after limiting |

Amber levels followed by `*` indicate that the safety limiter is acting. The numeric levels remain visible.

Hover over a pair for its calibration reference, offset and available limiter information. When limiting is active, the tooltip also includes pre-limiter levels.

The display distinguishes:

- `-inf`: a valid silent signal.
- `--`: an unavailable measurement.
- `TEST`: calibration playback is active on this pair.
- `Paused`: calibration playback is active on another pair and normal measurements are paused.

Calibration availability and measurement availability are separate. A calibrated pair may temporarily have no current measurement. A known silent output may be reported as silence even without calibration.

### OSC queries

The former `/control/getSoundLevel` command has been removed. Use the following queries:

| Command | Measurement | Calibration required | Includes physical limiting |
|---------|-------------|----------------------|----------------------------|
| `/control/getOutputSoundLeveldBSPL` | Estimated physical output level for a stereo pair | Required for a non-silent signal | Yes |
| `/control/getListenerSoundLeveldBSPL` | Theoretical level calculated from the listener's own RMS signal level | Yes, directly or through a reference | No |
| `/control/getListenerSoundLeveldBFS` | The listener's own digital RMS level | No | No |

The Active/Standby state does not affect a listener's own RMS measurement or its theoretical dBSPL calculation.

A listener's theoretical dBSPL level may exceed the physical safety limit. The limiter changes the signal sent to the physical output, not the listener's theoretical measurement.

Each query response includes the requested identifier, a `valid` flag, left and right levels, and an error string.

- `valid=true` indicates an available measurement. A silent channel may have a level of negative infinity.
- `valid=false` indicates an unavailable measurement. Inspect the error string rather than interpreting the numeric fields as silence.

During calibration playback, the output-level query for the pair being tested reports that the measurement is unavailable.

See [OSC sound-level commands](/BRT-Documentation/osc/control/#sound-levels) for complete syntax and examples.

## Interpretation and limitations

BeRTA's dBSPL values are estimates derived from a digital signal measurement and a stored calibration offset. They are not live acoustic measurements and do not directly quantify perceived loudness.

Their agreement with an external measurement depends on factors including:

- **Playback gain:** Changes to interface volume, amplifier gain or other gain stages alter the relationship established during calibration.
- **Playback hardware:** Replacing the interface, amplifier, headphones or loudspeakers may require a new calibration.
- **Frequency response:** A single calibration offset does not compensate for frequency-dependent differences between the reference signal and other audio.
- **Measurement conditions:** Meter weighting, microphone placement, headphone coupling and the acoustic environment affect the reference measurement.
- **Non-linear behaviour:** Clipping, saturation and distortion in the playback chain can invalidate the assumed relationship between digital and acoustic levels.
- **Left/right differences:** A shared stereo-pair calibration does not independently compensate differences between its two physical channels.

Keep the playback configuration consistent with the conditions used for calibration. Recheck the acoustic output after relevant hardware, gain or measurement changes.

Calibration provides a reproducible level reference within those conditions. External measurements remain necessary to establish and verify that reference.
