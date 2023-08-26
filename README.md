# CNC-Macros
CNC Macros for Klipper

Heaviy inspired by Klippy-cnc: https://github.com/vladbabii/klippy-cnc/tree/master


## Overview
* Simplified macros to override standard move macros: G0 to G3
* The overrides will adjust the move commands by an offset value in X Y Z
* Offsets are set with another macro: SET_WORKSPACE_XYZ
* Inteded for use on a 3 axis CNC like the 3018, spindle is controlled via the heater bed output, set in my printer.cfg as a PWM output with custom M3 and M4 macros
* Custom G28 macro for my machine (need to home Z first before moving XY)


## Process
1. Include the two cfg files in your printer.cfg
1. Home your machine
1. Jog your machine to the starting 'workspace' position (I use stock Top, Front, Left as 0,0,0 in Fusion 360 CAM)
1. Use the Set Workspace XYZ macro to set the X Y Z offset coordinates
1. Start the job
1. Profit


## G-Code Macros
- **CHECK_WORKSPACE_XYZ**: Reports the current workspace offsets for X, Y, and Z.
- **SET_WORKSPACE_XYZ**: Stores the current X, Y, Z positions as workspace offsets.
- **CLEAR_WORKSPACE_XYZ**: Clears the stored workspace offsets for X, Y, and Z.
- **ZERO_WORKSPACE_XYZ**: Sets the workspace offsets to zero for X, Y, and Z.
- **WORKSPACE**: Manages variables for workspace X, Y, and Z offsets.
- **ADJUSTED_MOVE**: Handles movement commands (G0, G1, etc.) and applies workspace offsets if they are set. Also includes safety measures like stopping the job if the printer is not homed.
- **PROBE_WORKSPACE_Z**: Probes the Z-axis and stores the height as a workspace offset.
- **MOVE_Y_MAX**: Moves the Y-axis to its maximum position as defined in the printer's config.

## Built-In Macros Overrides
- **FW_RESTART**: Firmware Restart
- **PAUSE**: Pause the print and move the Z-axis to a predefined position.
- **RESUME**: Resume the print and restore the Z-axis to its original position.
- **CANCEL_PRINT**: Cancel the print and move the Z-axis to a predefined position.

### G-Code Overrides
- **G0**: Rapid linear move with workspace offset applied.
- **G1**: Linear move with workspace offset applied.
- **G2**: Clockwise arc move with workspace offset applied.
- **G3**: Counterclockwise arc move with workspace offset applied.

### G and M Commands
- **G0, G1, G2, G3**: Move commands that pass through ADJUSTED_MOVE macro for workspace offset adjustment.
- **M112**: Emergency stop (default in ADJUSTED_MOVE).
- **M114**: Get the current position.
- **M3 S10000**: Start the spindle at full speed.
- **M5**: Stop the spindle.
- **M84 X Y Z**: Turn off the motors.
