# Klipper macros
The example macros below are for the CR10v2, the positions will need to be changed to suit your printer configuration. These macros have been developed based on examples from Klicky, Unclicky, Euclid, etc., further printer examples will be added as they are developed.

The standard Klipper macros do not include the deployment and retraction of the probe so these actions can be added to macro as needing using the following format:

``` general format
[gcode_macro MACRO_NAME]
rename_existing: _MACRO_NAME # Renames the existing macro with and underscore prefix
gcode:
  PROBE_EXTEND  #Deploy the probe
  _MACRO_NAME #Call the original macro that was renamed
  PROBE_RETRACT  #Retract the probe
```

Homing the printer with the probe extended may damage the probe if it dragged sideways on the bed. Use 5mm z hop and home x and y first before homing z.

``` Safe Z Home
[safe_z_home]
home_xy_position: 0,0 # Change coordinates to the preferred location on your print bed
speed: 50
z_hop: 5 # Move up 5mm
z_hop_speed: 5
```

# The following macros control the probe deployment and motion
The movements to retract and extend the probe are identical however there are separate macros for PROBE_EXTEND and PROBE_RETRACT that check the status of the probe before performing the action. Note also, due to the way templates are evaluated, separate macros are needed for checking the status to the macros before performing actions.

For example, before running a bed mesh calibrate use Probe_Extend and then afterward use PROBE_RETRACT before printing.

The following macros are all needed and work together when using the probe. 
- PROBE_EXTEND
- PROBE_RETRACT
- MOVE_PROBE_CLEAR
- SET_PROBE
- CYCLE_PROBE

## Macro to prepare the probe ready for use
This will initiate all steps needed to use the probe.
```PROBE_EXTEND
[gcode_macro PROBE_EXTEND]
gcode:
  MOVE_PROBE_CLEAR # ensure the probe is not pressed in by the bed before checking status
  Query_Probe #get the current probe status
  SET_PROBE ACTION=extend
```
## Macro to retract the probe after use
This will retract the probe after use but can also be used after a printer restart or before printing to ensure the probe is retracted.
```PROBE_RETRACT
[gcode_macro PROBE_RETRACT]
gcode:
  MOVE_PROBE_CLEAR # ensure the probe is not pressed in by the bed before checking status
  Query_Probe #get the current probe status
  SET_PROBE ACTION=retract
```

## Macro to move the head away from the bed before checking the probe
This makes sure the probe is not pressed so the Query_Probe result can be used to check the latched position of the probe. 
It also makes sure the printer is homed before using the probe.
```MOVE_PROBE_CLEAR
[gcode_macro MOVE_PROBE_CLEAR]
gcode:
  {% if not 'xyz' in printer.toolhead.homed_axes %}   # Check if already homed
      RESPOND MSG='Homing first'
      G28
  {% endif %}
  {% if printer.toolhead.position.z < 8 %}   # Check if the current Z position is less than 8mm
        RESPOND MSG="Current Z position: { printer.toolhead.position.z } - Moving up by 8mm"
        G91 ; Set to relative positioning
        G1 Z8 ; # Move up 8mm to clear probe extension which is approximately 6mm
        G90 ; Set back to absolute positioning
  {% endif %}
  RESPOND MSG="Current Z position: { printer.toolhead.position.z } - Probe is clear of bed"
```

## Macro to set the probe to either retract or extend
This will check the current probe setting and call for the probe to be cycled if needed. 
Query_Probe must be used before this macro and then use 'SET_PROBE ACTION=retract' or 'SET_PROBE ACTION=extend'
```SET_PROBE
[gcode_macro SET_PROBE]
gcode:
  {% set probe_action = params.ACTION %}
  {% if probe_action == "extend" %}
      {% if printer.probe.last_query == 0 %}
        RESPOND MSG=" Probe is already extended! " ; Send message to console
      {% else %}
        RESPOND MSG=" Extending probe " ; Send message to console
        CYCLE_PROBE
      {% endif %}
  {% elif probe_action == "retract" %}
    {% if printer.probe.last_query == 1 %}
        RESPOND MSG=" Probe is already retracted! " ; Send message to console
    {% else %}
        RESPOND MSG=" Retracting probe! " ; Send message to console
        CYCLE_PROBE
    {% endif %}
  {% endif %}
  Query_Probe
 ```
       
 ## Macro to push the probe onto the trigger post to retract or extend
 This will cycle the probe from one state to the other.
```CYCLE_PROBE
[gcode_macro CYCLE_PROBE]
gcode:
  {% if not 'xy' in printer.toolhead.homed_axes %}   # If x and y are not homed
      { action_raise_error("Must Home X and Y Axis First!") }
  {% endif %}
  G90 # Set absolute positioning
  G1 Z20 # set Z height clear of trigger post
  G1 X310 F4000 # move x position over trigger post (include y position if needed)
  G1 Z3.5 # move Z down to the latching point (obtain this value by setting the probe over the trigger post and jogging z down until the click is heard and subtract 0.5mm for reliability)
  G4 P100 Pause
  G1 Z20 # set Z height clear of trigger post
  G4 P100 Pause
  G1 X100 F4000 # move x position to middle of bed or other preferred start point
```

# Modifications to standard Klipper Functions
The following macros modify the existing functions to include the EXTEND and RETRACT process.

## Macro for bed mesh calibrate
```BED_MESH_CALIBRATE
[gcode_macro BED_MESH_CALIBRATE]
rename_existing: _BED_MESH_CALIBRATE
gcode:
  PROBE_EXTEND
  _BED_MESH_CALIBRATE
  PROBE_RETRACT
```

## Macro for probe callibration
```PROBE_CALIBRATE
[gcode_macro PROBE_CALIBRATE]
rename_existing: _PROBE_CALIBRATE
gcode:
  PROBE_EXTEND
  G90
  G1 Z20
  G1 X115 Y115 F20000
  _PROBE_CALIBRATE
  TESTZ Z=20
  PROBE_RETRACT
```

## Macro for checking the probe accuracy
```PROBE_ACCURACY
[gcode_macro PROBE_ACCURACY]
rename_existing: _PROBE_ACCURACY
gcode:
  PROBE_EXTEND
  G90
  G1 Y115 X115 F20000
  _PROBE_ACCURACY
  PROBE_RETRACT
```

## Macro for adjust z tilt 
Note, this is if you have a setup that allows independent control of z motors, this does not apply for CR10v2.
```Z_TILT_ADJUST
[gcode_macro Z_TILT_ADJUST]
rename_existing: _Z_TILT_ADJUST
gcode:
PROBE_EXTEND
Z_TILT_ADJUST
PROBE_RETRACT
```

# Printer and probe configuration details
The following configurations also need to be added to Printer.cfg when adding a probe

# Configuration of the probe
This is based on the configuration for the CR10v2, change the settings for other printers
``` CR10v2 Probe configuration
[probe]
pin: ^PD2 #Probe pin for CR10v2 2.5.2 Board (Uses the BLTouch connections)
x_offset: 50 #probe to nozzle x distance
y_offset: -3.4 #probe to nozzle y distance
#z_offset: -6 #probe to nozzle z distance, this gets overwritten by the calibration macro
speed: 5
lift_speed: 50.0
samples:2
samples_result: median
sample_retract_dist: 1.0
samples_tolerance: 0.01
samples_tolerance_retries: 6
```

## Configuration for the bed mesh settings
Change the setting for mesh size and coverage to suit your printer.
``` bed mesh configuration
[bed_mesh]
speed: 100
horizontal_move_z: 5  #Height of the Z above the bed during the horizontal moves between points
mesh_min: 50,5
mesh_max: 250,290
probe_count: 5,5
algorithm: bicubic
fade_start: 1
fade_end: 10
#fade_target:
#   The z position in which fade should converge. When this value is set
#   to a non-zero value it must be within the range of z-values in the mesh.
#   Users that wish to converge to the z homing position should set this to 0.
#   Default is the average z value of the mesh.
split_delta_z: 0.015
#   The amount of Z difference (in mm) along a move that will
#   trigger a split. Default is .025.
move_check_distance: 3
#   The distance (in mm) along a move to check for split_delta_z.
#   This is also the minimum length that a move can be split. Default
#   is 5.0.
mesh_pps: 4,4
#   A comma separated pair of integers (X,Y) defining the number of
#   points per segment to interpolate in the mesh along each axis. A
#   "segment" can be defined as the space between each probed
#   point. The user may enter a single value which will be applied
#   to both axes.  Default is 2,2.
#bicubic_tension: .2
#   When using the bicubic algorithm the tension parameter above
#   may be applied to change the amount of slope interpolated.
#   Larger numbers will increase the amount of slope, which
#   results in more curvature in the mesh. Default is .2.
```

## Configuration for bed screw tilt adjustment
This is used when measuring the bed height above each corner screw for manual adjustment. Change the X, Y positions and screw thread type to suit your printer.
``` screw tile adjust
[screws_tilt_adjust]
screw1: 0,29
screw1_name: front left screw
screw2: 228,29
screw2_name: front right screw
screw3: 228,269
screw3_name: rear right screw
screw4: 0,269
screw4_name: rear left screw
speed: 50
horizontal_move_z: 10
screw_thread: CW-M3
```

