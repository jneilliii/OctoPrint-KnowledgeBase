# Printer
X = 220, Y = 270, Z = 300

# Start G-code
```
;; START Cleaning
M117 Cleaningg			; Indicate nozzle clean in progress on LCD
M82 					;set extruder to absolute mode
M107 					;start with the fan off
G21 					;metric values
G90 					;absolute positioning
G28 					;Home (X/Y/Z=0)
M140 S50				; Bed-Temp
;M109 S205				; Nozzel-Temp
G0 Z15 F9000
G0 X20 Y50 Z0.3 F500	; Startposition for Cleaning in bed
G92 E0              	; Set extruder to [0] zero
G1 X80 E25 F500			; Extrude for cleaning (or long line X190 E50)
G92 E0              	; Reset extruder to [0] zero end of cleaning run
G1 E-3 F500         	; Retract filiment by 3 mm to reduce string effect
G1 Z15					; Save home-position			; Z Save-Postion
G1 X0 Y0 Z15 F9000
G1 F9000
M117 Printing...
```
# End G-code
```
M104 S0 						;extruder heater off
M140 S0 						;heated bed heater off (if you have it)
G91 							;relative positioning
G1 E-1 F300  					;retract the filament a bit before lifting the nozzle, to release some of the pressure
G1 Z+0.5 E-5 X-20 Y-20 F9000 	;move Z up a bit and retract filament even more
G1 Z+10 Y+200 F9000  			;NEW:  bring bed to the front
M84 							;steppers off
G90 							;absolute positioning
```

