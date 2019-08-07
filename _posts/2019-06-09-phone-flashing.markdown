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

# An Exciting Process

**9:00pm to 10:00pm.** LineageOS didn't work, the ZIP just didn't be flashed, error occured. I found the same error on XDA but the poor solutions also didn't work. Someone said maybe a full data wipe can work? I tried to wipe the whole Internal Storage, which means losing all data. Then, guess what, the TWRP stucked on the wipe! Also, there are "solutions" in forums on the Internet, some said this will last for 30mins to 5hrs, but can I wait? Not! 

Pressing the power button, I rebooted my phone by force. Then, strange things happened again: It cannot boot! Even boot into recovery, or the OEM's erecovery! What I have access to is the bootloader. But, "it's enough and I can deal with it", I said to myself, "I've handled bricked device before, no need to panic!". 

It's late now, I want to go back to dorm to enjoy my holiday.

But since I've already wiped almost everything and my phone is bricked, there's no way back. 

**To be continued...**









---











Ok, my phone bricked, but at least I still had a bootloader, there's still hope. 

Re-flash TWRP? Seems plausible, but didn't work.

Wait a moment... I had backuped kernel and ramdisk img before using TWRP, have a flash? Still didn't work.

Then I realized because I'd updated to EMUI 9, so the previous backups may have rendered useless.

So what to do now?

**10:30pm**

I wandered in XDA and found that they always download stock firmware, flash it, and then flash custom ROM. So I download stock `update_full_base.zip` from a web link given in the forum. Thanks good-hearted people. Ok, downloading directly from hiupdate.huawei.com:xxxx/xxx/xx was fast, about 2M/s, felt like exploiting the servers :-). 

- Interesting moment: I saw a post on XDA(Of course, English site) that use the word "brush" to express the action "flash", which is a direct translation of the Chinese word meaning flash: "brush the device". Is that Chinglish? Or foreigners also use this expression? Don't know, but it's really funny. 

Grabbing an package extractor from the forum, and I got the latest stock KERNEL.img. Flash, and boot! I booted into TWRP again! But though all data wiped, LineageOS still failed to flash. I had another look at the Honor View 10 section of XDA, there are tons of ROMs, one called AOSiP, or Android Open Source Illusion Project, seems good. Download the ~500MB(it's VERY small compared to the 3GB+ stock ROM full of bloatware!) zip, flash and, voila! Boot into android in less than a minute!

Actually, it took me some time to realize that I succeeded, because the AOSiP system was so minimal! At first glance I thought it was only a recovery or something instead of a real system! There were less than 10 apps! No bloat! I had a look at the sys, pretty good. Fingerprint, works; camera, works; flashlight, works. Seems better than I'd expected. 

I found native Android have an exotic feeling, considering that I've been using Chinese ROM for all these years. 

A change of flavor. Cool.

Meanwhile, I packed my bag, preparing to leave. 

- Interesting moment: I kept quiet when doing these in library, but I really wonder, what will other students think about that guy: Press enter on the laptop, then skillfully unplug phone from computer, press power and volume button together using both hands then strange things appear on the phone screen...

**10:45pm**

Really late, wasn't it? The library was closing, this was the first time I stayed at the library until it closed at night, since I went to the college. The closing music was played out loud, I was tired, sleepy, but both heart high in the sky and a little bit worry. Happy: I've got a working (nearly) native Android running on my primary device, for the first time! Worry: Will anything broken in the sys and then I'm forced to go back to EMUI? And, can I get up early tomorrow? Can I finish my homework?

**11:30pm**

Back to dorm at about 11:10pm. And fortunately, I managed to lay in bed at 11:30pm. Don't want to burn midnight oil. 

And the alarm clock works to wake me up 8 hours later :-)

# Conclusion

Tweaking is interesting. Also time wasting. You may suffer from difficult moments but at last they will become beautiful memories, no matter you succeeded or not. 

**Advice to choose phone if you want to tweak:**

- Check XDA for information, whether there are ROMs and other related tools.
- Wait a few month. A newly released phone, of course, don't have a lot of support. 
- Check whether bootloader is unlock, or can you get it unlocked. Don't bother unlock it yourself, have a search on Taobao, usually it's less than RMB 50 to unlock a phone. 
- Choose a popular phone. This is the same as advice 1. 

**Advice when tweaking**

- Finish your homework first. 

- Backup. 

- Be prepared. 

- Give yourself enough time. Choose a holiday or so. 

- Read carefully. Don't miss a single step. 

- Global and fast Internet access. 

- Calm down. Calm down. Calm down. The world won't end if your bricked your device.

  





Thx 4 reading ~