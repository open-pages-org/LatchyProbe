# Klipper macros


The macros are similar to those for other docable probes [here](docs/Dockable_Probe.md)

The standard macros do not include the deployment and retration of the probe so these actions can be added to  macro as needing using the following format:

The example macros below are for the CR10v2, the positions will need to be changed to suit your printer configuation

Homing the printer with the probe extended may damage the probe if it dragged sideways on the bed, home x and y first before homing z. Before homing it may be helpful to lift the print head 5mm to ensure the probe is clear of the surface before homing.

[gcode_macro MACRO_NAME]
rename_existing: _MACRO_NAME # Renames the existing macro with and underscore prefix
gcode:
  PROBE_OUT  #Deploy the probe
  _MACRO_NAME #Call the original macro that was renamed
  PROBE_IN  #Retract the probe

The movements to retract and demply the probe are identicle however there are spearate macros for PROBE_OUT and PROBE_IN that check the current status of the probe before perforning the action.

Note also that due to the way templates are evaluated we have separate macros for checking status and setting a global variable prior to the if statements.

## Include the configuration for the probe details

``` CR10v2 Probe configuration
[probe]
pin: ^PD2 #Probe pin for CR10v2 2.5.2 Board (Uses the BLTouch connections)
x_offset: 50 #probe to nozzle x distance
y_offset: -3.4 #probe to nozzle y distance
#z_offset: -6 #probe to nozzle z distance, this gets overwritten by the callibration macro
speed: 5
lift_speed: 50.0
samples:2
samples_result: median
sample_retract_dist: 1.0
samples_tolerance: 0.01
samples_tolerance_retries: 6
```

## Include the configuration for the bed mesh settings, adjust to allow for bed size and probe offset from the nozzle:
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

## This configuration is used for levelling using the probe to measure the bed height above each corner screw. 
Change the X, Y positions and screw thread type to suit your printer.

``` Include the configuration for screw tile adjust
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

# This checks the probe status and the triggers the action

[gcode_macro PROBE_OUT]
gcode:
  G90
  G1 Z20 #Move up to ensure probe is clear of bed
  G4 P300
  Query_Probe
  # If not triggered (open), probe is extended and the probe circuit is complete ready to probe
    {% if Query_Probe == 'open' %}
      RESPOND TYPE=command MSG='1 Probe is already deployed'
    {% else %}
      CYCLE_PROBE
      Query_Probe
      # Check to see if deployment successful with probe extended and the probe circuit is complete ready to probe
        {% if not Query_Probe_triggered %}
        RESPOND TYPE=command MSG='2 Probe is now deployed'
        {% else %}
        RESPOND TYPE=command MSG='3 Probe did not deploy'
        # { action_raise_error("Probe deployment failed!") }
        {% endif %}
    {% endif %}

```macro
 dock_position: 300, 295, 0
```
