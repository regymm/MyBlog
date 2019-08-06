---
layout: post
title: "Virtual Machine the Personal Uses"
data: 2019-08-05 19:47:00 +800
categories: [Experience, Tips]
---

I've been using virtual machines since I'm in high school. I thought it's cool then, and still now. I'm just an ordinary student and use computers to do daily tasks, and some programming and some funny things. But virtual machine can still make my(and our) life much easier and funny! Here are my funny experience of using virtual machines, and you may also want a try =)

I wanted to write this long ago, so some of the cases described happened more than two years ago, so I can't remember clearly what happened, and I'm not able to find enough pictures of years ago. Sorry for that. 

The title sounds big but this post is not technical and I'm just going to talk about how to use virtual machine for daily use, not for study the machines themselves. I'm just a silly user and don't know the things behind these machines. 

Virtual machine is really a bless. 

# 1. Be a normal user - use VM to run another OS

Wanna have a taste of Linux on you Windows laptop? Can't use a software because you are using Mac? Just use a virtual machine. There is not much to talk here.

# 2. WSL is not enough!

**Just in case of future misunderstanding, when I'm writing this WSL2 hasn't come out yet.**

Now I'm using ArchLinux, but I've been using Windows 10 for several months. And of course, I use WSL every day. From a single command to coding a (not very) big project, WSL is just easy and comfortable to use than the native Windows command line, especially for coding stuff. I use CLion and my heavily customized VIM in WSL. CLion for relatively bigger C/C++ projects, and VIM for small things and other languages, like assembly and markdown, for example. About Python? I just use PyCharm with Windows python, it's no big difference. 

Sounds perfect? 

The problem is, some times I still need to use commands like `dd` and `losetup` which are not fully supported in WSL, like when you are burning your compiled assembly onto disk image files and booting stuffs in QEMU. Just as an example. You may find something you can't do in WSL but can in a REAL VM. Then you are in a dilemma: WSL's so convenient, you've already configured it to suit you needs, but a few commands just stall you from doing all your work in it. A VM is a heavy thing, you don't like to work in it all time. 

**My solution:  SSH + shared folders**

WSL works well with SSH, so why not boot a VM, put it aside, and access it in WSL using SSH? Install guest addition, setup shared folders, then you'll have the same feeling as working in WSL. But this time all stuff works because it's real Linux. And, you don't need all you toolchains installed in the VM, you can just do the `losetup` and `dd` in VM, then run the QEMU in WSL as usual, so no need to install QEMU in you VM! 

Also, since your VM is just to be put aside and accessed by SSH, running simple commands, you can just close the GUI, it'll save a lot of processor time and (maybe) memory. Allocate it 20G disk space and 1G RAM, it'll be enough. It's a bless for low-end laptops. 

# 3. VM + CHROOT

**I know there are many better solutions for this, but ...(I don't have a good excuse here, but I just did all these stuff...)**

Another case: I'm using Debian, but I want to use certain softwares that runs better on Deepin Linux, like QQ and WeChat. Many Chinese Linux users may meet this problem. I don't want to use a heavy VM just to run simple softwares. Actually maybe after a painful tweaking to solve those libraries and dependencies, I can run the softwares directly on Debian, but the time spent doesn't worth it, and I may not have that ability. Like I can install `rpm` on Debian using some fancy tools after trying for a day or two, but the procedure is just too inconvenient to be a part of everyday life. 

They are both Linux, it's awful to be unable to run softwares from another distro. So, why not have a try of `chroot`? 

First, install a Linux VM as usual, it may be Deepin, Fedora, or whatever your app runs on. Then, **mount the virtual disk in your host machine, bind mount system partitions, setup correct display, chroot, and run whatever you want.**

As an example, here are some parts of my script:

Mount the VMDK file.

```
vdfuse -w -f "$VMDISK_PATH" "$VMDISK_MOUNT_PATH"
```

Use host network configuration:

```
function updateresolv()
{
	mv -f $DEEPIN_ROOT/etc/resolv.conf{,.bak}
	cp -a /var/run/NetworkManager/resolv.conf $DEEPIN_ROOT/etc/resolv.conf
	echo "-->update resolv.conf done: $?"
}
```

Setup display:

```
	export DISPLAY=:0
	xhost +local:
```

Run WeChat in Deepin wine:

```
			chroot $DEEPIN_ROOT su $U -c "env WINEPREFIX=/home/$U/.deepinwine/Deepin-WeChat/ $DWINE \"C:\\Program Files\\Tencent\\WeChat\\WeChat.exe\"" &
```

where `$U` is my username.

It works, but not without problems. The Chinese input method just won't work in the chroot-ed windows, and fonts are a little bit weird. But anyway, it works. Things like deepin-terminal works without any problem. And with a bind mount of your home directory, file access is easier than in VM. 

By the way, there is a tool called `xchroot` (or `arch-chroot` in ArchLinux) that can automatically do those bind mounts.

# 3. "Dual" boot with GRUB

Scenario: You have migrated to Linux, but still need to use Windows in VM sometimes. You hate reboot your computer to another OS, closing all those working windows which have already filled several virtual desktops. 

Your VM, maybe a Windows 7 with some basic softwares installed, is enough for daily use, but it's far from complete compared to the (maybe Win10) Windows installed on your physical disk. So, do you feel excited if you can boot the Windows lying in physical disk directly in VM, while also be able to boot it physically? Actually, it's quite possible (and simple) to do so. There are many tutorials about this. Just make sure you have chosen the right partition, and don't make silly mistakes, your data will be safe, at least in my case. I do have lost some data in other cases but not in this case.  By saying this I mean I won't be responsible for your data loss.

The problem is, having the VM using a physical disk partition is quite simple, but let Windows boot correctly is not. 

As by default, Win10 is install to a big NTFS partition, but it's bootloader is placed in the EFI partition, shared with Linux, which is mounted on `/boot/efi` in Linux. So, things will be simpler if you keep the EFI partition mounted in Linux, passing only the NTFS partition to VM. But now how can the VM boot? No worries, GRUB will do it. Having a grub ISO file and a Windows installation ISO file at hand, then all difficulties will disappear. 

The grub ISO file can be made by `grub-mkrescue -o grub.iso` command. Or simpler, just take a Ubuntu (or other distro) LiveCD, it'll contain a grub bootloader, boot the LiveCD, press `c` at the boot menu, then there will be a grub commandline. With a grub commandline, you have power to boot other EFI files(Like Windows bootloader), directly load and boot Linux, load another grub, boot directly into ISO LiveCD, and much more. Maybe I'll write a passage on this later? 





However, I don't use this configuration frequently, because Win10 has too much stuff running in the background( mostly spyware and malware I guess =) ), so my laptop heats a lot and the fan keeps running at high speed. I hate both. Win7 is better for me. But probably Win7 cannot be booted both in VM and physical machine as Win10. I tried it but failed, and didn't try further, though I think I can succeed. I gave up partly because my 8th generation Intel processor does not officially support Win7.  And as Virtualbox doesn't support Win7 to boot in UEFI mode but I prefer Virtualbox to VMWare and others. Why UEFI mode? because my disk is GPT, it also shows up as GPT in VM, and GPT disk doesn't support BIOS boot. If you really want to try disk, think twice. What I get best is, I copied the Win7 in Virtualbox to a physical partition(GPT disk), setup UEFI boot in VMWare successfully, then I tried to boot this monster physically. It went to black screen, but I can here the boot sound, and after typing `Windows+R` then `shutdown /s /t 0` then Enter, I can here the shutdown sound and the computer turned off peacefully. It's the driver problem I thought, if I were able to inject drivers, I may succeed? Tired of it though. Never want to try again. 

# 4. "Dual" Boot VHD 



# 4. GPU intensive jobs on Linux?