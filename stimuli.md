---
layout: page
title: Stimuli
nav_order: 4
---

# Stimulus File

Each stimulus file is designed to be easily created by any user. It is a CSV file with a specific structure and content that depends on the intended stimulus. Typically, one stimulus file contains only one type of stimulus object (e.g., just circles or just bars), but it can also be used to mix different stimulus types (stimulus objects), which can be individually referenced by specific stimulus functions.

All stimulus files consist of two sections. One section contains general stimulus parameters, which apply to all stimulus epochs, while the other section is epoch-specific, applying to each epoch individually and varying based on the type of stimulus (e.g., circles, bars, gratings, etc.). Below, the general parameters are explained. For explanations of the epoch-specific parameters, refer to the specific stimulus functions.

### General Parameters:

- **EPOCHS**: total number of epochs.  
- **MAXRUNTIME**: to define a “search” vs a “normal” stimulus file to be used during a recording. (For search, set this to `0`; for the recording file, set it to any reasonable number of seconds, e.g., `3600`, that the recording would maximum take. It does not need to be exact. Usually, it oversees it.)
- **STIMULUSDATA**: if the stimulus type needs extra data to be plotted. For example…, otherwise, `NULL`.
- **PERSPECTIVE_CORRECTION**: if perspective correction is applied `1` or not `0`.
- **RANDOMIZATION_MODE**: if the epochs’ presentation order is pseudorandomized `1 or 2` or not `0`. The randomization option `1` randomizes the epoch presentation by defining the first epoch of the file as an interstimulus epoch. The randomization option `2` shuffles the order of all epochs.

---

# Stimuli Functions

The following functions are used to generate different types of stimuli, located in ["modules/stimuli.py"] of our pyVStim custom package. PyVStim was created as a collaborative project, encouraging users to contribute new functions for additional stimulus types. The list of available stimuli is expected to grow and will need regular updates to include new contributions.

### 1. Full-field Flash

**Brief Description**:  
This stimulus function creates a circle (a PsychoPy stimulus object, `stim_obj`) with a specified size, contrast, position, and duration. Typically, the circle is centered in the middle of the projected rectangular window (the virtual screen, as defined by the function inputs) and is larger, resulting in a full-field flash. For instance, if the rectangular screen spans 80 degrees of the fly's visual field, the circle's size is set to be greater than 80 degrees. However, this function can also be used to draw smaller circles on the screen and position them wherever desired.

### Important Stimulus Parameters

#### General Parameters:
- **EPOCHS**: total number of epochs. E.g., `2` for ON-OFF full-field flashes.
- **MAXRUNTIME**: to define a “search” vs a “normal” stimulus file to be used during a recording.
- **STIMULUSDATA**: if the stimulus type needs extra data to be plotted. In the case of a circle object, no extra data is needed. Set this to `NULL`.
- **PERSPECTIVE_CORRECTION**: if perspective correction is applied `1` or not `0`.
- **RANDOMIZATION_MODE**: if the epochs’ presentation order is pseudorandomized `1 or 2` or not `0`.

<br>

#### Epoch-specific Parameters

| Stimulus.stimtype | circle | circle |
|:-------------------|:--------|:--------|
| Stimulus.bg       | 0      | 1      |
| Stimulus.fg       | 0      | 1      |
| Stimulus.duration | 2      | 2      |
| Stimulus.radius   | 120    | 120    |
| Stimulus.tau      | 0      | 0      |
| Stimulus.x_center | 0      | 0      |
| Stimulus.y_center | 0      | 0      |

<br>

#### List of Acronyms:
- **stimtype**: The type, so choose is `circle`.
- **bg**: Background level of luminance.
- **fg**: Foreground level of luminance.
- **duration**: Total duration (in seconds) of the epoch.
- **radius**: Circle radius in degrees of visual angle.
- **tau**: Time (in seconds) that an epoch shows the foreground luminance during the total `duration`.
- **x_center**: Position of the center (in degrees of visual angle) of the `stim_obj` along the x-axis.
- **y_center**: Position of the center (in degrees of visual angle) of the `stim_obj` along the y-axis.

### Current Stimulus Function:
```python
def field_flash(exp_Info, bg_ls, fg_ls, stim_texture, noise_arr, stimdict, epoch, window, global_clock, duration_clock, outFile, out, stim_obj, dlpOK, viewpos, data, taskHandle=None, lastDataFrame=0, lastDataFrameStartTime=0)
```
-	**exp_Info**: a dictionary containing information about the experiment which must be provided when running pyVStim.
-	**bg_ls or fg_ls**: list of background and foreground luminance for all epochs defined in the stimulus file
-	**stim_texture**: extra STIMULUSDATA that can be provided. Potential use.
-	**noise_arr**: array to add some noise to the data. Potential use.
-	**stimdict**: a dictionary containing all information provided in the stimulus file.
-	**epoch**: current epoch in the stimulation.
-	**window**: the defined virtual screen where all objects are drawn.
-	**global_clock, duration_clock**: clocks to keep track of stimulus duration concerning the start of every epoch.
-	**outFile**: file where the stimulus output will be stored.
-	**out**: the actual stimulus output.
-	**stim_obj**: pre-defined (before calling the stimulus function) PsychoPy stimulus object
-	**dlpOK**: if the second screen (our projector, DLP) is being used.
-	**viewpos**: variables (size and positions) to draw the window (virtual screen) in the monitor space (it refers to the second screen, the DLP)
-	**data**: counter of the microscope frames
-	**lastDataFrame**: counter for the stimulus frames
-	**lastDataFrameStartTime**: time of the stimulus frames


---

### 2. Standing Stripes

**Brief Description**:  
This stimulus function creates bars (a PsychoPy stimulus object, `bar`) with a specified height, width, orientation, contrast, and duration, and presents them in pseudorandomized positions.

#### Important Stimulus Parameters

#### General Parameters:
- **EPOCHS**: total number of epochs. For presenting bars at random positions, we need just `1` epoch.
- **MAXRUNTIME**: to define a “search” vs a “normal” stimulus file to be used during a recording.
- **STIMULUSDATA**: If no extra data is needed, set this to `NULL`.
- **PERSPECTIVE_CORRECTION**: if perspective correction is applied `1` or not `0`.
- **RANDOMIZATION_MODE**: Set to `0` when the total number of EPOCHS = 1.

<br>
#### Epoch-specific Parameters

| Stimulus.stimtype      | stripe(s) |
|:------------           |:-------   |
| Stimulus.bg            | 0.5       |
| Stimulus.fg            | 0         |
| Stimulus.bg.duration   | 1         |
| Stimulus.bar.duration  | 1         |
| Stimulus.bar.width     | 5         |
| Stimulus.bar.height    | 360       |
| Stimulus.bar.orientation | 90      |
| Stimulus.bar.xmin      | -40       |
| Stimulus.bar.xmax      | 40        |
| Stimulus.bar.ymin      | -40       |
| Stimulus.bar.ymax      | 40        |
| Stimulus.bar.distance  | 5         |

<br>

#### List of Acronyms:
- **stimtype**: Choose `stripe(s)`.
- **bg**: Background level of luminance of the window.
- **fg**: Foreground level of luminance of the bar.
- **bar.width**: Width of the stimulus object in degrees of visual angle.
- **bar.height**: Height of the stimulus object in degrees of visual angle.

### Current Stimulus Function:
```python
def standing_stripes_random(exp_Info, bg_ls, fg_ls, stimdict, epoch, window, global_clock, duration_clock, outFile, out, bar, dlpOK, taskHandle=None, data=0, lastDataFrame=0, lastDataFrameStartTime=0)
```
-	**exp_Info**: a dictionary containing information about the experiment which must be provided when running pyVStim.
-	**bg_ls or fg_ls**: list of background and foreground luminance for all epochs defined in the stimulus file
-	**stimdict**: a dictionary containing all information provided in the stimulus file.
-	**epoch**: current epoch in the stimulation.
-	**window**: the defined virtual screen where all objects are drawn.
-	**global_clock, duration_clock**: clocks to keep track of stimulus duration with respect to the start of very epoch.
-	**outFile**: file where the stimulus output will be stored.
-	**out**: the actual stimulus output.
-	**bar**: pre-defined (before calling the stimulus function) PsychoPy stimulus object
-	**dlpOK**: if the second screen (our projector, DLP) is being used.
-	**viewpos**: variables (size and positions) to draw the window (virtual screen) in the monitor space (it refers to the second screen, the DLP)
-	**data**: counter of the microscope frames
-	**lastDataFrame**: counter for the stimulus frames
-	**lastDataFrameStartTime**: time of the stimulus frames



---

### 3. White Noise / Ternary Noise

**Brief Description**:  
This stimulus function generates ternary noise stimulation (using bars or squares) by applying a texture to a PsychoPy grating stimulus object (`stim_texture`).

#### Important Stimulus Parameters

#### General Parameters:
- **EPOCHS**: total number of epochs.
- **MAXRUNTIME**: to define a “search” vs a “normal” stimulus file to be used during a recording.
- **STIMULUSDATA**: For ternary noise, we need a `TERNARY_TEXTURE`.
- **PERSPECTIVE_CORRECTION**: if perspective correction is applied `1` or not `0`.
- **RANDOMIZATION_MODE**: Set this to `1` or `2` for pseudorandomized epochs.

<br>
#### Epoch-specific Parameters

| Stimulus.stimtype      | circle | noise |
|:------------------------|:--------|:-------|
| Stimulus.bg            | 0.5    | 0     |
| Stimulus.fg            | 0.5    | 0     |
| Stimulus.texture.duration | 0    | 0.05  |
| Stimulus.texture.count | 0      | 10000 |
| Stimulus.texture.hor_size | 0    | 1     |
| Stimulus.texture.vert_size | 0   | 16    |
| Stimulus.duration      | 2      | 0     |
| Stimulus.radius        | 120    | 0     |
| Stimulus.tau           | 2      | 0     |
| Stimulus.x_center      | 0      | 0     |
| Stimulus.y_center      | 0      | 0     |

<br>

### Current Stimulus Function:
```python
def stim_noise(exp_Info, bg_ls, stim_texture, stimdict, epoch, window, global_clock, duration_clock, outFile, out, noise, dlpOK, taskHandle=None, data=0, lastDataFrame=0, lastDataFrameStartTime=0

)
```

-	**exp_Info**: a dictionary containing information about the experiment which must be provided when running pyVStim.
-	**bg_ls or fg_ls**: list of background and foreground luminance for all epochs defined in the stimulus file
-	**stim_texture**: the texture with all the ternary noise frame information.
-	**stimdict**: a dictionary containing all information provided in the stimulus file.
-	**epoch**: current epoch in the stimulation.
-	**window**: the defined virtual screen where all objects are drawn.
-	**global_clock, duration_clock**: clocks to keep track of stimulus duration concerning the start of every epoch.
-	**outFile**: file where the stimulus output will be stored.
-	**out**: the actual stimulus output.
-	**noise**: pre-defined (before calling the stimulus function) PsychoPy stimulus object
-	**dlpOK**: if the second screen (our projector, DLP) is being used.
-	**viewpos**: variables (size and positions) to draw the window (virtual screen) in the monitor space (it refers to the second screen, the DLP)
-	**data**: counter of the microscope frames
-	**lastDataFrame**: counter for the stimulus frames
-	**lastDataFrameStartTime**: time of the stimulus frames


