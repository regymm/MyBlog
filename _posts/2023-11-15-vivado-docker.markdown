---
layout: post
title: "Dockerized Vivado for Compilation Services"
data: 2023-11-15 15:00:00 +800
comments: true
categories: [Experience]
---

![](/MyBlog/images/vivado-battle.png)

### **Background**

If you've been following up or peeking on me, you'll know that an online FPGA compilation project called CaaS, or compiling-as-a-service, is being actively developed. It'll provide easy-to-use demos, online editors, and, more importantly, FPGA toolchain for compilation on remote servers so users don't need to install the heavy and/or hard-to-setup software on their own machines. 

What toolchains? You name it. The newly-emerged [OpenXC7](https://github.com/openXC7), for the 7-series Xilinx devices, will be the main target -- imaging doing Kintex 7 xc7k325t development in the browser. More or less matured [YosysHQ](https://github.com/YosysHQ) projects that targets Lattice's ECP5 and ICE40 are also on the list. For apparent safety reasons and easier management, these toolchains run in Docker, and are called with limited memory and life time. You don't want user's genuine mistake to eat up 30 GB of memory, or a deliberate `$system("rm -rf")` to let bad things happen. 

The server is now at [https://caas.symbioticeda.com/](https://caas.symbioticeda.com/), and another testing site, more like my own backyard and less carefully maintained, will be deployed at [https://fpgaol-ce.top/](http://fpgaol-ce.top/) soon. 

### **Then...**

Where's Vivado then, the official Xilinx IDE? It's probably not very legal to host it on public server, but having it as a tester backend or share the Docker on private servers between different machines at home feels something worth trying(so in this article license's off the table). 

First, the usual GUI-based installation can be done with scripts: we can use `xsetup -b ConfigGen` to generate a install_config.txt, and customize the options, like this one with only 7-series enabled, installed to /opt: 

<details>
<summary>install_config.txt</summary>
<code>
#### Vivado HL WebPACK Install Configuration ####<br>
Edition=Vivado HL WebPACK<br>
<br>
# Path where Xilinx software will be installed.<br>
Destination=/opt/Xilinx<br>
<br>
# Choose the Products/Devices the you would like to install.<br>
Modules=DocNav:0,Kintex UltraScale:0,Spartan-7:1,Artix-7:1,Model Composer:0,ARM Cortex-A53:0,Zynq UltraScale+ MPSoC:0,Zynq-7000:0,SDK Core Tools:0,ARM Cortex-A9:0,ARM Cortex R5:0,System Generator for DSP:0,Kintex-7:1,Kintex UltraScale+:0,MicroBlaze:0<br>
#Modules=DocNav:1,Kintex UltraScale:1,Spartan-7:1,Artix-7:1,Model Composer:0,ARM Cortex-A53:1,Zynq UltraScale+ MPSoC:1,Zynq-7000:1,SDK Core Tools:1,ARM Cortex-A9:1,ARM Cortex R5:1,System Generator for DSP:0,Kintex-7:1,Kintex UltraScale+:1,MicroBlaze:1<br>
<br>
# Choose the post install scripts you'd like to run as part of the finalization step. Please note that some of these scripts may require user interaction during runtime.
InstallOptions=Enable WebTalk for SDK to send usage statistics to Xilinx:1<br>
<br>
## Shortcuts and File associations ##<br>
# Choose whether Start menu/Application menu shortcuts will be created or not.
CreateProgramGroupShortcuts=0<br>
<br>
# Choose the name of the Start menu/Application menu shortcut. This setting will be ignored if you choose NOT to create shortcuts.<br>
ProgramGroupFolder=Xilinx Design Tools<br>
<br>
# Choose whether shortcuts will be created for All users or just the Current user. Shortcuts can be created for all users only if you run the installer as administrator.
CreateShortcutsForAllUsers=0<br>
<br>
# Choose whether shortcuts will be created on the desktop or not.<br>
CreateDesktopShortcuts=0<br>
<br>
# Choose whether file associations will be created or not.<br>
CreateFileAssociation=0<br>
<br>
# Choose whether disk usage will be optimized (reduced) after installation<br>
EnableDiskUsageOptimization=1
</code>
</details>
<br>

And use `xsetup --agree XilinxEULA,3rdPartyEULA --batch Install --config install_config.txt` for the installation, no matter in a Docker environment or just on host machine. 

It should be ready then, isn't it? Partly due to my laziness and partly due to the long install time, I didn't use two-stage Docker build but just archived the installation directory once, and use a single line of `ADD ./Xilinx_Vivado_2019.1_Installed_ForDocker.tar.gz /` in the Dockerfile. 

### **Size**

The only thing is, as of 2019.1, this very "basic" installation takes over 20 GB of space. After `ncdu`-ing the installation directory, you'll soon find out not all of this are needed. 

Following the heaviest directory, you'll get to a 1.8 GB directory called `data/parts/xilinx/devint/vault/versal`. Versal? [Here](https://support.xilinx.com/s/question/0D52E00006ln0TnSAI/question-regarding-vivado-20211-installation-size-and-device-support?language=en_US) states that: 

>  ... we can't currently remove this, as ML standard can be upgraded to ML Enterprise and Vitis. If we remove vault directory, when later on you want to upgrade to include versal, the versal flow will fail. 

But this just won't happen, and for every non-versal player it's 1.8 GB wasted. 

Similar things have been stated at [here](https://support.xilinx.com/s/question/0D72E0000069NoWSAU/detail?language=en_US) -- in that case of 2020.2, the versal directory has grown to 5.9 GB. Hmmmmm. 

If you're sure that a certain thing won't be used, more radical trimming can be done, like in the Docker no simulation will be performed, and I just purged the data/xsim, saving another 2.2 GB. 

At the end I got the the following trim list:

```
RUN tar xf /Xilinx_Vivado_2019.1_Installed_ForDocker.tar.gz && rm /*.tar.gz && \
		rm -rf /opt/Xilinx/Vivado/2019.1/data/parts/xilinx/devint/vault \
		/opt/Xilinx/Vivado/2019.1/data/xsim \
		/opt/Xilinx/Vivado/2019.1/data/embeddedsw \
		/opt/Xilinx/Vivado/2019.1/data/simmodels \
		/opt/Xilinx/Vivado/2019.1/tps/lnx64/git-2.16.2 \
		/opt/Xilinx/Vivado/2019.1/tps/lnx64/git-1.9.5 \
		/opt/Xilinx/Vivado/2019.1/ids_lite \
		/opt/Xilinx/Vivado/2019.1/msys64 \
		/opt/Xilinx/Vivado/2019.1/examples \
		/opt/Xilinx/Vivado/2019.1/hybrid_sim
```

There're even two versions of git costing over 1.5 GB summed up. There're certainly more things to do, like how about deleting ZYNQ-related IP cores because usually few people use ZYNQ without a interactive block design? But anyway, after this, the Docker imaged is shrinked to 11.1 GB: I'm fine with this for now. 

One ultimate measurement would be use some kind of access tracker to see what Vivado really accessed during some typical project compilations (I believe Project X-ray fuzzers would be a good choice), and drop everything else. If anyone did this, please tell me! 

### **Alpine**

Using Alpine as the base docker image can save some space, but somehow I didn't get Vivado working on this musl-based Linux distribution: the `libtinfot5` library on Ubuntu just doesn't have an alternative on Alpine, I tried replacing and migrating some .so files extract from Ubuntu directly, but still Vivado can't find the shared libraries. 

### **Dockerfile**

Finally, the Docker file as on [FPGAOL-CE GitHub](https://github.com/FPGAOL-CE/osstoolchain-docker-things/blob/master/vivado/Dockerfile.vivado-lite), not that well written but is what I'm using now: 

```dockerfile
FROM ubuntu:20.04 AS builder

COPY ./Xilinx_Vivado_2019.1_Installed_ForDocker.tar.gz /

RUN tar xf /Xilinx_Vivado_2019.1_Installed_ForDocker.tar.gz && rm /*.tar.gz && \
		rm -rf /opt/Xilinx/Vivado/2019.1/data/parts/xilinx/devint/vault \
		/opt/Xilinx/Vivado/2019.1/data/xsim \
		/opt/Xilinx/Vivado/2019.1/data/embeddedsw \
		/opt/Xilinx/Vivado/2019.1/data/simmodels \
		/opt/Xilinx/Vivado/2019.1/tps/lnx64/git-2.16.2 \
		/opt/Xilinx/Vivado/2019.1/tps/lnx64/git-1.9.5 \
		/opt/Xilinx/Vivado/2019.1/ids_lite \
		/opt/Xilinx/Vivado/2019.1/msys64 \
		/opt/Xilinx/Vivado/2019.1/examples \
		/opt/Xilinx/Vivado/2019.1/hybrid_sim

FROM ubuntu:20.04

ENV ALLOW_ROOT 1

ENV TZ=Asia/Seoul
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y libtinfo5 libncurses5 libx11-6 make --no-install-recommends

COPY --from=builder /opt /opt

ENV LD_PRELOAD /lib/x86_64-linux-gnu/libudev.so.1
ENV PATH /opt/Xilinx/Vivado/2019.1/bin:$PATH

CMD ["/bin/bash"]
```

Note that at the end, the LD_PRELOAD is needed for some potential crashing, and setting the PATH is enough, sourcing the `settings64.sh` does the same thing. 