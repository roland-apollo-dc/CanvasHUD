# Zhakami Zhako's Gauge Script / Canvas HUD System
#### For use with SaccFlight; For internal Saccflight, BLACK ACES staff, 425th VTFTW, YURU YURU AVIATION members, UHR and selected individuals only. 
#### Not to be redistributed unless permitted.
#### Discord: ZhakamiZhako#2147 | Twitter: [@ZZhako](https://twitter.com/ZZhako/) | zhintamizhakami@gmail.com
#### Documentation is still in progress.
#### Tutorial is still being made.


## Introduction

Sacchan's Flight and Vehicles (SaccFlight) is an amazing system when it comes to creating Aircraft, Boats and Vehicles in VRChat and yet lacks an easier way in order to create your own custom instruments or HUD.

This system aims to allow people to create their projected instruments / HUD / Effects via Animations, Animation parameters, Animation layers and Text Elements.


## The Script Explained

The script works in a way where it takes certain values of the aircraft's state; Altitude, Speed, etc.
Any parameters that are empty will be ignored by the script; However... If you have animators without these parameters, you might want to either create these parameters or leave them empty; as Unity attempts to 'input' these values but will error out in the logs due to parameters missing from the said animators.


Reference:

### Animations

Typically, the way you would animate your gadgets/hud element/instrument is basically animating them from the 0 value, up to the maximum value in order for this to work.
CanvasHUD/GaugeScript takes the values from the SaccAirVehicle components and translates these to a normalized parameter from 0~1f. 

| Function          |Description| No. of Parameters |No. of dividers|Animation|Looped?|Parameter Range|
|--|--|--|--|--|--|--|
| Altimeter         | Altitude Readout based on aircraft's absolute position (+ if supplied, map offset.)   | 3 |3|0 to Maximum         ||0 - 1 Continuous|
| Speedometer       | Speed readout based on aircraft's velocity.                                           | 3 |3|0 to Maximum         ||0 - 1 Continuous|
| Radar Altimeter   | Altitude readout based on raycast shot from the bottom of the center of mass          | 2 |2|0 to Maximum         ||0 - 1 Continuous|
| Pitch             | Aircraft's absolute local rotation in X axis. For use with the ADI;                   | 1 | |-90, 0, +90          |Yes|0, 0.5, 1|
| Roll              | Aircraft's absolute local rotation in Z axis. For use with the ADI;                   | 1 | |-180, 0, +180        |Yes|0, 0.5, 1|
| Heading           | Aicraft's absolute rotation in Y Axis. For use with the ADI / Heading                 | 1 | |0 to +360            |Yes|0 - 1|
| Gs                | Aircraft's G's output.                                                                | 1 | |Minimum, 0, Maximum  ||0 - 1 Continuous|
| Mach              | Aircraft's Velocity in Mach                                                           | 1 | |0 to Maximum         ||0 - 1 Continuous|
| AoA               | Aircraft's Angle of Attack readout.                                                   | 1 | |0 to Maximum         ||0 - 1 Continuous|
| Afterburner       | Aircraft if Afterburner is engaged                                                    | 1 | |false, true          ||True/False|
| Directional G's X | Aircraft G's readout on the X axis                                                    | 1 | |0 to Maximum         ||0 - 1|
| Directional G's Y | Aircraft G's readout on the Y axis                                                    | 1 | |0 to Maximum         || 0 - 1|
| Directional G's Z | Aircraft G's readout on the Z axis                                                    | 1 | |0 to Maximum         || 0 - 1|
| Side G            | Aircraft's Gravity readout, Affected by the G's on the Y axis.                        | 1 | |Minimum, 0, Maximum  ||0, 0.5, 1|
| Velocity Indicator X| Aircraft's Velocity Indicator based on it's local X Axis.                           | 1 | |Minimum, 0, Maximum  |                       |0, 0.5, 1|
| Velocity Indicator Y| Aircraft's Velocity Indicator based on it's local Y Axis.                                                 | 1 | |Minimum, 0, Maximum  ||0, 0.5, 1|
| Velocity Indicator Z| Aircraft's Velocity Indicator based on it's local Z Axis.                                                 | 1 | |Minimum, 0, Maximum  ||0, 0.5, 1|
| Rotational Velocity Indicator X| Aircraft's rotational Velocity Indicator based on it's local X Axis.                           | 1 | |Minimum, 0, Maximum  ||0, 0.5, 1|
| Rotational Velocity Indicator Y| Aircraft's rotational Velocity Indicator based on it's local Y Axis.                           | 1 | |Minimum, 0, Maximum  ||0, 0.5, 1|
| Rotational Velocity Indicator Z| Aircraft's rotational Velocity Indicator based on it's local Z Axis.                           | 1 | |Minimum, 0, Maximum  ||0, 0.5, 1|
| Pull Up| Indicator if the aircraft has reached a lower altitude limit without the landing gears down | 1 | | True, False | | 
| Rate of Climb | Aircraft's climb rate in seconds | 1 | | Minimum, 0, Maximum |No | 0, 0.5, 1



### Text

|Function|Description|Parameters
|---|---|---|
|RPM|Aircraft's RPM / Current Engine output. Can be customized with Min RPM, Max RPM. Engine off will lerp back to 0.|Min RPM, Max RPM as mapped values
|Temperature|Aircraft's 'Virtual' Temperature output. Responsive according to RPM. Can be customized with Min Temp, Max Temp. Engine off will lerp back to 0.|Min Temp, Max Temp as mapped values
|Gs|Aircraft's G's readout. Can be useful for debugging|
|Altimeter|Aircraft's Altimeter readout. Can be useful for debugging. Can be customized with Altimeter Divisor. |3 Dividers
|Speedometer|Aircraft's Speed readout. Can be customized with Speed Divider| 3 Dividers
|Fuel|Aircraft's Fuel readout. Can be customized with Min Fuel and Max Fuel.| Min Fuel, Max Fuel as mapped values
|Gun Text| Gun Ammo readout in bullets. Requires to have DFUNC_GUN(s) assigned. Can be customized with Min ammo, Max Ammo. |Min Ammo, Max Ammo as mapped values
|Heading| Aircraft's absolute rotation in Y Axis readout. Returns 00X, 0XX, or XXX.|
|Rate of Climb in Seconds| Aircraft's Rate of climb in text.|


### Inspector Properties
|Variable Name|Required?|Description
|---|---|---|
|Engine Control|Y|The Aircraft's Sacc Air vehicle Object|
|DFUNC_Guns||



## CanvasHUD to Instruments / Elements
This section will explain on how the script converts parameters from the vehicle to animation controller to animation.


### Attitude / Artificial Horizon Indicator

Suppose that you already have a model / canvas that indicates where the aircraft is facing at, you may have to create two empties before the mesh itself; One that represents Roll, and one that represents pitch.
#Insert Photo of hierarchy


Let's focus on the pitching. Make an animation that represents the aircraft when facing -90 degrees down, level, then 90 degrees up.
#Insert photo of animation.

Once done, go to your Animation Controller. Create the parameter that represents your pitching parameter as a float. If the layer does not exist, create a layer that represents your aircraft's pitch for the instrument/HUD.
#Insert photo

In your Layer, insert the animation. Set the animation's Motion time to the parameter you have created that represents the pitch. 
#Insert photo for layer, animation, motion time.

In the CanvasHUD script, set the Pitch parameter. Set the animation controller as well if not assigned.

#Insert Photo of inspector.
