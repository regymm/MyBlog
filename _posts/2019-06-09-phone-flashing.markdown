---
layout: post
title: "Experience About Phone Flashing"
data: 2019-06-09 11:43:00 +800
categories: [Tweaking, Personal]
---

After 8 months I finally want to continue on my blog, in the three-day-vocation of the Chinese Dragon Boat Festival. 

Put it in a nutshell, I spent some time to flash my phone, it succeeded, and feels pretty good. 

So let me share my experience here!

# About my phone, motivation, and the past

I bought a new Huawei Honor View 10(BKL-AL20, 6G+64G) last year, and when I was choosing which phone to buy I had thought about flashing, rooting, and things like these. I don't want to suffer from bloatware and malicious apps, so root permission is a must, and tweaking software like greenify and appops should be installed. 

On the first day I got the phone I had the bootloader unlocked, buying a service on Taobao which cost me RMB 25. Then I did a lot of tweaks, and it works fine, at least for daily use.

However, Huawei's EMUI do weird things, the power manager is a mystery, I have to disable the powergenius app just to get my proxy software running in background without being paused. But at the same time, awful apps like Taobao and Jingdong, also QQ and WeChat, never get paused or "frozen" even I strongly want them to stop using my battery. Since powergenius is disabled, these apps won't get paused, so I have to freeze them(disable them in system, and enable when I want to use) using another strange app. That's weeks of tweaking. Full of tears. 

What's more, rumors about surveillance and safety problems keeps haunting me. I really want my privacy protected, though I don't know whether it is possible. 

So why not try something new? 

# Preparation

I've been thinking about flashing the whole week. I don't know why but the feeling is strong. First I backed up my photos and files, also some apps, downloaded LineageOS flashing package and read some tutorial. I thought I'm ready. 

Of course I was nervous about it, because if I failed and bricked my phone, I will have to spend a lot of time, even money to have it fixed. What's more, the final term examinations is coming, and my homework wasn't done. Even if it works, I have to spend time installing software and dealing with it, I don't know whether there will be further difficulties. As a student, my time is precious. 

So on Friday night about 9:00pm, at school's library, I connected my phone to my laptop, thought twice, entered `adb reboot recovery` at command line, and pressed enter with my trembling finger. 

# Exciting moments

**9:00pm to 13:00pm.** LineageOS didn't work, the ZIP just didn't be flashed, error occured. I found the same error on XDA but the poor solutions also didn't work. Someone said maybe a full data wipe can work? I tried to wipe the whole Internal Storage, which means losing all data. Then, guess what, the TWRP stucked on the wipe! Also, there are "solutions" in forums on the Internet, some said this will last for 30mins to 5hrs, but can I wait? Not! 

Pressing the power button, I rebooted my phone by force. Then, strange things happened again: It cannot boot! Even boot into recovery, or the OEM's erecovery! What I have access to is the bootloader. But, "it's enough and I can deal with it", I said to myself, "I've handled bricked device before, no need to panic!". 

It's late now, I want to go back to dorm to enjoy my holiday.

But since I've already wiped almost everything and my phone is bricked, there's no way back. 

**To be continued...**