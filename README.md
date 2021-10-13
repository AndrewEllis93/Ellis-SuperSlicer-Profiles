## Volumetric Speed / Auto Speed
<b>It is very important that you update the volumetric speed setting.</b>

These two setting serve as a universal "speed limit". No matter how much you push speeds, layer heights, or line widths, it will never allow you to exceed the maximum flow rate or of your hotend, or the maximum print speed.

This is important because I keep my infill speed set to 0. This means it will print infill as fast as the hotend will allow, or up to the 300mm/sec max print speed limit, whichever comes first.

If you have this set too high, your extruder *will* skip and/or grind.

![](Images/VolumetricSpeed.png)  

#### Approximate values:
V6: <b>12</b>\
Dragon SF: <b>15</b>\
Dragon HF: <b>24</b>\
Mosquito: <b>20</b>\
Mosquito Magnum: <b>30</b>

See the last section ("determining volumetric flow") for more details.

## Acceleration Control

This profile uses a custom acceleration control setup. Acceleration would typically be done directly in the speed settings, but currently SuperSlicer does not allow setting accelerations for every extrusion type (for example internal vs external perimeters).

I advise leaving the accelerations conservative for anything visible, but especially for perimeters. While you can push the accels quite high with input shaper, I have found that it can still cause bizarre bulging issues. 

I now only push accelerations for things like infill and travels.

I use 8 square corner velocity because I have found it to make corners slightly crisper.

If you use these advanced acceleration controls, ensure that you have the normal acceleration controls under speed settings completely disabled.

![](Images/AccelControls.png)  


## Post Processing (Travel Accels)
This is optional, in fact I would start with it disabled and come back to it later.

The sole purpose of this post processing script is to set accels/square corner velocity for travel moves, as it is not supported by the above accel controls.

I use the script from Stephan: https://github.com/Stephan3/Schnitzelslicerrepo/blob/master/superslicer/pp.py

Install Python on your computer and swap the two python exe path and the script path accordingly. 

Adjust your desired accel, accel to decel, and square corner velocity at the top of the scipt file.

![](Images/PostProcessing.png)  

## Cooling

This profile uses <b>static fan speeds</b>. The community has found that varying fan speeds, particularly with high-shrinkage materials, can cause layer inconsistencies. Essentially some areas will cool and contract faster than others.

The exact fan speed will vary based on your fan, material, layer times, and chamber temps. You may need to play with this. 

I use BadNoob's AB-BN-30 duct with the Sunon fan, and my chamber temp is around 63C. The stock fan setup may need more cooling as the airflow is weaker.

![](Images/FanSpeeds.png)  

## "45 Degree" Profile vs Standard Profile

My primary profile is the "45 degree" profile. This means that I print all of my parts at 45 degrees, with the seams set to "rear". I orient my desired seam edge towards the rear of the plate.

This makes it much easier to align the seams where I want them, as otherwise SuperSlicer tends to place them oddly.

The differences with the "45 degree" profile are:
- <b>Print Settings > Perimeters & Shell > Seam:</b> \
    Set to "rear" (rather than cost-based)
- <b>Print Settings > Infill > Angle > Fill:</b> \
0 degrees (rather than 45 degrees)

![](Images/45DegreePlate.png)  

## Start G-code
My start gcode follows the convention I laid out in my Discord pin: https://discord.com/channels/460117602945990666/460172848565190667/866045862782304326

This passes the bed, hotend, and chamber temps to my `PRINT_START` macro so that I can control exactly when they happen during my start gcode.

If you are have not set up your `PRINT_START` based on my pin, you may just want `PRINT_START` here without the other stuff, or you may get heating errors.

![](Images/StartGcode.png)  

## Determining volumetric flow

Keep in mind this is a rough calculation - maximum volumetric flow rate can change with a number of factors like temperatures and material type. Once you find your maximum with the below method, I would still advise setting it slightly lower for margin of safety. I set mine slightly on the low side so that I don't have to tune it per filament/material.

Volumetric flow is expressed in mm^3/sec (cubic millimeters per second)

- <b>Volume = mm divided by 0.415.</b>

Or, inversely, 

- <b>mm = volume * 0.415.</b>

For example, if you extrude at 5mm/sec, that is a flow rate of ~12mm^3/sec (5mm / 0.415).

You will follow a similar process to extruder calibration. 

<b>1)</b> Heat your hotend. \
<b>2)</b> Extrude a little bit to ensure your E motor is energized and holding.\
<b>3)</b> Mark a 120mm length of filament going into your extruder.\
<b>4)</b> Extrude at increasing speeds. At each interval, measure to ensure that exactly 100mm entered the extruder.\
<b>5)</b> Keep going until it starts dropping below 100m/sec. This is your max flow rate. \
<b>6)</b> Convert your extrusion speed to volumetric speed using the above formulas. \
<b>7)</b> Enter a slightly lower volumetric speed into the slicer.
