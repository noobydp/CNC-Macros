# CNC-Macros
CNC Macros for Klipper

Heaviy inspired by Klippy-cnc: https://github.com/vladbabii/klippy-cnc/tree/master

[h1] WARNING - THIS HAS NOT BEEN TESTED TO ACTUALLY WORK WITH A JOB

[H1] Overview
[ul] Simplified macros to override standard move macros: G0 to G3
[ul] The overrides will adjust the move commands by an offset value in X Y Z
[ul] Offsets are set with another macro: SET_WORKSPACE_XYZ
[ul] Inteded for use on a 3 axis CNC like the 3018, spindle is controlled via the heater bed output, set in my printer.cfg as a PWM output with custom M3 and M4 macros
[ul] Custom G28 macro for my machine (need to home Z first before moving XY)
[ul]


[H2] Process
[li] Include the two cfg files in your printer.cfg
[li] Home your machine
[li] Jog your machine to the starting 'workspace' position (I use stock Top, Front, Left as 0,0,0 in Fusion 360 CAM)
[li] Use the Set Workspace XYZ macro to set the X Y Z offset coordinates
[li] Start the job
[li] Profit