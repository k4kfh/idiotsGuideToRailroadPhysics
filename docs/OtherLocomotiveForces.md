# Other Locomotive Forces

## Traction Motor Quirks

In the last chapter we discussed the equation for approximating tractive effort:

$$T=2650\frac{nP}{V}$$

While this equation does an excellent job, there are other quirks that the locomotive traction motors have that it doesn't take into account.

### Quirk #1 - Speed Limit
Traction motors have a finite speed limit for a given voltage (which is directly controlled by the throttle notch). While the tractive effort formula above will give (decreasing) force constantly as speed increases, eventually the electric motors reach the maximum RPM for a given voltage. So for example, an F7 in RUN8 typically has a maximum speed of around 60mph. This is similar to a car; at a given throttle (gas pedal) level, without shifting gears, there is only so fast your engine can turn, and thus there is only so fast your car can move. Traction motors have a similar property, it's just related to the voltage of the electricity used to run them (which is from the generator turned by the prime mover) rather than mechanical limits of a combustion engine.

### Quirk #2 - Gear Ratio
Okay, I didn't tell you the whole story in the last paragraph. Traction motors do have a finite RPM limit, but the resulting speed limit for the locomotive is also dependent on the gear ratio. The typical gear ratio for an EMD F7 is 62:15, meaning the axle gear has 62 teeth and the motor gear has 15 teeth. But there _were_ other gear ratios sold by EMD, to offer more speed at the cost of power, or more power at the cost of speed. This has to do with gear reduction, and it will affect the maximum speed at a given notch.

### Math for the Quirks

Maximum speed for a given gear ratio, in my experience, is just something you have to look up. Finding a data sheet, [like this one for the F7,](http://www.thedieselshop.us/Data%20EMD%20F7.HTML) will often tell you. If you can't find it there, try to find a PDF of the locomotive's manual, like [this, also for the F7.](http://users.fini.net/~bersano/english-anglais/EMD-F7.pdf)

Once you have the _maximum_ maximum speed, you can approximate the maximum speed for a given notch with some simple multiplication.

```
MaxSpeed = Absolute Max Speed * (Current Notch / 8)
```

So an example for an F7 in RUN3 would be:

```
MaxSpeed = 65mph * (3 / 8)
         = 24.375mph
```

There are probably more accurate methods for this, but simple multiplication will get the job done. If you're really adventurous, you could use an RPM table for different throttle positions on a given prime mover, and do some math with that. But that's more advanced than I'll get into here.

---

## Rolling Resistance

All rolling objects have a natural resistance to rolling. This is called (uncreatively) [**rolling resistance**](http://www.engineeringtoolbox.com/rolling-friction-resistance-d_1303.html).

Rolling resistance is constant, so it's very simple to calculate:

```
Rolling Resistance = (Coefficient of Rolling Resistance) * (Weight of the Object)
```
|Common Coefficients of Rolling Resistance |
|---------------|--------------------------------------|
| 0.001 - 0.002 | railroad steel wheels on steel rails |
| 0.001         | bicycle tire on wooden track         |
| 0.004         | bicycle tire on asphalt road         |
| 0.02          | car tires on asphalt                 |
| 0.04 - 0.08   | car tires on loose gravel            |

If you're dealing with American trains, chances are you already have the locomotive's weight (in pounds). If you happen to be working in SI units (grams) then you can convert to Newtons (the SI unit of force) by multiplying your object's mass by 9.8 m/sec^2, the acceleration of Earth's gravity.

### Example #1: EMD F7

The EMD F7-A weighs 230,000lbs. We know from the table above that the coefficient of rolling resistance for steel wheels on steel rails is around 0.0015. So let's do the math:

```
Rolling Resistance = 0.0015 * 230,000lbs
                   = 345lbs
```

So at _any_ speed, a force of 345lbs is acting to stop the rolling of an EMD F7-A.

### Example #2: Loaded Boxcar

According to [Union Pacific](https://www.up.com/customers/all/equipment/descriptions/boxcars/index.htm), the total weight of a loaded boxcar is between 263,000 and 286,000lbs. Let's assume ours is in the middle and weighs 270,000lbs. The boxcar has steel wheels, and will roll on steel rail, so again we'll use 0.0015 as the coefficient of rolling resistance.

```
Rolling Resistance = 0.0015 * 270,000lbs
                   = 405lbs
```

So at any speed, a loaded Union Pacific boxcar has a force of 405lbs acting to slow down/stop it.

---

## Grade Forces

Up until this point, we've been assuming that trains all operate on perfectly flat, level track. Of course we know that's not the case. There are hills all over the place in the real world. So how do we account for that?

Simple: **For every ton of train weight on a 1% grade a force of 20 pounds is acting to roll the train down the hill.**

![](http://alkrugsite.evilgeniustech.com/rrfacts/trainon1.gif)<br>
_Image Credit: Al Krug_

So for a 1% grade, convert your train weight to tons (2000lbs = 1 ton) and multiply by 20. For other grades, use the following logic:

For every ton of train weight on a 1% grade, a force of 20 pounds is acting to roll the train down the hill. Therefore, for every ton of train weight on a 2% grade, a force of _40lbs_ is acting to roll the train down the hill.

Using the logic/math above, you can calculate the pull of a grade on any train just by knowing the train's weight and the grade it's on (1%, 2%, etc).

---

That's all for this page, in the next chapter I'll discuss summing up these forces to find acceleration/speed of the train.