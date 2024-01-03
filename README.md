# CNC-Macros
CNC Macros for Klipper

Heaviy inspired by Klippy-cnc: https://github.com/vladbabii/klippy-cnc/tree/master


This collection of CNC macros for Klipper firmware enhances the functionality and precision of 3-axis CNC machines like the 3018. It offers advanced workspace management, customized movement commands, and specialized operations for CNC machining.

## Features
Workspace Management: Macros for setting, adjusting, and recalling workspace coordinates, crucial for precision and repeatability in CNC operations.

Custom Command Overrides: Redefines standard G-code commands to include workspace offsets, ensuring accurate movements.

Specialized Operations: Includes specific macros for tasks like slab flattening and probing, adding automation and precision to CNC processes.

Enhanced Machine Control: Macros for spindle control, emergency stops, and firmware restarts, improving safety and usability.

### File Structure
workspaceMacro.cfg: Focuses on workspace coordinate management.

codeOverrides.cfg: Contains overrides for built-in macros and custom G-code/M-code commands.

customOperations.cfg: Offers macros for specific CNC tasks.

These files collectively enhance the CNC machining experience by providing more control, accuracy, and safety, making them a valuable addition to any CNC setup using Klipper firmware.


## Usage
1. Include the cfg files in your printer.cfg

## Processes
### Simple Job Setup Process
1. Home your machine
1. Jog your machine tool/endmill to the start position (I use stock Top, Front, Left as 0,0,0 in Fusion 360 CAM)
1. Use the Set Workspace From Tool macro to set the X Y Z offset coordinates
1. Optionally use Save Variables to save the offset to a file
1. Start the job
1. Profit

### Probing Process
I have two methods for probing metal workpieces:
1. A pair of alligator clips that I attach to the endmill and the metal piece
2. A Z Puck Probe [like this](https://www.aliexpress.com/item/1005001344723565.html)

With the alligator clips, you can probe the X, Y and Z positions of the workpiece.
Probing X and Y
1. Install a 4mm endmill or rod - I suggest using a sacrifical / broken endmill the same diameter as your
2. Attach the clips to the endmill and workpiece.
3. Move the tool very close to the X, Y or Z part of the workpiece
4. Use the Start Probe Axis macro with variables - Axis is required, others are optional
5. The toolhead will move towards the workpiece in 0.1mm increments until they touch

Probing Z is the same - EXCEPT - I install the actual endmill I will use so the height offset is correct.

## G-Code Macros
### Workspace Macros
Macros designed for managing workspace coordinates, essential for CNC machining precision.
The workspace offets are managed manually and regular move commands have been overriden to include the offset adjustment.

**SAVE_VARIABLES**: Saves the current workspace offsets to a file on the Raspberry Pi. This allows the offsets to be reloaded even after a firmware restart or power cycle, ensuring workspace coordinates persist.
**LOAD_VARIABLES**: Loads the previously saved workspace offsets from the file on the Raspberry Pi, reinstating the workspace position settings.
**CHECK_WORKSPACE_XYZ**: Reports the current workspace offsets for X, Y, and Z. Provides warnings if any of the offsets have not been set, ensuring the user is aware of the current workspace state.
**GOTO_WORKSPACE_X_Y**: Moves the tool to a specified X and Y position within the defined workspace, allowing for precise positioning based on the set workspace coordinates.
**SET_WORKSPACE_FROM_TOOL**: Captures the current position of the tool and sets it as the new workspace offset, providing a quick way to redefine the workspace origin based on the tool's position.
**UPDATE_WORKSPACE_XYZ**: Allows the user to manually set or update workspace offsets for X, Y, and Z axes, offering flexibility in adjusting the workspace origin as needed.
**CLEAR_WORKSPACE_XYZ**: Resets the workspace offsets, clearing any previously stored values and reverting the offsets to their default state.
**ZERO_WORKSPACE_XYZ**: Sets the workspace offsets to zero for X, Y, and Z, effectively aligning the workspace origin with the machine's current position.
**WORKSPACE**: A utility macro that reports the current workspace offsets, providing quick access to the current offset values.
**ADJUSTED_MOVE**: Intercepts movement commands (like G0, G1) and applies the workspace offsets to them. This macro ensures all movements are correctly offset according to the defined workspace, and includes safety checks like ensuring the machine is homed.

## Built-In Overrides
This section covers the codeOverrides.cfg file, which contains a series of macro overrides and custom G-code and M-code commands. These macros provide enhanced control and functionality for CNC operations.

### Built-In Macro Overrides
**NOTE THE PAUSE AND RESUME MACROS ARE BROKEN**
FW_RESTART: Simply executes a firmware restart. Useful for quickly rebooting the system.
**EStop**: Triggers an emergency stop (M112) to immediately halt all operations.
**Pause**, Resume, and Cancel Print Macros
**PAUSE**: Saves the current position, stops the spindle, and then executes the base pause command (PAUSE_BASE). It moves the Z-axis to its maximum position for clear access.
Need to update Pause and Resume to use the built-in save state process.
**RESUME**: Restores the spindle speed to its pre-pause state and moves the tool back to its saved position before resuming the print job.
**CANCEL_PRINT**: Raises the Z-axis to its maximum, stops the spindle, turns off the motors, and then calls the base cancel print command (CANCEL_PRINT_BASE).
G-Code Overrides
**G0** (Rapid Linear Move): Processes a rapid linear move command, passing through the ADJUSTED_MOVE macro for workspace offset adjustments.
**G1** (Linear Move): Similar to G0, but for standard linear moves.
**G2** (Arc Move Clockwise) and G3 (Arc Move Counter-Clockwise): These macros process arc movements, applying workspace offsets and arc-specific parameters (I, J, K).
**G28** (Custom Homing Sequence): Custom homing macro that ensures Z-axis is homed first to avoid collisions, then homes X and Y axes.
M-Code Commands
**M3** (Start Spindle Clockwise): Starts the spindle motor. Includes checks for workspace offsets and homing status to ensure safe operation. Features a ramp-up delay to allow the spindle to spin up.
**M4** (Counter-Clockwise Spindle): Calls M3 as the spindle can't spin counter-clockwise. A placeholder for compatibility.
**M5** (Stop Spindle): Stops the spindle motor.

## Custom Operations
**FLATTEN_SLAB**: This macro performs a flattening operation on a workpiece slab. It calculates the cut depth, feed rate, step over, and spindle speed based on provided parameters or defaults. The macro executes a perimeter cut followed by an H-pattern cut across the slab, ensuring an even and flat surface.

**MOVE_Y_MAX**: Moves the Y-axis to its maximum position. It first checks if the Y-axis is homed, then performs the movement, moving the gantry/head for easy access or preparation for the next operation.

### Probing Operations
**PROBE_AXIS**: Sets up variables for probing operations, including the axis to probe, step size, feed rate, total steps, and distance.
**START_PROBE_Z**: Initiates a Z-axis probing sequence, useful for setting tool length or finding the workpiece surface. This is a shortcut to execute START_PROBE_AXIS with defaults for Z movement downwards.

**START_PROBE_AXIS**: General-purpose macro to start probing on a specified axis. It handles setup for probing direction, distance, feed rate, and step size. This macro is flexible and can be adapted for various probing needs on different axes. You can specify the Axis X, Y or Z, the direction as below. Other variables have defaults, the toolhead will move in 0.1mm increments for 10mm or until the endmill touches the workpiece.
This macro defaults to a 4mm endmill to calculate the offset between the touch point and centre.
Directions:
X: 1 = left to right, -1 = right to left
Y: 1 = front to back, -1 = back to front
Z: 1 = upwards (this normally is wrong), -1 = downwards
Note: 1 is default for X and Y, -1 is default for Z if you dont send a value.

**CONTINUE_PROBE** (Delayed Gcode): Continues the probing process. It incrementally moves the tool along the specified axis, checking for probe contact. If the probe is triggered, it updates the workspace coordinates; if not, it continues until the probe triggers or the maximum distance is reached.


