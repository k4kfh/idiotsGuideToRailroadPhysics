## Introduction to Simulation Physics

The backbone of train simulation physics is the simple equation, *F=MA* (*Force = Mass x Acceleration*). Acceleration is a measure of how quickly you are gaining or losing speed. [Read more on accceleration here.](http://www.physicsclassroom.com/class/1DKin/Lesson-1/Acceleration)

We want to find the acceleration of the train, and we can use algebra to rearrange *F=MA*, making it *Acceleration = Force/Mass*. So to find the acceleration (and thus the speed) we need to know the force acting on the train and  the mass of the train. The mass is easy to find. The force...not so much.

!!! Tip "Not familiar with F=MA?"
	If you are not already familiar with the basics of F=MA, please [read more on that here.](http://www.physicsclassroom.com/class/newtlaws/Lesson-3/Newton-s-Second-Law) There are some excellent practice problems to get you comfortable with Newton's second law on that page as well.

## Horsepower

*Horsepower is an interesting concept. We know that the F7-A has a 1500HP motor, but what can we do with that number?*

One **Horsepower** is equal to 550 foot-pounds per second. This means that one horsepower could lift 550 pounds by 1 foot in 1 second. This can also be represented as approximately 745.7 watts.

**Important Concept:** On a diesel locomotive, you are not always outputting the maximum horsepower. It is roughly proportional to throttle position, so we can estimate the output horsepower like this:

```
Output Horsepower = (Throttle Position/8) * Maximum
```

So for an F7 with the throttle in RUN1, the math would look like this:

```
187.5 HP = (1/8) * 1500HP
```

And it looks like, from this, we could convert horsepower to foot-lbs and be all done, right? ``187.5 * 550 = 10,3125 foot-pounds per second``, right?

No, not exactly.

Because of the complex machinery generating this force, the output force at the wheels is not linearly proportional to the throttle position. We'll cover more on this later, but it has to do with the properties of electric motors (such as our traction motors), and specifically [Back-EMF.](https://en.wikipedia.org/wiki/Counter-electromotive_force)

## What is Tractive Effort?

On a locomotive, the linear output force (how hard it pulls) is called **tractive effort**. Because of how [Back-EMF](https://en.wikipedia.org/wiki/Counter-electromotive_force) affects electric motors, the faster a motor spins, the less current it draws, and consequently, the less force it outputs. The specifics behind this decline in **tractive effort** get very complex, but the principle is simple:

**The higher your speed, the lower your locomotive's tractive effort.**

This decline is not linear; **it happens in an exponential curve.**

With this in mind, there are two places we can get tractive effort:

- **Starting Tractive Effort**, found in the locomotive data sheet, is the rated tractive effort starting from a standstill in RUN8. On the F7, this is 56,500lbs.
- **The Tractive Effort estimation formula,** which I found in a paper from Virginia Tech University.

---
## Tractive Effort Estimation Formula

I do not take credit for this excellent formula. I found it in [a paper by Virginia Tech on Rail Resistance Equations.](http://128.173.204.63/courses/cee3604/cee3604_pub/rail_resistance.pdf) It is located on page 11.

$$T=2650\frac{nP}{V}$$

- *T* is Tractive Effort, in Newtons.

- *P* is Horsepower. Remember, the output horsepower, not the maximum horsepower!

- *V* is the velocity (aka speed) in km/hr.

- *n* is the locomotive's efficiency in converting power output to tractive effort. For the F7, I found ~0.72 is best.

!!! Note "You might wonder...how accurate is this?"

	For the F7, within 0.75%.

	The other way that you could find tractive effort, of course, is by using a table of measured values, however these are hard to find for many locomotives. I tested this equation against a known table of values for the EMD F7, and by tuning the efficiency coefficient (n) I was able to get an average percent error of 0.33%. I deemed this acceptable, so this guide will be using this formula for tractive effort unless a better method comes along.

### How do I use this formula?

**1. Find your locomotive's starting tractive effort. This is available in most locomotive data sheets.** For the F7, this is 56,500lbs.

**2. Find a "Continuous Tractive Effort" rating for your locomotive.** You may find multiple ratings for a variety of different speeds. Pick one. For the F7, the data sheet says 40,000 lbs @ 9.3 mph.

**3. Convert that rating to metric units.** This converts to 177928.86 Newtons @ 14.9669 km/hr.

**4. Input this speed into the formula,** using your maximum horsepower as *P*. Use 0.85 for your efficiency coefficient. For the F7, that looks like this:
$$T=2650\frac{0.85 * 1500}{14.9669}$$
When we work that out, *T = 225748.1509 Newtons*.
**5. Convert your calculated tractive effort value to pounds.** I'm lazy, so I use Google's built-in converter. 225748.1509 Newtons converts to about 50,750 lbs.

**6. Compare this calculated value to the measured value from your data sheet.** In my case, I calculated 50,750lbs, but the measured value is 40,000lbs. So this means my efficiency coefficient of 0.85 was too high. The locomotive is not 85% efficient, it is less than that. We can tell because we got 10,750lbs more tractive effort than we should have.

**7. Tune your efficiency coefficient.** This will take some fiddling, so I highly recommend creating a spreadsheet to do the math for you. If your calculated value was too high, lower your efficiency coefficient. If your calculated value was too low, raise your efficiency coefficient. For the F7, I found that the optimal efficiency is 72%, which sets the efficiency coefficient at 0.72.

**8. Double check your work.** If we work the same math with the new 0.72 efficiency, we get this:
$$191221.9631=2650\frac{0.72 * 1500}{14.9669}$$
Convert 191221.9631 Newtons to pounds and we are left with about 42,988lbs. This is much closer to the rated value of 40,000lbs, so this coefficient works.

**9. Test the coefficient with other speeds.** You might be able to find another rating for your locomotive (at a different speed). Test the same coefficient at that speed, and see how close you are. This is where a spreadsheet can come in handy, because you can adjust the efficiency coefficient and see instantly how it affects your tractive effort values. I have created an example spreadsheet on Google Docs, which is available [here.](https://docs.google.com/spreadsheets/d/1CWs0fOG9Q3OgsK_oHv4MgxoOLBmlDokxzW71WFaxru8/edit?usp=sharing) You can also make your own spreadsheet, and I have included a tractive effort table for the F7 at the bottom of the page.

**10. Find the speed where the equation returns a lower value than your starting tractive effort.** For the first 5-10 miles per hour, you need to just use the starting tractive effort and ignore the equation. The speed where the equation matches the starting tractive effort is the speed where you start using the equation. For the F7, this is 8.9mph, but if you're using another locomotive you'll have to find it.

Once you've found the speed where the equation starts, as well as tuned the efficiency coefficient to match the measured values, you can use the equation to find the tractive effort for any throttle position, not just the max horsepower output (RUN8).

## Examples

---

**1. An F7 is moving at 10.2mph, and is in RUN2. Find the tractive effort.**

10.2mph = 16.41531km/hr

1500HP * 2/8 will be our horsepower. The 2/8 is because we're in RUN2 out of 8.

$$T=2650\frac{0.72 * (1500 * \frac{2}{8})}{16.41531}$$

So T equals approximately 39,193lbs.

---

**2. An F7 is moving at 40mph, and is in RUN6. Find the tractive effort.**

40mph = 64.3738 km/hr

1500HP * 6/8 = 1125HP

$$T=2650\frac{0.72 * 1125}{64.3738}$$

So T equals approximately 7,496lbs.

*See how much tractive effort you lose with speed?*

---

## F7 Tractive Effort Table

| Speed        | Tractive Effort |
|--------------|-----------------|
| 0 (Starting) through 8.8mph | 56,500lbs       |
| 8.9mph       | 45,000lbs       |
| 13.4mph      | 30,000lbs       |
| 15mph        | 26,786lbs       |
| 25mph        | 16,035lbs       |
| 30mph        | 13,363lbs       |
| 40mph        | 10,021lbs       |
| 60mph        | 6,681lbs        |

---
That's all for this chapter. In the next chapter, we'll discuss a few quirks to the tractive effort formula, and some other forces that act on a train.