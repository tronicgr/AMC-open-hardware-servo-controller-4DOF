# AMC-open-hardware-servo-controller-4DOF
For use with 4DOF rigs with 100mm stroke actuators


### Warning, its not compatible with Simfeedback software, the programmer was kind enough to not allow ANY other servo controller except the arduino leonardo. 


The purpose of this is to make available the design files for this 4DOF servo controller for DIY construction from scratch. You will need to supply your own Arduino Mega 2560 and load on it the supplied firmware here. You can use this controller to connect your 4DOF rig on Simtools or FlyPT Mover motion software using the usual AMC or Thanos interface plugins.
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



### ======= FIRMWARE 4DOF - 100mm stroke - AASD-15A servos =======
```
release date: 11/25/2019

List of limitations and features:
  -motion range:  100mm stroke
  -speed: 250mm/s
  -Pulse frequency: 45khz  
  -Controller loop frequency: 800 times/sec
  -Automatic Park-Standby
  -E-stop disables Servos
  -Same datapackets as AMC device in Simtools and FlyPT, just 4 axis usable.
  -Automatic home calibration of the actuators.
        
```

### Video tutorial of loading the firmware on the Arduino Mega 2560:

https://www.youtube.com/watch?v=zZX-h4tYw1E

### Other Video regarding testing and Simtools connection:

Manual testing with servomotors: https://www.youtube.com/watch?v=ZpkSNeMeobE

Test on Simtools manual slider: https://www.youtube.com/watch?v=LB0JdBqnM4k


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

### ---Programmers information---
```
x-sim:
Make sure to set the axis to 16bit resolution, Binary
On the USO set the BAUD speed to 250000 , 8 , NO , 1
Then the dataformat for axisinformations in x-sim is:
~255~~255~~a01~~a02~~a03~~a04~~0~~0~~0~~0~~10~~13~

Generic data format information:
The data string is 20 bytes long total.
0xFF 0xFF b1 b2 b3 b4 b5 b6 b7 b8 b9 b10 b11 b12 b13 b14 b15 b16  LF CR

0xFF 0xFF - start of data identifier for the receiving micro controller
byte1 - 8 bit binary number giving act1 demand MSB 
byte2 - 8 bit binary number giving act1 demand LSB         
byte3 - 8 bit binary number giving act2 demand MSB
byte4 - 8 bit binary number giving act2 demand LSB
byte5 - 8 bit binary number giving act3 demand MSB
byte6 - 8 bit binary number giving act3 demand LSB 
byte7 - 8 bit binary number giving act4 demand MSB
byte8 - 8 bit binary number giving act4 demand LSB 
byte9 - 8 bit binary number giving act5 demand MSB 
byte10 - 8 bit binary number giving act5 demand LSB 
byte11 - 8 bit binary number giving act6 demand MSB 
byte12 - 8 bit binary number giving act6 demand LSB 
byte13 - 8 bit binary number giving act7 demand MSB 
byte14 - 8 bit binary number giving act7 demand LSB 
byte15 - 8 bit binary number giving act8 demand MSB 
byte16 - 8 bit binary number giving act8 demand LSB 
LF   - Line Feed character
CR  - Carriage Return character

I add the two bytes to form a 16-bit value (for 0 to 65535 range, with 32512 mid position) like this:
act1word = act1high
Shift act1word , Left , 8bits
act1word = act1word + act1low

where 
act1word is word type (65535)
act1high is byte type
act1low is byte type


---Example of data to send for 4 Axis (Must include 0 values for not used axis to be compatible with AMC protocol):
    1 0xFF  ID
    2 0xFF  ID
    3 0x7F  AXIS1 MSB
    4 0x0F  AXIS1 LSB
    5 0x7F  AXIS2 MSB
    6 0x0F  AXIS2 LSB
    7 0x7F  AXIS3 MSB
    8 0x0F  AXIS3 LSB   
    9 0x7F  AXIS4 MSB  
    10 0x0F  AXIS4 LSB  
    11 0x00    
    12 0x00    
    13 0x00    
    14 0x00  
    15 0x00   
    16 0x00    
    17 0x00   
    18 0x00  
    19 0x0A  LF
    20 0x0D  CR     


-----Simplified example code for sending axis data (for arduino):
int outputValue0 = 0;        // value output
int outputValue1 = 0;        // value output
int outputValue2 = 0;        // value output
int outputValue3 = 0;        // value output

byte buf0[2];
byte buf1[2];
byte buf2[2];
byte buf3[2];
byte buf4[2];
byte buf5[2];
byte buf6[2];
byte buf7[2];
byte ID[2];
byte endstring[2];
void setup() {
  Serial.begin(250000);
}
void loop() {
  // ID AXIS1 AXIS2 AXIS3 AXIS4 AXIS5 AXIS6 AXIS7 AXIS8 LF/CR
  // - The ID is byte values 0xFF + 0xFF
  // - Each Axis is 16bit wide.
  // - LF+CR is required in the end (0x0A + 0x0D)
  // change the analog out value:
  ID[0] = 255;
  ID[1] = 255;
  buf0[1] = outputValue0 & 255;
  buf0[0] = (outputValue0 >> 8) & 255;
  buf1[1] = outputValue1 & 255;
  buf1[0] = (outputValue1 >> 8) & 255;
  buf2[1] = outputValue2 & 255;
  buf2[0] = (outputValue2 >> 8) & 255;
  buf3[1] = outputValue3 & 255;
  buf3[0] = (outputValue3 >> 8) & 255;
  buf4[1] = 0;
  buf4[0] = 0;
  buf5[1] = 0;
  buf5[0] = 0;
  buf6[1] = 0;
  buf6[0] = 0;
  buf7[1] = 0;
  buf7[0] = 0;
  endstring[0] = 10; //LF
  endstring[1] = 13; //CR
  Serial.write(ID, sizeof(ID));
  Serial.write(buf0, sizeof(buf0));
  Serial.write(buf1, sizeof(buf1));
  Serial.write(buf2, sizeof(buf2));
  Serial.write(buf3, sizeof(buf3));
  Serial.write(buf4, sizeof(buf4));
  Serial.write(buf5, sizeof(buf5));
  Serial.write(buf6, sizeof(buf5));
  Serial.write(buf7, sizeof(buf5));
  Serial.write(endstring, sizeof(endstring));
  delay(2);   // wait 2 milliseconds before the next loop
}



```
