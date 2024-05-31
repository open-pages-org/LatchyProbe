# Latchy Probe
3D printable alternative to the Omron TL-Q5MC2 inductive sensor commonly used on FDM printers for bed levelling

Having a printer with no auto bed levelling is frustrating, BL-Touch is a common choice, Inductive probes are simple off the shelf item, Klicky and Un-Klicky probes can be printed but need magnets.

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

# Presenting the Latchy Probe
<img src="./Probes/KlickyNG/Photos/klickyNG.png" alt="klickyprobe" style="zoom:50%;" />


Latchy Probe is a direct size for for size replacement for the omron TL-Q5MC2 inductive probe, this allows it to use the same mounting points or adaptaors. 

The only difference in operation is the probe tip is deployed and retracted by pushing the probe tip down on to a trigger post just like using retractable pen. This process can be automated by adjusting the existing macros to include the extend/retract moves.

The trigger post can be rigid or flexible. The flexible option ensures any accidental horizontal contact between the probe and the post does not cause breakages.

I have included example macros for deploment and retraction moves, however the probe is generally set up as fail safe. If the probe is not deployed before use the probe connection will be open and therfore the normal Klipper macros will report 'Probe is already triggered' and stop. 

The probe has been developmed over many months and after dozens of prototypes 3 versions have been fully tested on a Creality CR10 (my Voron is stil in the pipeline) and works well. It has been a challenge to get the design reliable to 3D print on FDM due to the close tollerances, I am always looking for improvemetns so any feedback is welcome here or on Discord ( crashysmashy1 )

If you want to donate something regarding this project, please use this [link](https://paypal.me/CrashySmashy?country.x=GB&locale.x=en_GB) to top up the tea kitty - thanks.


Building the Probe

The probe only requires 3 printed parts, a spring (from a retractable pen) and thick copper wire for the contacts.

Print the parts, these must be:
- very clean prints with no stringing, zits or elephant foot
- dimensionally accurate - it is worth slowing down, reducing the layer hight and possibly switching to a fine nozzle if avaialble (although 0.4 works)

Find a spring from a doner pen

Cut some short lengths of copper wire



If you are upgrading from an earlier version, check the klipper macros folder, it contains update instructions.

Probe options
Right now, there are two probe attachment options, each with two probe types.

Regular Klicky
First klicky probe, based on the Quickdraw probe, with an added third magnet for added stability and fixed dock gantry setups.

klickyprobe

It uses magnets to secure the probe to the mount and also to make the electrical connection. The magnets can be glued to prevent them from coming loose. It supports a microswitch probe and Unklicky (invented by DustinSpeed) (self built probe, that so far surpasses the microswitches in common use) based probing.

Assembly instruction
KlickyNG
New enclosed magnets probe, it does not require glue to help prevent the magnets from coming loose, magnets are also self aligning. This approach only uses common and easy to source parts.

klickyprobe

Also supports microswitch probe and Unklicky (invented by DustinSpeed) (self built probe, that so far surpasses the microswitches in common use) based probing.

Assembly instruction
Printers With detailed instructions and specific parts (by support order)
The specific parts with install, configuration, troubleshoot and recommended settings can be found on each printer page, linked below.

Voron v2.4

Voron v1.8

Voron Legacy

Voron Trident

Voron v0

VCore 3.0/3.1

MercuryOne

Voron Switchwire

There are also docks and mounts submitted by users to support other printers and toolheads, you should check it out.

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

1.5mm Drill (optional)
Multimeter to check for Continuity
Super Glue
Soldering Iron for the heat inserts
Probe BOM:

1x microswitch (the omron D2F-5 or D2F-5L (removing the lever) is recommended), D2F-1 and similar sizes microswitch also work
2x M2x10 mm self tapping
some 6 mm x 3 mm magnets (it ranges from 8 to 10)
some m5 screws
some m3 screws
some m3 heat inserts
some m3 nuts
Sourcing guide
To get the best experience, please consider purchasing from the trusted list of suppliers bellow.

trusted suppliers list

Assembled Klicky Probe on a Voron v2.4
Assembled Klicky Probe
Dock and undock video
 Dock_and_Undock.mp4 
It is working very well, if you decide to use it, give me feedback, either here, or on Voron discord, my discord user is JosAr#0517.

By standing on the shoulders of giants, lets see if we can see further.
