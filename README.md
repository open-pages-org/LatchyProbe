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
<img src="Images/Latchy Probe.jpg" alt="latchyprobe" width="600" style="zoom:50%;" />

Latchy Probe is a direct size for for size replacement for the omron TL-Q5MC2 inductive probe, this allows it to use the same mounting points and adaptaors for many printers. 

So what is Latchy Probe?

- It's the same size as the inductive probe, but it is a physical contact switch. 
- It's deployed using the 3D printers motion system like Klicky, but by pushing the probe tip down on to a trigger post. 
- It has similar internal contacts to Unclicky, but no detachable parts. 
- The probe tip retracts when not in use, similar to a BLTouch. 

Most importantly, it is cheap, accurate and can be built at home using parts from a retactable pen (now you know why you grabbed all those free pens at trade fairs).

Electrically it uses only 2 wires that can be connected to the probe input on controller boards, no external signals or power connections are needed. The probe circuit is fail safe because it has a normally closed circuit when corectly deployed, if the circuit reads open the printer will assume the probe is in surface contact and stop.

The trigger post can be located in various positions. For a bed slinger this can be at one end of the X axis. For Core XY this can be near the Z homing pin. The trigger post can be rigid or flexible, a flexible post allows for accidental horizontal contact between the probe and the post without incident.

Just like Klicky, macros allow the deployment process to be automated and example macros for deploment and retraction moves have been provided here XXXX.  If the probe is not deployed before use the probe circuit will remain open and therfore the standard Klipper macros will report 'Probe is already triggered' and stop. 


## Building the Probe

The probe only requires 3 printed parts, a spring (from a retractable pen), thick copper wire for the contacts (1.3mm diameter is ideal, 16AWG, 17SWG or 18SWG), connection wiring and mounting screws.

### Print the parts, 
The components have many small surfaces and unfortunately this required a well tuned printer to ensure there are:
- no stringing
- no layer zits (use painted seam to position these in recesses rather than on curved surfaces)
- no blobs
- no elephant foot
- no over extrusion

The parts need to be dimensionally accurate so it is helpful to use the following settings:
- slow print speed with sufficient cooling
- small layer height (0.1mm)
- a good quality 0.4mm nozzle (or finer nozzle if available)

Parts to print:
- Probe Body (select the body to suite the available wire size)
- Probe  (select the pin to suite the available wire size)
- Probe Latch

<img src="Images/LatchyLatch Print.jpg" alt="latchy 3D prints" width="300" style="zoom:50%;" />

- Optional bending and cutting jig for cross pin  (select the jig to suite the available wire size)
<img src="Images/Parts.jpg" alt="latchy probe parts" width="300" style="zoom:50%;" />

### Find a spring from a doner pen
Spring diameter - this needs to be smaller than 4.5mm diamter but larger than 3mm an ideal size is 4.25mm
Spring length - this needs to be approximately 23mm free lenght and 7mm compressed.
Spring force - 0.35mm wire diameter gives a good level of force
Note: if the spring is too soft the contacts will not be reliable. If the spring is too stiff it will displace the bed surface when probing giving inaccurate readings.

### Assembly

1) Cut 3 lengths of copper wire approximately 15mm long. Bend 1 piece to match the profile of the jig and trim the ends flush with the jig, make sure there are no burs, round the ends with a file if needed.

<img src="Images/PinBendingJig.jpg" alt="Pin bending jig" width="300" style="zoom:50%;" />

2) Insert the latch part into the probe part, the latch should rotate freely, bumping up and down on the ramps.

<img src="Images/latch and probe.jpg" alt="Images/latch and probe" width="300" style="zoom:50%;" />
    
3) Insert the copper cross pin (this retains the parts and forms the electrical contact). The latch should still rotate freely, bumping up and down on the ramps.

<img src="Images/LatchProbePin.jpg" alt="Latch in Probe with Pin" width="300" style="zoom:50%;" />
 
4) Insert the spring into the cup on the latch
   
<img src="Images/probe assembly.jpg" alt="probe assembly" width="300" style="zoom:50%;" />
   
5) Insert the assembly into the body and stand on end. The assebmly should slide in and out smoothly for the first 5mm. The resting position should be as shown so that light spring compression will move the cross pin into the body.

<img src="Images/probeposition.jpg" alt="probe in initial position" width="300" style="zoom:50%;" />
   
6) Time for inital testing
   
Press the assmbly further into the body and it should latch in place. Press again and it should release. 

The latch rotates inside the body with each click and if there are any thick layer lines, blobs, zits or strings the part may not rotate reliably.

The spring force is not high and when first assembled the latch may not rotate or slide smoothly. The hole in the top of the body can be used to push the pin assembly down and help the rotation. Usually after a few clicks any small burrs are smoothed out and the latch works reliably. If necessary, clean up the parts and sand lighlty to reduce layer lines on the sliding surfaces. 
  
7) Strip the connection wires back about 5 to 7mm
  
<img src="Images/wire preparation.jpg" alt="wire preparation" width="300" style="zoom:50%;" />
   
8) With the pin inserted and lactched, insert the connection wire into the body. Do not insert too far, only insert the stranded wires and not the insualtion.

<img src="Images/wire insertion too far.jpg" alt="wire insertion too far" width="300" style="zoom:50%;" />
   
9) Insert the copper pin approximately 10mm, this must be stiff to grip the braided wires and extend across the slot to form the contact. If the wire is too fat it may split the body apart, if needed either drill the holes larger, print a different body or use thinner wire.
    
<img src="Images/Contact pin insertion.jpg" alt="contact pin insertion" width="300" style="zoom:50%;" />

10) Bend the end of the copper pins down into the wire slot. Pins can be removed relatively easily by levering out with a small screwdriver.

<img src="Images/bend pin flush.jpg" alt="bend pin flush" width="300" style="zoom:50%;" />
      
11) Repeat step 7 with the second copper pin.
    
NOTE 1: It is Important that both pins are the same diameter and sit evenly in the body. If these pins are uneven then the cross pin will not be able to make reliable contact. 

NOTE 2: There is an alternative option to use a crimp pin from a JST socket. Select a size that can be easiy inserted into the probe body but be warned there is no release hole and pins are likey to be damaged on removal.
 
<img src="Images/alternative crimp pin.jpg" alt="alternative crimp pin" width="300" style="zoom:50%;" />
    
12) Route the wires in the channel on the body.

<img src="Images/Wireguide.jpg" alt="wired guide" width="300" style="zoom:50%;" />

### The probe is now assembled and ready for initial electrical testing.

Test the electrical continuity, the circuit should be complete when the probe is extended. When the probe is pressed in a fraction of a mm the circuit should open. The contact pins need to be firmly mounted and have no movement or flex in the body to produce repeatable measurements.

## Selecting and building the trigger post and mounting brackets
(not including the trigger post assembly or monting brackets)


If you need help designing a trigger post for your specific printer then let me know and I can try to help create some standard parts.

I welcome the sharing of trigger posts and mounts submitted by users, please message mea and i will include in the STL folders.

## General Bill of Materials (BOM)
Tools:

- 3D Printer etc.
- 1.5mm Drill (optional)
- Multimeter to check for Continuity
- Wire Cutters

Probe BOM:

- Filament
- Spring from an old pen
- Copper Wire (1.3mm diameter)
- Connection wires
- Mounting screws


# Klicky said it best: By standing on the shoulders of giants, lets see if we can see further.
