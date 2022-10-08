---
layout:	post
title:	"My Apocalypse"
date:	2022-10-08 22:00:00 +0800
categories: [Experience, Artwork]
---
*WARNING: this passage makes no sense.*

**Part I**

Recently I got interested in gathering junk electronics. In a newly arrived parcel, a special board found itself among various old-fashioned PCI cards and ISA-like adapters. 

The board's not new. Time and lack of care have certainly left mark on this poor being, but still, it distinguished itself even to mediocre eyes like mine: PCIe x4 interface, a matrix of DDR3 memory chips, and high-speed connectors I cannot name. The main chip had been decapped, revealing the silicon die. And certainly the decapper damaged the PCIe traces and capacitors when doing his/her rough job. Luckily and surprisingly, I could still easily identify it as a Kintex Ultrascale -- from footprint comments, nearby 6-pin differential oscillator, and length-tuned differential lanes going into to suburbs of the PCB. 

Not far away from the Ultrascale lay a BGA-packaged MCU, a NAND-like flash, and two pin headers -- again, one was lucky to have the familiar 2x7 pin Xilinx JTAG standard shape. Canonical MCU + FPGA thing, huh. 

Then it was the old profession of FPGA evacuators - alligator the power lines and hook up a JTAG adapter. Things went on smoothly: an xcku035 shown itself without much hassling. 

---

**Part II**

The downfall came when I was trying to download bitstream into the chip. 

I looked up the Ultrascale series datasheet. 

I found wrong voltage level on pins named INIT and DONE. 

I saw more crushed traces on the PCB. 

I destructively removed the BGA MCU. 

I cut wires and flew wires. 

I tweezered untented vias when power on. 

It worked. But what next?

No clock, no input, no LED. I should be able to get clock from the differential oscillator, but sadly the whole high-speed transceiver banks had certainly been damaged. The ethernet PHY should have output 25 MHz but it just didn't. I had no way but to fly another wire from the MCU single-ended crystal to a via under the FPGA. I also had an LED soldered on another via, it's easier said than done. 

Now blinky could finally work on this crippled board. Was I excited? No way. No one would be happy about a thousand-dollar chip doing no more than blinky. 

And where went the board? Deep inside my drawer. 

---

**Part III**

Months later, maybe by sheer luck, I got to know what this board really was. It was the control board of some smart RAID devices. 

Once gigabytes of data passed through the silicon and copper every second. Now it barely competes with an ice40. 

The time has passed, and there is no way for me to restore even a slight amount of its original power. Delicate electronics crushed by time, entropy, and my soldering iron & heat gun. 

Kintex Ultrascale, how high a device. Now with damaged GT, unknown DDR pinouts, and IOs less than one hand can count, it really can't do more than lie deep inside a drawer, and I really can't do a thing about it. 

Lost of function, lost of knowledge. 

Crushingly remove the MCU for a mere pin pullup is just like burning books as fuel. The sleeping DDR feels like lingering satellites high above a nuked wasteland. I think I just witnessed a little apocalypse right on my desk. 

In the ourside world meanwhile, millions of electronics roars on, and we rely on them. But how will these end up? Can they escape their fate and end better than this little Kintex? I'm afraid. 

Then if this is not coming apocalypse, what is?

![](/MyBlog/images/my-apocalypse.png)

*(Art inspired by Hyper Light Drifter)*

2022.10.8 by regymm, licensed under CC-BY-SA 4.0
