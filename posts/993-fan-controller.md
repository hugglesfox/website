# Custom fan controller for external water cooling radiators
tags: embedded, pcb, esphome

_Published: TBD_

My partner's PC has a cool external water cooling setup where her radiators are
outside the computer case, allowing her to have more radiators then the case
would fit as well as to move the fan noise away from earshot. Though this comes
at the trade-off of either needing to run very long fan and ARGB cables from the
motherboard or having a separate controller.

So I thought, how hard could it be to make a little board with an ESP32,
connect it up to homeassistant, get it to read the liquid temperature from a
standard PC water cooling temperature probe and control a few 4 pin fan and 5v
ARGB headers? ... Famous last words.

## PC hardware

First I should cover the PC hardware which I intend to communicate with.

PC PWM fans have 4 lines

- 12v power
- Ground
- 25kHz PWM signal for speed control
- Tachometer signal

The tachometer signal is an open collector which pulses twice each revolution
of the fan. Meaning that my board just needs a resistor to pull the tachometer
signal to 3.3v for the ESP32 and it just needs to count the number of times in
a minute which the line gets pulled to ground to calculate the fan's RPM.

Additionally the RPM signal's logic low is defined as anything less then 0.8v
therefore it should be fine driving it with a 3.3v GPIO pin and an ESP32c3
should be able to generate 25kHz using it's onboard PWM controller.

This all sounds really promising, I was expecting to need to use extra
components for the 12v fan to communicate to the ESP32 without frying it. I
should be able to just wire it straight up and be fine as the fan doesn't put
12v on any of the signal lines.

Next is the lines for the all important _(satire)_ RGB

- 5v power
- Ground
- Data

From what I can tell the data line uses a variation of the ws2812 protocol. It
should have an addition to it to be able to count how many LED modules are
connected in the strip though I haven't figured out how that works yet. Shoving
ws2812 signals at the data line gets them to light up though.

Finally PC water temperature probes are NTC thermistors which have a resistance
of 10kΩ at 25°C and typically a B-constant of 3950 (this will probably get you
close enough though the actual value will vary from manufacturing tolerances).
Hence we can measure it using a resistor divider and the ESP32's on-board ADC.

## PCB Revision 1

So I designed up a PCB in an afternoon, sent it off JLCPCB and in a few weeks I
had a board. Also I know it's unusual to use through hole ceramic capacitors, I
decided that I wanted to only use through hole components for this project
(but then I went and used pads for the ESP32?).

It features

- 12v DC barrel jack input
- L7805 5v 1.5A linear regulator for powering the RGB and the ESP32
- DHT22 for ambient air temperature sensing

Though it had some issues

- The pins used for ARGB connectors, whilst using the standard 2.54mm pitch, are way thinner then standard pins so the connectors didn't fit
- 1.5A was no where near enough to power all the RGB which was going to be connected to it
- I ran the tachometer line parallel to the PWM line without putting in any filtering capacitors

This last point meant that, through capacitive coupling, the high frequency PWM
signals were showing up on the tachometer line making my fan read that it was
running at multiple thousands of RPM (the max RPM of the fan is 1200).

Not a great start. However I was able to confirm that PWM fan control was
working and I was able to get the ARGB working too after hacking up some dupont
cables and using a PC ATX power supply to feed the 5v line.

## Resources

Things which I referenced during the design and construction

- <https://cdn.noctua.at/preview/media/Noctua_PWM_specifications_white_paper.pdf>
