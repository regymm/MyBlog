---
layout: post
title: "PYNQ Tutorial 2: Overlay"
data: 2019-02-06 11:00:00 +800
comments: true
categories: [Tutorial]
---

This is the second part of the PYNQ tutorial series. This article is about basic knowledge and usage about overlay, and preparations for the next ones in which we need to create our own overlays to satisfy our specific needs. 

## So What's an Overlay?

As is said in [documentation](https://pynq.readthedocs.io/en/v2.5/pynq_overlays.html), an overlay is actually a "hardware library", it's an configurable FPGA design that can be loaded into the hardware(PL), and let users in PS to control: it extents the user application from PS to PL. And the advantage of PYNQ is the control can be done in Python(running in PS). 

If you are using existing FPGA based libraries to accelerate things, like image processing, only dealing with the Python API part is enough(and so is this article): the FPGA part is hidden behind the overlay. 

And of course you can, and probably have to, create your own overlay library and own Python interface: doing both software and hardware engineering, which will be more difficult. We'll talk about this in the following tutorials. 

## Have a Try of the Default Overlay

The *base* overlay is the one get loaded into PL on boot time, the default one, which acts like a reference. 

And new overlays can be loaded when the system is running using Python libraries. Like doing this will load the base overlay again:

```python
from pynq import Overlay
overlay = Overlay("base.bit")
```

Then you can explore the overlay a bit, like reading status from SW1 and SW0:

```python
overlay.switches_gpio.read()
```

Or control the 4 LEDs LD0 to LD4:

```
overlay.leds_gpio.write(0,0b1101)
```

But here we are not using the `BaseOverlay` class and it's comfortable APIs, but directly using raw `Overlay` class and raw PS/PL communication `read` and `write` methods, directly based on the `base.bit` overlay file(yes, an overlay contains a bitstream). This is more similar to what we are doing with our own overlay in the later tutorials. 

Now the `BaseOverlay` Python API version:

```python
from pynq.overlays.base import BaseOverlay
base = BaseOverlay("base.bit")
```

Control an LED:

```python
led0 = base.leds[0]
import time
for i in range(20):
    led0.toggle()
    time.sleep(.1)
```

Compared with the above method, this one looks more like pure software -- this is what overlay and the Python API for. 

And of course, there are more powerful overlays like the *logictools overlay*. As is said in [documentation](https://pynq.readthedocs.io/en/v2.5/pynq_overlays/pynqz1/pynqz1_logictools_overlay.html), you'll have the power to write Finite State Machine for FPGA from a Python description, write combinatorial logic functions, and capture IO signals to PS DRAM. Sounds very interesting. 

## What's Next?

Because my project focuses on the PL part, using existing overlays is not enough. So I'm going to package my PL part in an overlay, and do necessary control using Python in the PS. 

In the process I need to package custom IP cores, create a block design, and deal with the communication between the PL and PS, then finally create an overlay. 

