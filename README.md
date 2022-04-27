## **Please <ins>read each section</ins>**, especially anything marked with ":warning:".
### **These are important warnings that may cause you issues if missed.**

# Important Notes

- :warning: **Required SuperSlicer version:** [:page_facing_up:**2.4.58.2**](https://github.com/supermerill/SuperSlicer/releases/tag/2.4.58.2)

    - Last version update: **2022-04-11**.

    - **Use different SS versions at your own peril.**

        - Newer versions often introduce new bugs or defaults, and older versions may not be compatible with certain settings (or will just error when importing the profile)

        - I will update this as I test newer versions.

        - There is a [:page_facing_up:known bug](https://github.com/AndrewEllis93/Ellis-PIF-Profile/issues/7#issuecomment-1098462899) in this version with the new wall thickess setting, causing it to show crazy values. Just ignore this setting for now (or click "expert" to hide it) - it does not affect anything.

            - ![](/Images/bug.png)

- This profile is more aggressive than most stock profiles, and some things may also need turning down if your printer is still teething. 

- **:warning: This profile's speeds/accels are tuned for linear rail CoreXY (V2/V1/Trident/V0)**. For other printer types (Switchwire, Legacy, others), you will likely need to turn down some speeds and accelerations. 

    - I actually use the same print settings on my Ender 3, just with speeds and accelerations toned down.

- See my [:page_facing_up:tuning guide](https://github.com/AndrewEllis93/Print-Tuning-Guide)! (primarily written for Klipper printers)

- You can find the bed models and textures I am using in [:page_facing_up:Hartk's GitHub repo](https://github.com/hartk1213/MISC/tree/main/Voron%20Mods/SuperSlicer).

- Support my drinking habits:
[![](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/paypalme/AndrewEllis93)

# Table of Contents
**:warning:** = has important warning
- [How to Download](#how-to-download)
- [How to Import](#how-to-import)
- [**:warning:** Start G-code](#start-g-code)
- [**:warning:** Volumetric Speed Limiting](#volumetric-speed-limiting)
- [**:warning:** Accelerations](#accelerations)
- [Cooling](#cooling)
- ["45 Degree" Profile vs Standard Profile](#45-degree-profile-vs-standard-profile)
- [Infill Line Widths](#infill-line-widths)
- [Tips and Tricks](#tips-and-tricks)
- [Profile Change Log](#profile-change-log)

# How to Download
**1)** Navigate to the .ini file.

**2)** Right click "Raw" and click "Save link as"

**If you do not use the "Raw" button, you will get errors trying to import.**

- ![](Images/Download.png) 

Alternatively, download the whole repository as .zip:

- ![](Images/DownloadRepo.png) 
# How to Import
If you downloaded the whole repository as .zip, extract it.

Select **file -> import -> import config** or press ctrl+L.

- ![](Images/Import.png)  

Select the **\.ini** file of the profile you want to import.

Open each tab and click the floppy icon to save the profile. You may have to pick a new name. (you can type in the box that pops up!)
- ![](Images/Saving.png) 

# Start G-code
**:warning: Replace everything in this box with just `PRINT_START`\* if you are not yet passing temperature variables to `PRINT_START`.**
- *\* or the appropriate start g-code for your particular printer.*

See the [:page_facing_up:"Passing Slicer Variables to `PRINT_START`"](https://github.com/AndrewEllis93/Print-Tuning-Guide/blob/main/articles/passing_slicer_variables.md) section of my tuning guide for more information (rationale & instructions).

- If you are new to this, don't worry about this yet - come back to it later on.

![](Images/StartGcode.png)  
# Volumetric Speed Limiting
**:warning: It is very important that you update the volumetric speed setting, otherwise you may have extruder skipping and/or filament grinding.**

![](Images/VolumetricSpeed.png)  

## What is a volumetric speed limit?

The volumetric speed limit controls the maximum rate that your hotend is allowed to extrude. 

No matter how much you push speeds, layer heights, or line widths, **it will never allow you to outrun your hotend.**

Even when not pushing for speeds, I highly recommend putting an appropriate value for your hotend. It's a great failsafe.

## How is it used here?

I keep my infill speed set to **300mm/s**. This is the absolute max I want my infill to ever print - but in reality, it will usually print more slowly due to this limit. 

This essentially prints infill **as fast as the hotend will allow, up to 300mm/s.**.

## Approximate Values

| Hotend     | Flow Rate (mm<sup>3</sup>/sec) |
| :---        |    :----:   |
| E3D V6            | 11
| E3D Revo            | 15
| Dragon SF| 15
| Dragon HF| 24
| Mosquito| 20
| Mosquito Magnum| 30

You should be okay using an approximate value and just lowering it if you have any issues. 

These are approximate values **assuming a standard brass 0.4mm nozzle.** 

Nozzle properties may affect these numbers. For example:
- Larger diameter nozzles will have higher flow rates
- Hardened steel has a lower thermal conductivity and you may get lower flow rates unless you compensate with higher temperatures. 
- Plated copper and tungsten carbide have higher thermal conductivity and might allow a bit higher flow rate. 
- Bondtech CHT nozzles use a different internal geometry that allows higher flow rates.

If you want to get more scientific, test with a specific nozzle or setup, or your hotend just isn't listed, see the [:page_facing_up:"Determining Max Volumetric Flow Rate"](https://github.com/AndrewEllis93/Print-Tuning-Guide/blob/main/articles/determining_max_volumetric_flow_rate.md) section in my print tuning guide.

# Accelerations

**:warning: If you have not yet tuned input shaper, consider reducing these accelerations to 5000 and below.** You may get skipping otherwise (or just very violent toolhead movements).

![](Images/AccelControls.png)  

# Cooling

This profile uses **static fan speeds**. The community has found that varying fan speeds, particularly with high-shrinkage materials, can cause layer inconsistencies.

:warning:Your fan speed will vary based on your fan, material, layer times, and chamber temps. You will need to play with this. See my the [:page_facing_up:"cooling and layer times"](https://github.com/AndrewEllis93/Print-Tuning-Guide/blob/main/articles/cooling_and_layer_times.md) section of my tuning guide.

I use BadNoob's AB-BN-30 duct with the Sunon 5015 fan, and my chamber temp is around 63C. Your setup will likely vary.

![](Images/FanSpeeds.png)  
# "45 Degree" Profile vs Standard Profile

My primary profile is the "45 degree" profile. I orient the STLs to be at a 45 degree angle.

![](Images/45DegreePlate.png)  

## Differences
| Setting | "45 Degree" Profile | Standard Profile |
| --------------- | --------------- | --------------- |
| **seam_position** | Rear | Corners |
| **fill_angle** | 0 | 45 |
| **support_material_angle** | 45 | 0 |

## Pros
- The **main** reason I do this: easier seam placement for large numbers of parts using "rear" seams.
    - Orient the desired seam edge towards the rear of the plate (preferably the sharpest edge).

    - The alternative is "cost-based" *(similar to "sharpest corner" in Cura)*. Cost-based does a good job of placing the seams in corners, but crucially it does not align them. They tend to be scattered around the print at random corners.

- With CoreXY, 45 degree motions only use one motor. 

    - This can sometimes lead to better surface quality on straight walls. Patterns (VFAs) can sometimes occur when both motors are in use (with certain motor models).

- Can *sometimes* result in better overhangs. It seems that airflow sometimes prefers 45 degree prints.

- Aligning the seams to one area of the print can help with layer consistency. The other options ("Aligned", "Cost-Based", and "Corners") can all allow the seam to jump around. That can cause artifacts like this:

    - You can see bands where the seam has hopped to a different edge of the print.

    - *(This example is from Discord. The pressure advance is a bit too low, but it helps to better demonstrate the effect)*

    - ![](/Images/seamjump.png)

## Cons
- Rear seams don't tend to align nicely on rounded corners.

    - ![](Images/ScatteredSeam.png)  

    - I usually try to orient the sharpest corner to the rear of the place. 

    - For objects with only rounded corners, I will sometimes manually place the seam.

    - For some plates, I may also set the seam to "cost-based" or "corners" for certain objects.

- Sometimes the seams can still be placed oddly. Have a quick look at the gcode preview before printing.

Manual seam placement will **always** have the best results. This method is a compromise to save myself a lot of manual work for plates with large numbers of parts.

## Rotating Parts

NEW: SuperSlicer allows you to set auto rotation when importing models in your printer profile:
- ![](Images/auto-rotation.png) 

To manually rotate, press **ctrl+a** to select all objects. \
Type the rotation amount in "Z" the box at the bottom right:

- ![](Images/Rotation.png) 

# Infill Line Widths

The infill line widths are set to a high value in my profile (180%) to save some print time, and to help with infill layer adhesion strength. 

**This proportionally reduces the amount of lines to be printed.** The overall coverage area is still 40%.

If you need greater top layer support, or are printing decorative / low infill parts, you may want to reduce this value for for greater line density.

## 180% Line Width @ 40% Infill
- ![](Images/infill-180.png) 

## 140% Line Width @ 40% Infill
- ![](Images/infill-140.png) 

## 110% Line Width @ 40% Infill
- ![](Images/infill-110.png) 

# Tips and Tricks
## Part Spacing / Plating
Right click the "arrange" button to change part spacing. 

![](Images/Spacing.png) 

Then click the arrange button (or press A), to automatically arrange everything.

Too close can introduce cooling issues. I tend to spread parts out when I can.
# Profile Change Log

Rather than having to re-import the profiles when updates are made, please check the change log occasionally to grab important settings changes / bug fixes.

Use **ctrl + f** in SuperSlicer to find these settings by their internal names below.

- **2022-04-27:**
    - ***thin_perimeters*** to 80% (default)
    - ***thin_perimeters_all*** to 20% (default)
        - Both of these settings are new defaults that got didn't come over properly with the update (came over as 100% / 0%)
- **2022-04-14:**
    - ***use_relative_e_distances*** back to **enabled**.
        - Some people had `M83` in their PRINT_START macros which didn't agree with this. 
- **2022-04-11:**
    - Profiles updated for new beta SuperSlicer version 2.4.58.2.
    - Acceleration controls were moved to the *print settings > speed* tab and ***feature_gcode*** and ***post_process*** were **cleared**. Some accelerations were tweaked.
        - Fine-grained acceleration controls are now natively supported. The old custom g-code method and python scripts are no longer needed! :tada::tada::tada:
    - ***first_layer_size_compensation_layers*** to **3** (was 2)
        - Greater fade distance for elephant's foot compensation, to help prevent some perimeter separation on chamfered parts.
    - ***first_layer_height*** to **0.25** (was 0.24)
        - Just reverting a previous change where I set it to 0.24 to fix a weird bug.
    - ***max_print_speed*** to **300** (was default/80)
        - This doesn't actually do anything since "auto speed" is no longer used (only volumetric speed limiting), but the default value of 80 was confusing people.
    - ***thin_walls_speed*** to **80** (was 30)
    - ***support_material_contact_distance_type*** to "**from filament**" (default) (was "from plane")
        - Cleaning up. I had changed this at one point and don't really know what it does, so default is probably better.
    - ***z_step*** to **0.04** (was default) 
        - This doesn't actually do much except affect the number of decimal places in Z moves. But some people were driving themselves nuts trying to find the "correct" value to put here, so... here.
    - ***duplicate_distance*** to **6** (default)
        - Just affects default part spacing when you auto-arrange your plate.
    - ***support_material_angle*** to **45** (only on 45 degree profile)
    - ***support_material_interface_speed*** to **100** (was 150)
        - These fast speeds could occasionally knock over supports.
    - ~~***use_relative_e_distances*** to **disabled**.~~
        - ~~Absolute mode is theoretically more accurate, but it's probably negligible.~~
    - ***time_estimation_compensation*** to **100%** (default) (was 133%)
        - This was a bodge compensating for the old custom accel controls messing with time estimates, and is no longer needed.
    - ***retract_lift_above[0]*** to **0.25mm** (was 0.2mm).
        - Set to match first layer height.

- **2022-02-19:**  
    - ~~Set accel_to_decel values to half of accel values in ***feature_gcode***.~~
        - ~~Previously accel and accel_to_decel were the same value.~~
        - ~~(Sorry, changed this multiple times now, keep flip-flopping on it)~~
    - Disable ***external_perimeter_cut_corners***
        - This could contribute VERY minorly to gapping between perimeters on corners. You would probably never notice without a magnifying glass.
- **2022-02-10:** 
    - Change ***infill_speed*** to **300** ~~and set **max_print_speed** back to default.~~
        - ***infill_speed*** was previously **0**. 
            - This was not always reaching maximum volumetric speed, due to a misunderstanding on my part of [how auto-speed works](https://github.com/supermerill/SuperSlicer/issues/1629#issuecomment-1013791149). 
            - Setting it to a maximum speed value (instead of 0) better accomplishes my intended goal (maxing out the hotend's capability for infill). 
                - See the [Volumetric Speed / Auto Speed](#volumetric-speed--auto-speed) section for an updated (corrected) explanation.
        - ~~**max_print_speed** was previously 300.~~
            - ~~This setting does not universally limit in the same way that the volumetric speed limit does (for some reason), so it's redundant and confusing to leave it on.~~
    - Change ***support_material_speed*** to **150**.
        - Was previously **240**. This could cause supports to not adhere properly or get knocked over (though supports are disabled by default in this profile).
- ~~**2022-01-02:**~~
    - ~~Change ***first_layer_height*** to **0.24**.~~
        - ~~SuperSlicer currently has a weird bug causing slicing to give up part way through. Setting first layer height to something that's not 0.25 fixes it for me. ¯\\\_(ツ)_/¯~~
- **2021-12-29:** 
    - Change ***resolution*** to **0.0125** (new SS default) and update formatting of ***feature_gcode***
        - Some have reported "move out of range" errors with the old resolution setting of 0.002, likely a bug in SS.
        - The feature_gcode change is purely stylistic. 
- **2021-12-01:** 
    - Enable ***ensure_vertical_shell_thickess*** and revert ***solid_over_perimeters*** to default (2)
        - This can prevent occasional perimeter gapping on steep angles.
- **2021-11-19:** 
    - Set new ***bridge_overlap_min*** setting to **50%**
        - Fixes 50% bridge density in new SuperSlicer version (2.3.57.6).
- **2021-11-11:** 
    - Change ***fill_pattern*** to **grid**.
        - Previously adaptive cubic. Caused occasional pillowing.
    - Change ***max_layer_height[0]*** to **75%**.
        - Previously 0.3mm. Support for nozzle size based percentages was added.
    - Reduce **seam_gap** to **0.**
        - Previously 15% internally. A new SS update now allows control over it. This should prevent seam gaps that occasionally cropped up previously.
- **2021-11-07:** 
    - Fix various support material settings.
        - Supports are disabled in this profile, but the disabled settings had some unnecessary leftovers.
    - ~~Added quotes around ***post_process*** paths.~~
        - ~~This fixes an issue where it would error if trying to use a path with a space.~~
- **2021-10-31:** 
    - Reduce ***retract_length[0]*** to **0.5mm**
        - Previously set to 1mm, which was a bit too aggressive to start with.
    - Change ***overhangs_width_speed*** to **0**.
        - This completely disables applying bridge settings to overhangs.
        - This setting was causing issues for some people, essentially setting overhangs to use 85% flow and high speeds.
- **2021-10-24:** 
    - Disable ***only_one_perimeter_first_layer***.
        - This was causing SS to crash when slicing first layer test patches.
        - I also just changed my mind about the aesthetics.
    - ~~Change ***time_estimation_compensation*** to  **133%.**~~
        - ~~Previously set to 87%.~~
        - ~~This should make the time estimates a *bit* closer to reality, at least with Voron parts. As mentioned above, they will still only be so accurate with the custom acceleration controls, however.~~