---
layout: post
title: "A noisy refrigerator and the hacker spirit"
data: 2025-08-22 17:00:00 +800
comments: true
categories: [Experience]
---

Have you ever stayed in a hotel room with a refrigerator? The periodic buzzing might be a minor hassle, but either unplugging the power cord or using up a pair of earplugs from the drawers of the nightstand makes a quick solution. 

It becomes slightly complicated if the refrigerator is in you own home. A rented one where you can't and don't want to throw it away. 

It's common sense and I know this "issue" will get rid of itself after a few days, human body is very good at ignoring such subtle annoyance. Thus, every attempt to deal with it will inevitably become premature optimization. 

### Solution 1

But for tinkerers and hackers, "sleep it over" is no option. The first "solution" came to thought is to power it off during night hours -- but this will certainly have bad influence on the food inside the fridge. An improved method is to only cut the power during the "before-sleep" period, as the influence to the fridge will be minimum. This requires some program control, but the exact time period may depends on the sleep quality, and need many days to measure. Also, personally, a "timed turn on" will quickly become a mentally deadline to fall into sleep, and will probably have the reversed effect of forcing me to stay awake until timer goes off and. 

One ultimate, "improved" solution will be use a smart watch to monitor the sleep, and only turn on the fridge after confirmed deep sleep entrance, and use another mechanism to monitor the turnoff duration, and calculate a "spoil index" to be a correction factor when reading the expiration dates for food inside. Maybe an overshot pre-cooling for the fridge before sleep, but make sure the temperature is above zero if there's vegetable inside. This, however, requires wearing a smart watch every night, and a full set of API and sleep detection, also either a digital-to-analog controller wired into the fridge itself to override the default work cycle (for pre-cooling), or a smart socket (for just power cutoff). An always-on MCU/SBC is needed, posting extra fan noise/LED light considerations. 

These are certainly too complicated, and a smarter way will be to reduced the noise itself, or damp it from being heard. 

### Solution 2

I dropped my head to the ground and poked the lower back side of the fridge. The buzzing sound is very clearly from the compressor (I assume, it's a metal, black painted cylinder), and is amplified by the empty spaces in the fridge and the copper tubes (certainly cryofluid circulates within). The tubes are bended, almost touching each other, vibrating when liquid flows. I quickly did some "optimizations" to bend the tube towards empty spaces, and it seems to reduce the vibration a little bit. Also tried attaching some plastic bags to the area, but this may cause overheat and fire hazard. 

About the noise itself, the empty area of the fridge certainly acts like some kind of cavity, and the noise may reflect from the room wall. I turned the fridge by around 30 degrees, which gives a negligible improvement -- it was turned back to standard position to fit in a shelf later, and I didn't feel a thing.  

### Solution 3

With the reduced vibration noise, I can tell apart that there is still a liquid circulation sound lie within. A quick search suggests that this might be due to a lack of cryofluid inside, often happening to old fridges. Then, can the fluid be refilled? A another quick search shows it's possible, requiring just two major parts: copper soldering and vacuum pump. Usually, tutorial shows some repairman with shady equipments. Nothing big? I've seen someone blasting a copper air-conditioner tube with lovely green flame, and I work with vacuum equipments quite a lot. The [detail](https://www.youtube.com/watch?v=arFVhgoZG0o) usually comes up as vacuum pumping, gas filling, a long and patient debugging of the amount, and finally sealing up. I will need to have access to a pump, a flammable compressed gas cylinder, a blowtorch, soldering metal, cutter, valves and tubes. Maybe the same set of equipments can help making some liquid nitrogen also, which is a big plus.

No, if you haven't already realized, I didn't fill cryofluid into a fridge by myself. The risk/reward just doesn't worth it. 

### The final tradeoff

So what's the best solution? As I found out the no options IS the best solution. I still want to hold a blowtorch and blast some copper tubes some day, but not today. 

Will you think the same when witnessing a noisy refrigerator? What's your hacker spirit? 

---

I cloned the blog repo on my laptop before departure, hoping to jot down a few lines during a trip, but no surprise, nothing was touched during the whole week. 

There are places tempting you to stand up, like concerts and tours, probably just because everyone else does so. 

There are places tempting you to sit down, like the fuwa-fuwa casino carpets.

There are sounds urging you pull all in, where most don't even know they're gambling at all. 

With hollow, blood-shot eyes staring the back of the fronter row seats, 

and another night under an unfamiliar ceiling, 

dreamed of waking up on a subway platform, unable to tell which city it is,

I'm glad I still remember the hacker spirit. 
