# CNC-Macros
Enhanced CNC Macros for Klipper Firmware

Drawing inspiration from Klippy-cnc ([https://github.com/vladbabii/klippy-cnc/tree/master](https://github.com/vladbabii/klippy-cnc/tree/master)), this collection of CNC macros is specifically designed for 3-axis CNC machines like the 3018 / 3040 / 6040, but should work on any. It brings advanced capabilities in workspace management, custom movement commands, and specialized operations for CNC machining, elevating precision and functionality.

The macros make use of the built-in SET_GCODE_OFFSET command to set the workpiece offset.

![Workspace Macros](Macro%20UI.png)

## Features
- **Workspace Management**: Comprehensive set of macros for defining, adjusting, and recalling workspace coordinates.
- **Custom Command Overrides**: Customizes standard G-code commands to incorporate workspace offset.
- **Specialized Operations**: A range of specific macros tailored for tasks like slab flattening and probing.
- **Enhanced Machine Control**: Macros for controlling the spindle, emergency stops, and firmware restarts, augmenting safety and ease of use.

### File Structure
- **workspaceMacro.cfg**: Dedicated to workspace coordinate management.
- **codeOverrides.cfg**: Contains overrides for default macros and custom G-code/M-code commands.
- **customOperations.cfg**: Features macros for specific CNC tasks and operations.
- **kcncCustomOperations.cfg**: Features macros for specific CNC tasks and operations when running klipper-for-cnc fork

Combined, these files significantly improve control, accuracy, and safety in CNC machining using Klipper firmware.

## Usage
1. Download the `.cfg` files or clone the repo into `~/printer_data/config` or your chosen directory.
2. add the [save_variables] section to your `printer.cfg`:
```
[save_variables]
filename:/home/pi/printer_data/config/savedVariables.cfg
```
3. Include the `.cfg` files in your `printer.cfg`:
```
[include CNC-Macros/codeOverrides.cfg]
[include CNC-Macros/workspaceMacro.cfg]
[include CNC-Macros/customOperations.cfg]

If you are using the klipper-for-cnc fork, also include this file

[include CNC-Macros/kcncCustomOperations.cfg]
- Note the kcncCustomOperations.cfg file overrides some macros in customOperations.cfg, this is by design.
```
4. If using a tool setter switch add a `[gcode_macro USER_VARIABLES]` macro to printer.cfg
```[gcode_macro USER_VARIABLES]
variable_tool_setter_xy: 1, 344.2
variable_tool_setter_z_offset: 2.359
variable_workpiece_probe_diameter: 4
variable_multi_diameter: 2.00
gcode:
```

5. Read through the macros and comments in each of the cfg files so you know what's going on.

## Processes
### Simple Job Setup
1. Home the machine.
2. Jog to the start position (e.g., stock Top, Front, Left as 0,0,0 in Fusion 360 CAM).
3. Use `SET_OFFSET_FROM_TOOL` to set X, Y, Z offsets.
4. Optionally, use `SAVE_OFFSETS` to save offsets to a file.
5. Start the job.
6. Profit.

### Probing
Two methods for probing metal workpieces:
1. **Alligator Clips**: Attach to the endmill and the metal piece.
2. **Z Puck Probe**: Like [this one from AliExpress](https://www.aliexpress.com/item/1005001344723565.html).

With alligator clips, you can probe X, Y, and Z positions.

For X and Y Probing:
1. Install a 4mm endmill or rod.
2. Attach clips to endmill and workpiece.
3. Position the tool close to the workpiece.
4. Use `PROBE_AXIS` with variables.
5. The toolhead moves in 0.1mm increments until contact.

For Z Probing:
- Use the actual endmill for correct height offset.

