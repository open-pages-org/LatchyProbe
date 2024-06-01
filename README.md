# Latchy Probe
A 3D printable alternative to the Omron TL-Q5MC2 inductive sensor commonly used on FDM printers for bed levelling

Having a printer with no auto bed levelling is frustrating, BL-Touch is a common choice, Inductive probes are simple off the shelf item, Klicky and Unklicky probes can be printed but need magnets.

What if there was an alternative that you could easily print and only needs a few parts that you probably already have?

The inspiration for this is the Klicky probe (an other similar devices) that have lead the way. In fact the objectives for this project are almost identical to the Klippy objectives: https://github.com/jlas1/Klicky-Probe/blob/main/README.md 
- drop in replacement for Omron TL-Q5MC2 or PL-08N2 (you don't need to replace the toolhead)
- soldering not required
- minimal adjustments required
- be able to detect all the print surfaces
- be as close to the hotend tip as possible
- highly repeatable and accurate probes
- less temperature variations
- no melting of its parts
- cheap to build
- reuse spare parts if possible

with some additions:
- no need to buy special parts and uses common items found at home
- does not reduce the printable area
- does not have detatachable parts

The Latchy Pprobe has been developed over many months, dozens of prototypes and 3 full test versions. Testing on a Creality CR10 (my Voron is stil in the pipeline) and works well. The design is optimised for FDM printing but has the possibility of minaturisation using resin printing. I am always looking for improvemetns so any feedback is welcome here or on Discord ( crashysmashy1 )

If you want to donate something to support this project, please use this [link](https://paypal.me/CrashySmashy?country.x=GB&locale.x=en_GB) to boost the much needed coffee supplies - thanks.

## Presenting the Latchy Probe
<img src="Images/Latchy Probe.jpg" alt="latchyprobe" width="300" style="zoom:50%;" />

Latchy Probe is a direct size for for size replacement for the omron TL-Q5MC2 inductive probe, this allows it to use the same mounting points and adaptaors for many printers. 

So what is Latchy Probe?

- It the same size as the inductive probe but it is a physical contact switch. 
- It is deployed using the 3D printers motion system like Klicky but by pushing the probe tip down on to a trigger post. 
- It has similar internal contacts to Unclicky but no detachable parts. 
- The probe tip retracts when not in use, similar to a BLTouch. 

Most importantly, it is cheap, accurate and can be built at home using parts from a retactable pen (now you know why you grabbed all those free pens at trade fairs).

Electrically it uses only 2 wires that can be connected to the probe input on controller boards, no external signals or power connections are needed. The probe circuit is fail safe because it has a normally closed circuit when corectly deployed, if the circuit reads open the printer will assume the probe is in surface contact and stop.

The trigger post can be located in various positions. For a bed slinger this can be at one end of the X axis. For Core XY this can be near the Z homing pin. The trigger post can be rigid or flexible, a flexible post allows for accidental horizontal contact between the probe and the post without incident.

Just like Klicky, macros allow the deployment process to be automated and example macros for deploment and retraction moves have been provided here XXXX.  If the probe is not deployed before use the probe circuit will remain open and therfore the standard Klipper macros will report 'Probe is already triggered' and stop. 


## Building the Probe

The probe only requires 3 printed parts, a spring (from a retractable pen), thick copper wire for the contacts (1.3mm diameter is ideal, 16AWG, 17SWG or 18SWG), connection wiring and mounting screws.

### Print the parts, 
The compenents have multiple small surfaces and unfortunately this required a well tuned printer to get very clean prints the have:
- no stringing
- no layer zits
- no blobs
- no elephant foot
- no over extrusion

The parts need to be dimensionally accurate so it is helpfult to use the following settings:
- slow print speed with sufficient cooling
- small layer heigh (0.1mm)
- a quality 0.4mm nozzle or finer nozzle if available

Parts to print:
- Probe Body (select the body to suite the available wire size)
- Probe  (select the pin to suite the available wire size)
- Probe Latch

- Optional bending and cutting jig for cross pin  (select the jig to suite the available wire size)
<img src="Images/Parts.jpg" alt="latchy probe parts" width="300" style="zoom:50%;" />

### Find a spring from a doner pen
Spring diameter - this needs to be smaller than 4.5mm diamter but larger than 3mm an ideal size is 4.25mm
Spring length - this needs to be approximately 23mm free lenght and 7mm compressed.
Spring force - 0.35mm wire diameter gives a good level of force
Note: if the spring is too soft the contacts will not be reliable. If the spring is too stiff it will displace the bed surface when probing giving inaccurate readings.

### Assembly

Cut 3 lengths of copper wire approximately 15mm long. Bend 1 piece to match the profile of the jig and trim the ends flush with the jig, make sure there are no burs, round the ends with a file if needed.

1) Insert the latch part into the probe part, the latch should rotate freely, bumping up and down on the ramps.
2) Insert the copper cross pin (this retains the parts and forms the electrical contact). The latch should still rotate freely, bumping up and down on the ramps.
3) Insert the spring into the cup on the latch
4) Insert the assembly into the body and stand on end. The assebmly should slide in and out smoothly for the first 5mm. The resting position should be as shown so that light spring compression will move the cross pin into the body.
5) Time for inital testing
   
Press the assmbly further into the body and it should latch in place. Press again and it should release. The latch rotates with each click and if there are any thick layer lines, blobs, zits or strings the part will not rotate.

The spring force is not high and when first assembled the latch may not index smoothly. The hole in the top of the body can be used to push on the back of the pin assembly to help the rotation. Usually after a few clicks and small burrs are smoothed out and the latch works reliably. If necessary, clean up the parts and sand lighlty to reduce layer lines on the sliding surfaces. 
  
7) Strip the connection wires back about 5 to 7mm
8) With the pin inserted and lactched, insert the connection wire into the body, do not insert too far, only insert the stranded wires and not the insualtion
9) Insert the copper pin approximately 10mm, this must be stiff to grip the braided wires and extend across the slot to form the contact. If the wire is too fat it may split the body apart, if needed either drill the holes larger, print a different body or use thinner wire.
10) Repeat step 7 with the second copper pin.
      NOTE 1: It is Important that both pins are the same diameter and sit evenly in the body. If these pins are uneven then the cross pin will not be able to make reliable contact. 
      NOTE 2: There is an alternative option to use a crimp pin from a JST socket. Select a size that can be easiy inserted into the probe body but be warned there is no release hole and pins are likey to be damaged on removal.
11) Time for initial electrical testing - test the electrical continuity, the probe completes the circuit when extended and remains retracted when pressed once.
12) Once tested bend the end of the copper pins down into the wire slot. Pins can therfore be removed easily by levering out with a small screwdriver.
13) Route the wires in the channel on the body.


##Selecting and building the trigger post and mounting brackets
(not including the trigger post assembly or monting brackets)


Cut 3 short lengths (approx 15mm) of copper wire

If you need a trigger post for your printer then let me know and I can try to help create some standard parts.
There are also docks and mounts submitted by users to support other printers and toolheads, you should check it out.
[Voron v2.4](./Printers/Voron/v1.8_v2.4_Legacy_Trident)

[Voron v1.8](./Printers/Voron/v1.8_v2.4_Legacy_Trident)

[Voron Legacy](./Printers/Voron/v1.8_v2.4_Legacy_Trident)

[Voron Trident](./Printers/Voron/v1.8_v2.4_Legacy_Trident)

[Voron v0](./Printers/Voron/v0)

[VCore 3.0/3.1 ](./Printers/Ratrig/VCore3.0_3.1)

[MercuryOne](./Printers/ZeroG/Mercury_One)

[Voron Switchwire](./Printers/Voron/Switchwire)

Klicky Probe imageklicky early version.

Klicky components
All the compatible printers require:

Toolhead mount (the thing that the probe attached to when it's being used)
Klicky probe (there are three versions, all are interchangeable and compatible, more information on the specific printer page), what actually is used to probe the bed
Probe dock (all the printers use the same)
Probe dock mount (what attaches to the printer to dock the probe when not in use)
The CAD with all the files is located Here

KlickyProbe STL's are now located on each probe type directory.

Printer specific STL are in each printer directory.

The klipper macros are here, the RRF here.

Probe accuracy
The probe accuracy output is better than a range of 0.025mm (difference between highest and lowest), and a standard deviation of 0.01mm.

Print Settings
There are no need for supports, recommended settings are 4 perimeters/top/bottom, at least 23% infill, the STL's are already oriented, you only need to send them to the slicer.



Each printer family/version has it's own mounting options, Bill of Materials, assembly instructions and dock/attach setup.

General Bill of Materials (BOM)
Tools:
3D Printer etc.
1.5mm Drill (optional)
Multimeter to check for Continuity
Wire Cutters

Probe BOM:

Filament
Spring from an old pen
Copper Wire (1.3mm diameter)
Connection wires
Mounting screws


Klicky said it best: By standing on the shoulders of giants, lets see if we can see further.
