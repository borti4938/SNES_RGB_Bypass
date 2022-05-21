# The RGB Bypass Modding Boards for the SNES

There are three versions of the modding board available. Each version differs a bit in its components according to its purpose.

Take a look into the BOM, where the components are listed. Each modding board has its own page. Take also a look into the notes provided there to ensure that you create your board according to your needs.

#### Need a PCB?
Look at OSHPark: [SNES\_RGBAmp](https://oshpark.com/shared_projects/FGz6MoI9), [SNES\_RGBAmp\_wCSYNC](https://oshpark.com/shared_projects/hMAav8Ju), [SNES\_RGBAmp\_wCSYNC\_SVid](https://oshpark.com/shared_projects/QUsZvq5m)
  
Or use any other PCB service you like :) Make sure that your PCB will be produced on a **substrate of 0.8mm or thinner** to make installation smooth and easy.

If you want to go with [PCBWay.com](http://www.pcbway.com/) (non-affiliate link), you can also use [my affiliate link](http://www.pcbway.com/setinvite.aspx?inviteid=10658) to support me. Only do that if you want that!

Thank you very much!

# Installation Notes

## 1Chip-SNES (classical big model and SNES2 (Mini/Jr.))

PCB-Version to use:  

- SNES\_RGBAmp
  * mainly for 1Chip-01/02 boards
- SNES\_RGBAmp\_wCSYNC
  * mainly for 1Chip-03 boards and for SNES Mini/SFC Jr.
  * can be used in a 1Chip-01/02 if and only if the pin 3 of the MultiAV is freed (initially CSYNC (TTL) on NTSC and 12V on PAL models)
- SNES\_RGBAmp\_wCSYNC\_SVid **[1]**
  * same as SNES\_RGBAmp\_wCSYNC just with footprints for S-Video completion in SNES2 models (Mini/Jr.)

**Note [1]**  
Do not use this board in a classical big model SNES.

### Installation

#### Preparation of the SNES Mainboard

In the big model you have to free pin 1, 2 and 4 from the MultiAV, which are the outputs of the red, green and blue component, respectively. To do so, simply

- remove R15, R16 and R17 from the SNES mainboard

In the SNES2, these pins are unused, i.e., you don't have to do anything here.

If you want to /CSYNC from the modding board you also have to ensure that pin 3 of the MultiAV is also free. In a 1Chip-03 (NTSC only) and in SNES2 models, this is the case by default. In 1Chip-01/02 boards you have to:

- NTSC systems:
  * remove R12
  * optionally also remove R9 - R11 and Q1
- PAL systems:
  * remove R28 and D1

#### Decoupling caps at the MultiAV

There are some 47pF ceramic capacitors at the MultiAV at pin 1 - 4, namely C44 (pin 1), C45 (pin 2), C46 (pin 3) and C47 (pin 4). 
There are also some footprints for 47pF ceramic capacitors on the modding board , namely C14, C24, C34 and C44.  
Make sure that you either have for each pin at the MultiAV just a single cap installed. **If you unsure, just left the footprints on the MultiAV free as these caps are optional.**

**To sum up:**

- **1Chip-01/02** versions have all capacitors C44-C47 on board. So left C14-C44 on the modding board unpopulated.
- **1Chip-03** versions does not have C46 installed. So you can put C44 onto the modding board. C44 is directly next to R43.
- **SNES2** version do not C44-C47. This version even does not have footprints for them. So assemble C14-C44 on the modding board.

#### Board installation

Solder the modding board into your SNES directly on to of the MultiAV pins.  
Make sure that you don't have (electrical) contact between SNES mainboard and your modding board. If you are unsure, simply use isolating tape.  
The modding board has to face the long side of the SNES mainboard (opposite of output direction of the MultiAV port) with the solder pads.  

Connect the solder pads as follows:

- R - red image component from S-CPUN pin156
- G - green image component from S-CPUN pin157
- B - blue image component from S-CPUN pin158
- /CS - composite sync from S-CPUN pin151 (SNES\_RGBAmp\_wCSYNC and SNES\_RGBAmp\_wCSYNC\_SVid only)
- Y - luma component from S-RGB pin17 (SNES\_RGBAmp\_wCSYNC\_SVid only)
- C - chroma component from S-RGB pin12 (SNES\_RGBAmp\_wCSYNC\_SVid)
  
Look for appropriate vias and other equivalent solder joints ;) 

You find equivalent installation guide with pictures on [RetroRGB.com](http://retrorgb.com/snes.html)
(These guides do not exploit the decoupling caps. Just C46 is mentioned for /CSYNC in 1Chip-01/02 systems, but in a wrong way.)

### Misc
  
#### Jumper J1 - Low Pass Filter of the THS7373 and THS7374   

Both ICs are supported. The THS7373 has one SD-video low pass filter (used for /CSYNC) and three HD-video filters (used for R, G and B). The THS7374 has four SD-video filters.  
At the THS7373 the HD-video LPFs can be bypassed and at the THS7374 all four SD-video filters can be bypassed (using J1).  
  
The Jumper 1 is for bypassing the internal low pass filters.
 
- leave J1 open: internal filters are NOT bypassed  
- close J1: internal filters are bypassed (not effective for the SD-video channel of the THS7373)   
  
It is not possible to set the three HD-video (THS7373) or four SD-video (THS7374) low pass filters individually!

Normally, you don't need a LPF inside your SNES output. There are just a few capture cards out there who don't have a anti aliasing filter for is ADC inputs. There you have to have the LPF turned on.

On analog devices a LPF is not needed. On professional upscaling solutions like the Framemeister and OSSC you have an anti aliasing filter prior to its ADC circuits. This means you can turn the filter of the THS7374 off.

#### Some words on TTL or 75ohm /CSYNC

Actually I was under the impression that there are modern devices out there who need TTL sync. But that's not the case - at least I'm 99% sure. For example:

- OSSC needs 75ohm csync
- Framemeister needs 75ohm csync
- All consumer TVs needs 75ohm csync

Just a month ago I realised the reason, why most people want to requests TTL csync. Just because most csync cables have a resistor in the csync wire. And on the other hand - just a few weeks after that some YouTube videos arise and show how to modify RGB cables.  
A resistor inside the cable drops down the voltage level of /CSYNC to make it compatible to 75ohm inputs

Actually, all my consoles outputs true 75ohm csync (not approximately as others do "use a resistor of something in between..."), i.e. /CSYNC has negative impulses from 0V downto -300mV.  
And I went fine with it. I can run my OSSC without any problems, a Framemeister I borrowed a few weeks ago, went run fine, too :)
I even don't have to use sync filtering... 

#### Brightness and Ghosting Issues 

##### Brightness

The S-CPUN overdrives the outputs of R, G and B. This has the effect that R', G' and B' (signals going to your TV set) have higher peak-to-peak voltage values than the standard defines. In simple word, the **brightness is to high**.

The reduce the the brightness a common choice is to increase the load on the R, G and B lines (wires coming out of the S-CPUN) to correct that. Measurements were taken [by Ultron](http://assemblergames.com/l/threads/snes-mini-rgb-measurements.53053/). Thanks for that btw.
RetroRGB summarizes the final solution:

- for the 'regular' 1Chip model: [using 750 Ohm resistors](http://retrorgb.com/snes1chip.html)
- for the SNES Mini/Jr.: [using 1.2k Ohm resistors](http://retrorgb.com/snesminipremade7314.html)

The resistors, which are soldered to the SNES mainboard, got a place on the modding PCB (R11, R21 and R31). You may want to the SMD 0603 footprints for the resistors correcting the brightness, here.

**Alternatively**, you can adapt R3 on the SNES mainboard. This is a 1.6k Ohm resistor in SMD0805 (big model) or SMD0603 (SNES2) size.
From my measurements, replacing this resistor

- by a **1.74k Ohm** resistor in a big model SNES, and
- by a **1.69k Ohm** resistor in a SNES2 (Mini/Jr.)  
  (Just an estiamtion, I hadn't had a SNES2 for measurements here, so far)

seems to be a good choice. **By replacing R3 on the SNES mainboard** you must not use the brightness correction resistors R11, R21 and R31 on the modding board.

##### Ghosting

Also some consoles have some output swing on changes of the output amplitude. These swing can be +/- 10mV to 20mV in peak-to-peak and runs over several pixels width. This results into **ghosting**.

The ghosting can be vanished by replacing C11 on the SNES mainboard by a 220nF (accuracy +/- 20% is fair enough) X5R / X7R ceramic capacitor in SMD0805 (big model) or SMD0603 (SNES2) size.
**Many thanks to [Voultar and Ste](https://shmups.system11.org/viewtopic.php?p=1282757#p1282757) for finding that out!!!**

---

## 3Chip-SNES

PCB-Version to use:  

- SNES\_RGBAmp **[3]**
  * for 3Chip-SNES boards
  * 3Chip-SNES = SNES with dedicated S-CPU, S-PPU1 and S-PPU2
- SNES\_RGBAmp\_wCSYNC
  * can be used, too, but this is not documented here. Just make sure, that you don't have another output on pin 3 of the MultiAV and find an appropriate /CSYNC source.

**Note [3]** Left the footprints for the 47pF ceramic capacitors on the modding board , namely C14, C24, C34 and C44, **unpopulated** 

**INTSTALLATION MAY DIFFER FROM MAINBOARD VERSION TO MAINBOARD VERSION!**
  
The presented installation methods here are either

- reported: I haven't done the modification by myself but it is reported to work in that way, or  
- checked: I have done the modification by myself and have measured the output with an oscilloscope.

If I have a reference url, I will give it.

### Installation
#### SNSP-CPU-01/02 (checked)
  
##### additional components needed for the SNES mainboard:
- 3x 750Ohm resistor 0805 package (0.125W, 1% tolerance should be fine)  
- 3x 1.87kOhm resistor 0805 package (0.125W, 1% tolerance should be fine)   

##### how to install:  
- remove R8, R9, R13, R14, R18, R19 and Q4, Q6, Q8 from the SNES mainboard  
- solder the three 1.87kOhm resistors to R8, R13 and R18  
- solder the three 750Ohm resistors to R9, R14 and R19  
- solder the directly beneath the MultiAV  
- connect pad 'R' with the pad where the base of Q4 was  
- connect pad 'G' with the pad where the base of Q6 was  
- connect pad 'B' with the pad where the base of Q8 was  
  
##### reference url: [German forum Circuit-Board.de](https://circuit-board.de/forum/index.php/Thread/20199-SNES-RGB-Bypass-für-PAL-3-Chip-SNES-SNSP-CPU-01-02/)
  
#### SHVC-CPU-01 (reported)

##### additional components needed for the SNES mainboard:
- 3x 750Ohm resistor 0805 package (0.125W, 1% tolerance should be fine)  
- 3x 1.87kOhm resistor 0805 package (0.125W, 1% tolerance should be fine)   
  
##### how to install:  
- cut traces for R', G' and B' right before the MultiAV (but behind C44, C45 and C47)  
- solder the three 1.87kOhm resistors to R8, R13 and R18  
- solder the three 750Ohm resistors to R9, R14 and R19  
- solder modding board to the MultiAV  
- connect pad 'R' with the point (R8,R9)  
- connect pad 'G' with the point (R13,R14)  
- connect pad 'B' with the point (R18,R19)  
  
##### reference url: [English forum AssemblerGAMES](http://assemblergames.com/l/threads/attempted-to-rgb-bypass-shvc-cpu-01-super-famicom-picture-too-bright.63581/)


#### SNS-CPU-GBM-01/02 (---)

It likely the same procedure as for the SHVC-CPU-01. Yet, I haven't tested it and it was never reported! Please keep that in mind if you decide to install the bypass board into your SNES and please report the result to me.
  
#### SNS-CPU-RGB-01/02 (---) 
  
The installation on these mainboards should be similar to the one presented for the SNS-CPU-APU-01; but it was never reported so far.  
If you do the modification (on your own risk), please report the result to me. 
  
#### SNS-CPU-APU-01 (checked) 
  
##### how to install:  
- remove R56, R57 and R58 from the SNES mainboard  
- solder modding board to the MultiAV  
- connect pad 'R' with the point (R6,R7,C6)  
- connect pad 'G' with the point (R8,R9,C7)  
- connect pad 'B' with the point (R10,R11,C8) 

##### reference url: [German forum Circuit-Board.de](https://circuit-board.de/forum/index.php/Thread/20199-SNES-RGB-Bypass-f%C3%BCr-PAL-3-Chip-SNES-SNSP-CPU-01-02/?postID=537811#post537811)
