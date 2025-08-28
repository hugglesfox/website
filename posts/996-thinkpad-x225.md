# Adventures with old thinkpads
tags: thinkpad, hardware

_Published 2023-07-28_

![The thinkpad x220 with the lid open, showing the screen and keyboard](/media/thinkpad/IMG_1527.jpeg)

I needed a laptop for basic portable computing, so I built a laptop based on
the Lenovo Thinkpad X220 and Thinkpad X230.

## Requirements

The primary use case for the laptop will be to act as a device for typing code
and documents and light web surfing whilst traveling. Ideally it should fit the
following design requirements

1.  Must be easily portable (should fit in a 13" laptop sleeve)
2.  Must have a good keyboard
3.  Must be \<$500
4.  Should have a long battery life
5.  Should be repairable (with replacement parts available)
6.  Should be quiet

## The Thinkpad X220

![The thinkpad x220 with the lid open, showing the screen and keyboard](/media/thinkpad/IMG_0812.jpeg)

The Lenovo Thinkpad X220 laptop is a classic and well loved to this day. It has
a great feeling keyboard, small yet built like a tank, good software support
and can be picked up on the second hand market for cheap.

However using a laptop which was designed 10 years ago does come with some draw
backs

* The 2nd gen Intel Core processor is slow and not very efficient
* No USB 3.0 ports
* The screen, whilst IPS, is only 720p

## The Thinkpad X230

![The thinkpad x230 with the lid closed](/media/thinkpad/IMG_0817.jpeg)

The Lenovo Thinkpad X230 is the next generation up from the X220 featuring

* A USB 3.0 port
* 3rd gen Intel Core processor with improved efficiency
* Improved cooler design
* The newer chicklet style keyboard

So why not just use a Thinkpad X230? I dislike the feel of the chicklet
keyboard and couldn't find a fully working X230 for a reasonable price.

Interestingly the Thinkpad X230 has a security vulnerability which allows for
the [embedded controller to be
reprogrammed](https://github.com/hamishcoleman/thinkpad-ec) This unlocks some
neat functionality such as being able to modify the keyboard layout and remove
the battery validation which blocks the usage of third party batteries.

## Combining the two

The Thinkpad X220 and X230 are very similar in their physical design. Therefore
a common mod for the X230 is to replace the keyboard with the [one from the
X220](https://www.thinkwiki.org/wiki/Install\_Classic\_Keyboard\_on\_xx30\_Series\_ThinkPads)
But at the time, there were no undamaged Thinkpad X230's on the market so I
decided to do the opposite and install an X230 motherboard into a X220 chassis.

## The water damaged X230

So I brought a damaged Thinkpad X230 from ebay. It was missing a hard drive and
battery, the case was cracked from being dropped on one of the corners and had
what looked to be some minor water damage to the screen. However the ebay
listing showed it could boot so I was hopeful that the motherboard was ok.

![The mainboard of the thinkpad x230 with rust around the CPU socket and corrosion on the io ports](/media/thinkpad/IMG_0822.jpeg)

Upon taking apart the laptop it was very clear that the laptop had serious
water damage, possibly from salt water. The bottom of the case was covered of
metal corrosion and some of the metal components such as the CPU heatsink mount
on the motherboard had rust spots. Fortunately it still booted and everything
seemed to work.

## Black plastic

A quick detour but possibly one of the reasons why the water damaged
motherboard still worked was because Lenovo wraps their motherboards in a layer
of plastic which is glued to the motherboard with a reasonable adhesive.
Unfortunately that adhesive leaves a sticky residue which can't be easily
removed with isopropyl alcohol. I was able to remove most of it with a citric
acid based adhesive remover but that left a orange smelling residue all over
the board and the only way to remove it was to give the board a wash in warm
soapy water. I then left it for a few days to dry out completely.

![The mainboard of the thinkpad x230 being dipped into a sink filled with soapy water](/media/thinkpad/IMG_0867.jpeg)

## Cleaning up the X220

The Thinkpad X220 which I also brought was in much better shape. The keyboard
keys are a little worn and there's a couple of dings but that's to be expected
of a 10 year old laptop. Lenovo applies a soft touch coating to the top of the
lid and the underside of the laptop in which turns sticky as it ages but that
can be easily removed using some magic eraser.

## Swapping the motherboards

The main physical differences between the X220 mother board and the X230
motherboard are

* The X230 uses a mini DisplayPort port instead of the full sized port on the
  X220
* The front left USB port (next to the Display Port) is slightly longer on the
  X230 compared to the X220

Whilst the different sized DisplayPort ports is only a problem aesthetically
(as the display port hole in the case is larger then it needs to be) longer USB
port does pose a problem as it collides with a support on the bottom of the
case, pushing the USB port up. The laptop will still go together however that
USB port won't line up correctly with it's hole, stopping one from plugging
anything into it. Also bending the motherboard doesn't sound like a great idea.

![A USB port which doens't line up with the cut out in the chassis properly](/media/thinkpad/IMG_0897.jpeg)

Luckily this can be solved by filing down the supports a little until the board
nicely sits in position. The shiny spots in the following image were the areas
which I filed.

![The bottom of the chassis, showing the supports I needed to file down](/media/thinkpad/IMG_0898.jpeg)

## Modding the keyboard

The keyboard connector pinouts for the X220 and X230 are slightly different
such that if you were to directly plug the keyboard in, it would result in 3v3
being shorted to ground. However the pins that do this aren't important to the
keyboards functionality (as far as I can tell, looking at the schematic they
seem to be some sort of identification pins) so we can just cover them with
some electrical tape.

![The top of the x220 keyboard with the function buttons faceplate removed](/media/thinkpad/IMG_0858.jpeg)

There are plenty of [guides on the internet](https://imgur.com/a/fsRe6) which
give a step by step guide on how to do this.

![The keyboard ribbon cable with electrical tape covering some of the contact pads](/media/thinkpad/IMG_0861.jpeg)

## Removing the supervisor password

Putting things back together and powering on the Thinkpad X230 mainboard, we
get greeted by a locked bios. 

![The BIOS lock screen](/media/thinkpad/IMG_0853.jpeg)

Fortunately this is easy to fix. All one has to do is short the clock and data
pins on the security EEPROM (U42) while entering the bios with a screw driver.
The bios will happily ignore that there's a password. From there I cleared the
bios and supervisor passwords then removed the screwdriver and saved the
changes which overwrote the existing password. 

![A small flat head screw driver shorting the data and clock pins of the security EEPROM](/media/thinkpad/IMG_0893.jpeg)

## Conclusion

To finish up I flashed the bios with coreboot, replaced the fan/heatsink
assembly with a new one from ebay as the X230 fan was destroyed and the X220
bearings were worn out and brought a new X230 battery from Duracell.

All in all I'm rather happy with the result. My main gripes with it are the
lack of a USB-C (I've got a plan to fix that) and a short battery life. I get
about 5 hours from a full charge however being able to swap the battery helps.

Extra photos are available in this blog's git repo.
