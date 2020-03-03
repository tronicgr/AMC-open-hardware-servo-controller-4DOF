# AMC-open-hardware-servo-controller-4DOF
For use with 4DOF rigs with 100mm stroke actuators


### Warning, its not compatible with Simfeedback software, the programmer was kind enough to not allow ANY other servo controller except the arduino leonardo. 


The purpose of this is to make available the design files for this 4DOF servo controller for DIY construction from scratch. You will need to supply your own Arduino Mega 2560 and load on it the supplied firmware here. You can use this controller to connect your 4DOF rig on Simtools or FlyPT Mover using the usual AMC or Thanos interface plugins.
Link to the free FlyPT mover motion software:
https://www.xsimulator.net/community/threads/flypt-mover-interface.13464/


Warning: This project requires some soldering so be prepared.


You can order the PCB made on OshPark:
https://oshpark.com/shared_projects/qweSHDEY


![Alt Text](https://github.com/tronicgr/AMC-open-hardware-servo-controller-4DOF/blob/master/Gerber-files/TOP-view.jpg)

![Alt Text](https://github.com/tronicgr/AMC-open-hardware-servo-controller-4DOF/blob/master/Gerber-files/BOTTOM-view.jpg)


### List of materials:
```
C1-C9  0.1uF - CAP,SM,SER,X7R,0.1uF,10%,50V,(0805)
R1-R10  2.2k - RES,SM,THK FLM,2.2k,1%,0.1W,(0805)
LD1-LD2  LTST-C193TBKT-5A - DIODE,LED,Blue,470nm,2.8V,0603

BTN2-BTN3  6x6x7mm Momentary Push Button Switch Tact Through-Hole
https://www.ebay.com/itm/SI-10-Pcs-6x6x7mm-4-Pins-DIP-PCB-Momentary-Tactile-Tact-Push-Button-Switch/122630868699

Dsub1-Dsub4  D-SUB 25 Female - D-SUB 25 Round Pin Straight Female Through Hole Connector Adapter 2 Row
https://www.ebay.com/itm/5Pcs-D-SUB-25-Round-Pin-2-Row-Straight-Female-Through-Hole-Connector-Adapter/172251129183

2-pin 3.5mm Screw Terminal - 3.5mm Pitch 2 pin 2 way Straight Pin PCB Screw Terminal Blocks Connector
https://www.ebay.com/itm/181846953484

64 pin headers male 2.54mm

```


![Alt Text](https://github.com/tronicgr/AMC-open-hardware-servo-controller-4DOF/blob/master/AMC-Open-Hardware-4DOF_and_AMC-AASD15A.jpg)

The AMC-Open-Hardware-4DOF next to the fully loaded AMC-AASD15A servo controller


![Alt Text](https://github.com/tronicgr/AMC-open-hardware-servo-controller-4DOF/blob/master/shield_top_side.jpg)

![Alt Text](https://github.com/tronicgr/AMC-open-hardware-servo-controller-4DOF/blob/master/shield_bottom_side.jpg)



### Reminder: Cut the RESET_EN trace after loading the firmware.

![Alt Text](https://github.com/tronicgr/AMC-open-hardware-servo-controller-4DOF/blob/master/Arduino_Reset_EN_cut_trace.jpg)



### ======= FIRMWARE 4DOF - 80mm stroke - AASD-15A servos =======
```
release date: 11/25/2019

List of limitations and features:
  -motion range:  80mm stroke
  -speed: 250mm/s
  -Pulse frequency: 45khz  
  -Controller loop frequency: 800 times/sec
  -Automatic Park-Standby
  -E-stop disables Servos
  -Same datapackets as AMC device in Simtools and FlyPT
        
```

### Video tutorial of loading the firmware on the Arduino Mega 2560:

https://www.youtube.com/watch?v=zZX-h4tYw1E


### AC Servo Settings
```
Push MOD until you see Pn000. This enters the parameter mode.
Change and check these settings on all motors:

Pn8 = 300
Pn9 = -300
Pn51 = 3000
Pn98 = 20 - Pulse Multiplier (electronics gear)
Pn109 = 1 - smoothing, 1=fixed smoothing, 2=s-Shaped smoothing
Pn110 = 30 - Smoothing Filter Time
Pn113 = 20 - Feedforward %
Pn114 = 10 - Feedforward Filter Time (ms)
Pn115 = 100 - Gain %
---Extra parameters needed---
Pn24 = 100 
Pn52 = 1 
Pn60 = 2 
Pn61 = 6 
```


