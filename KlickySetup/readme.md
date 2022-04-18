## This is a short set of recommendations and requirements for setting up klicky and calibrate_z scripts.

### This uses the following 3 sets of macros: 

https://github.com/jlas1/Klicky-Probe 
https://github.com/VoronDesign/VoronUsers/tree/master/printer_mods/edwardyeeks/Decontaminator_Purge_Bucket_&_Nozzle_Scrubber
https://github.com/protoloft/klipper_z_calibration

1. You must enable and correctly set relative_reference_index under [bed mesh] section in printer.cfg. This is calculated by ((X sample points * Y sample points) -1) /2  EG for 5x5 grid, use 12, or for 7x7 grid, use 24.
2. You must not have load_gcode_state or G28 or G28 Z in your print_start macro *after* calibrate_z or it will overwrite your offset settings.
3. You must not have clear_bed_mesh in your print_end script unless you also have print_start either loading or generating a new mesh each time. 
4. For right side mounted buckets, you need to fix the decontaminator code as detailed here: https://github.com/VoronDesign/VoronUsers/issues/635

### Recommended Print_Start macro:
```
[gcode_macro PRINT_START]
#   Use PRINT_START for the slicer starting script
gcode:
  G90 #set absolute positioning
  G28 #home all axis
  
  Attach_Probe_Lock #prevent probe docking until unlocked, from klicky 
  #Z_TILT_ADJUST #Trident *or* 
  #QUAD_GANTRY_LEVEL #V2.4
  
  CLEAN_NOZZLE #requires brush/purge bucket, from decontaminator
  G28 Z #rehome Z axis 
  CALIBRATE_Z #automatic Z offset, from klipper z calibration  
  
  #BED_MESH_PROFILE LOAD=default #load saved mesh *or*
  #BED_MESH_CALIBRATE #generate new mesh
  
  Dock_Probe_Unlock #removes probe lock
```
 You can add heatup/wait functions and additional nozzle cleaning/G28 Z if you wish but do them *before* Calibrate_Z.  

### Recommended PREHEAT macro:
```
[gcode_macro PREHEAT]
gcode:
  G90 #set absolute positioning
  G28 #home all axis
  
  ## Move hotend a sufficent distance from heated bed for heat soak
  #--------------------------------------------------------------------
  #G0 X125 Y125 Z50 F3600 ## Uncomment for 250mm build
  #G0 X150 Y150 Z50 F3600 ## Uncomment for 300mm build
  #G0 X175 Y175 Z50 F3600 ## Uncomment for 350mm build
  #--------------------------------------------------------------------
  M106 S255 #set parts fan to full speed, helps circulate chamber air
  SET_HEATER_TEMPERATURE HEATER=heater_bed TARGET=110 #For ABS
 ``` 
### Setting your **switch_offset** in **klicky-z-calibration.cfg** 

#### Ensure your **position_endstop** is correct in **printer.cfg**
1. Set your **position_endstop to 0** *>>> Save & Restart* 
2. Home XYZ and then move your nozzle to bed center, and jog until you touch a piece of paper.
3. Take your current Z value and edit **position_endstop** to match, but invert the value.  Eg Z0.34 = **position_endstop: -0.34**  *>>> Save & Restart*

#### Ensure your calibrate_z is pressing the Z endstop switch on the switch body *next* to the trigger button, not the button itself. 
1. If it's not, then set in **klicky-z-calibration.cfg**: **probe_switch_x** = **probe_nozzle_x** - 8, **probe_switch_y** = **probe_nozzle_y** - 20  (you have to do the math youself and enter the resulting values, not the subtractions) *>>> Save & Restart*

#### Determine your **switch_offset** in **klicky-z-calibration.cfg**:
1. Set your **switch_offset** to a small value like 0.3 *>>> Save & Restart*
2. run **calibrate_z** and take note of the **z_offset** value generated.  
3. Again, move your nozzle to bed center and jog until you can touch a piece of paper. 
4. Take the current z value and add the inverse to your **switch_offset** value. Eg Z-0.21 is 0.3+0.21 = **switch_offset: 0.51** *>>> Save & Restart*
5. run **calibrate_z** again and your **z_offset** value should be very close to zero. 
 
