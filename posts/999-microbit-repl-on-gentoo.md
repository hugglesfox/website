# Accessing the BBC micro:bit REPL on Gentoo
tags: embedded, linux, gentoo, microbit

_Published 2019-02-09_

![The microbit repl in a terminal](/media/microbit_repl.png)

Thanks to the [National Computer Science School](https://ncss.edu.au), I'm now
the proud owner of a BBC micro:bit. The micro:bit has the ability to provide a
python REPL over USB serial but when plugging it into my Gentoo machine, the
micro:bit doesn't show up in `/dev`. What gives?

Like most things, a kernel module is required to provide the drivers for the
USB serial interface. On first inspection you would think that
`CONFIG\_USB\_SERIAL\_CONSOLE` is the correct module, but no. The micro:bit's REPL
is actually treated as a USB modem which requires `CONFIG\_USB\_ACM` to be set.

## Kernel Configuration

```
Device Drivers --->
	[*]  USB Support --->
	    <*>  USB Modem (CDC ACM) support
```

Now that the module is enabled, [rebuild the
kernel](https://wiki.gentoo.org/wiki/Kernel/Rebuild) and reboot. Once rebooted
plug in the micro:bit and there should be a new device in `/dev` called
`/dev/ttyACM0` (or something similar). Now it's as simple as connecting to your
micro:bit REPL using your serial console client of choice!

```
picocom /dev/ttyACM0 -b 115200
```

The [micro:bit documentation](https://microbit-micropython.readthedocs.io/en/latest/devguide/repl.html) as some more examples. 
