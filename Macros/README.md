# Klipper macros


The macros are similar to those for other docable probes [here](docs/Dockable_Probe.md)
These macros are based on the KlackEnder 
Include the probe details - this example is for the CR10v2, adjust for your printer

``` CR10v2 Probe details maccro
[probe]
pin: ^PD2 #Probe pin for CR10v2 2.5.2 Board (Replaces BLTouch)
x_offset: 50
y_offset: -3.4
#z_offset: -6
speed: 5
lift_speed: 50.0
samples:2
samples_result: median
sample_retract_dist: 1.0
samples_tolerance: 0.01
samples_tolerance_retries: 6
```

Include a macro for the bed mesh settings, adjust to allow for bed size and probe offset from the nozzle:
``` bed mesh macro
[bed_mesh]
speed: 100
horizontal_move_z: 5
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


This allows the bed to be manually levelled  using the probe to measure the height above each corner screw. Change the X, Y positions and screw thread type to suit your printer.
``` Screw tile adjust
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






```macro
 dock_position: 300, 295, 0
```
