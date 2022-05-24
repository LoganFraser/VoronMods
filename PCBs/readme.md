# 2 Part PCB with FFC for Stealthburner/CW2

This is an early peek at the 2part PCB for SB/CW2 for FFC cables, using jst-xh sockets, and a matching electronics breakout board, using 2 2x5 ribbon cables to connect.
24V and HE0 need to be connected from the YZ board to the controller using larger (22-24AWG) wires, as the 28awg typical in ribbon cables isn't sufficient. 

As this progresses, I'll be adding a replacement XY and YZ board so this can be an entirely self-sourced option. 

#### Update:  I have uploaded EasyEDA PCB and Gerber files.  These are for testing only, if you decide to get some made, thoroughly test all signal/voltage paths before installing. 


![FFC PCBs](ffc-pcbs.png)

### BOM: 
* 1 - FCI / Amphenol SLW20R-1C7LF FFC 90 degree 
* 4 - WR-BHD 2.54mm Box 10Pin Male Straight sockets - Same as EXP
* 4 - 4 pin JST-XH sockets
* 3 - 3 pin JST-XH sockets
* 5 - 2 pin JST-XH sockets
* 1 - 8 pin/2 row Dupont straight pins (or 90, see below)
* 1 - 8 pin/2 row Dupont 90 socket (or straight, need 1 straight and 1 90, whatever is easier to source)
* 1 - Bat85 diode (can bypass if using switch probe)
* 1 - 100ohm resistor (smd 0805 or 1/4W barrel)
* 2 - Molex Microfit 90 surface mount (or 1, if using JST-XH for thermistor)

#### Optional for Fan PCB (instead of soldering direct):
* 2 - 4 pin/1 row dupont 90 pins (either include 5V or 24V, not both)
* 1 - 3 pin/1 row dupont 90 pins (for fan pcb, instead of soldering)
* 2 - 2 or 4 pin dupont connector for Fans
* 1 - 3 pin dupont connector for LEDs


