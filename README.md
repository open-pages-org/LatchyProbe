# Latchy Probe
3D printable alternative to the Omron TL-Q5MC2 inductive sensor commonly used on FDM printers for bed levelling

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
- does not reduce the printable area
- no need to buy special parts and uses common items found at home
- does not have detatachable parts

The Latchy Pprobe has been developed over many months, dozens of prototypes and 3 full test versions. Testing on a Creality CR10 (my Voron is stil in the pipeline) and works well. The design is optimised for FDM printing but has the possibility of minaturisation using resin printing. I am always looking for improvemetns so any feedback is welcome here or on Discord ( crashysmashy1 )

If you want to donate something to support this project, please use this [link](https://paypal.me/CrashySmashy?country.x=GB&locale.x=en_GB) to boost the much needed coffee supplies - thanks.

## Presenting the Latchy Probe
<img src="Images/Latchy Probe.jpg" alt="latchyprobe" style="zoom:5%;" />

Latchy Probe is a direct size for for size replacement for the omron TL-Q5MC2 inductive probe, this allows it to use the same mounting points and adaptaors for many printers. 

So what is Latchy Probe?

- It the same size as the inductive probe but it is a physical contact switch. 
- It is deployed using the 3D printers motion system like Klicky but by pushing the probe tip down on to a trigger post. 
- It has similar internal contacts to Unclicky but no detachable parts. 
- The probe tip retracts when not in use similar to a BLTouch. 

Most importantly, it is cheap, accurate and can be built at home using parts from a retactable pen (now you know why you grabbed all those free pens at trade fairs).

Electrically it uses only 2 wires that can be connected to the probe input on controller boards, no external signals or power connections are needed. The probe circuit is fail safe because it has a normally closed circuit when corectly deployed, if the circuit reads open the printer will assume the probe is in surface contact and stop.

The trigger post can be located in various positions. For a bed slinger this can be at one end of the X axis. For Core XY this can be near the Z homing pin. The trigger post can be rigid or flexible, the flexible post allows for accidental horizontal contact between the probe and the post without incident.

Just like Klicky, macros allow the deployment process to be automated and example macros for deploment and retraction moves have been provided here XXXX.  If the probe is not deployed before use the probe circuit will remain open and therfore the standard Klipper macros will report 'Probe is already triggered' and stop. 


## Building the Probe

The probe only requires 3 printed parts, a spring (from a retractable pen), thick copper wire for the contacts, 1.3mm diameter is ideal (16AWG, 17SWG or 18SWG), connection wiring and mounting screws.

###Print the parts, 
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
- Probe Pin  (select the pin to suite the available wire size)
- Probe Latch

- Optional bending and cutting jig for cross pin  (select the jig to suite the available wire size)

###Find a spring from a doner pen
Spring diameter - this needs to be smaller than 4.5mm diamter but larger than 3mm an ideal size is 4.25mm
Spring length - this needs to be approximately 23mm free lenght and 7mm compressed.
Spring force - 0.35mm wire diameter gives a good level of force
Note: if the spring is too soft the contacts will not be reliable. If the spring is too stiff it will displace the bed surface when probing giving inaccurate readings.


Selecting and building the trigger post and mounting brackets
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
