# Klipper macros


The macros are similar to those for other docable probes [here](docs/Dockable_Probe.md)

The standard macros do not include the deployment and retration of the probe so these actions can be added to  macro as needing using the following format:

[gcode_macro MACRO_NAME]
rename_existing: _MACRO_NAME # Renames the existing macro with and underscore prefix
gcode:
  PROBE_OUT  #Deploy the probe
  _MACRO_NAME #Call the original macro that was renamed
  PROBE_IN  #Retract the probe
  
The example macros below are for the CR10v2, the positions will need to be changed to suit your printer configuation

Homing the printer with the probe extended may damage the probe if it dragged sideways on the bed, home x and y first before homing z. Before homing it may be helpful to lift the print head 5mm to ensure the probe is clear of the surface before homing.

[safe_z_home]
home_xy_position: 0,0 # Change coordinates to the center of your print bed
speed: 50
z_hop: 5 # Move up 5mm
z_hop_speed: 5

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

# These macros control the probe deployment

## Macro to deploy the probe
[gcode_macro PROBE_OUT]
gcode:
MOVE_PROBE_TO_FREE_SPACE
{% set probe_status = "need_to_deploy" %}
Query_Probe #get the current probe status
SET_PROBE



[gcode_macro MOVE_PROBE_TO_FREE_SPACE]
gcode:
  RESPOND MSG='Checking if homed'
  {% if not 'xyz' in printer.toolhead.homed_axes %}   # Check if already homed
      RESPOND MSG='Homing'
      G28
  {% endif %}
  {% if printer.toolhead.position.z < 8 %}   # Check if the current Z position is less than 8mm
        RESPOND MSG="Current Z position: { printer.toolhead.position.z } - Moving up by 8mm"
        G91 ; Set to relative positioning
        G1 Z8 ; # Move up 8mm to clear probe extension which is approximately 6mm
        G90 ; Set back to absolute positioning
   {% else %}
        RESPOND MSG="Current Z position: { printer.toolhead.position.z } - Z is already above 8mm"
   {% endif %}

[gcode_macro SET_PROBE]
gcode:
  {% endif %}
  {% if probe.status == "need_to_deploy" %}
      {% if printer.probe.last_query == "0" %}
        RESPOND MSG="Probe is already deployed!" ; Send message to console
      {% else %}
        RESPOND MSG="Deploying Probe!" ; Send message to console
        CYCLE_PROBE
      {% endif %}
  {% endif %}
  {% if probe.status == "need_to_retract" %}
    {% if printer.probe.last_query == "1" %}
        RESPOND MSG="Probe is already retracted!" ; Send message to console
    {% else %}
        RESPOND MSG="Retracting Probe!" ; Send message to console
        CYCLE_PROBE
    {% endif %}
  {% endif %}

        
    # {% set query_probe_triggered = printer.probe.last_query %}
    RESPOND MSG={test_output}
    {% if printer.probe.last_query == "0" %}
        RESPOND MSG="Probe is deployed!" ; Send message to console
    {% else %}
        RESPOND MSG="Probe is not deployed!" ; Send message to console
    {% endif %}

  [gcode_macro CYCLE_PROBE]
  {% if not 'xyz' in printer.toolhead.homed_axes %}   # If xy and z are not homed
      { action_raise_error("Must Home X and Y Axis First!") }
   {% else %}
   G90
   G1 Z20 # move clear of bed
   G1 X310 F4000 #move to position above trigger post
   G1 Z3.5 # move down
   G4 P150 # pause
   G1 Z20 # move back up
   G4 P150 # pause
   G1 X100 F4000 move to middle of bed
   {% endif %}




    
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
