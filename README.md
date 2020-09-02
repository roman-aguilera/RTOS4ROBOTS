# RTOS4ROBOTS : Hard Real-Time Operating Systems for Robots
This repository is part of a research project that aims to build end-to-end, hard real-time, control systems by integrating open source software tools. 
The emphasis here is HARD REAL-TIME (which significantly narrows down the posiible types of software options one can use). 
This research project is done in an open-source manner because it allows others to freely and rapidly prototype mission-critical projects, such as agile humanoids (life size robots that can run on 2 legs).


This README.md file includes step-by-step instructions for installing Xenomai 3. If you find this repository beneficial, we encourage you to cite it.

So far, this status of this project is having a successfully compiled Xenomai3 dual (codename Cobalt) kernel built on top of Linux version 4.4.71 to be used on x86 target hardware. 
This was done using an x86-based laptop running on the Ubuntu 16.04 operating system.

Current efforts of this project includes building a Xenomai3 kernel to be deployed on arm-based target hardware. This is being done on an x86-based laptop that runs on the Ubuntu 18.04 operating system.
I am currently investigating which mix of proceccors/chips, hardware, microcontrollers, System-on-Chip, and open-source software tools is best suited for building an agile humanoid robot from the ground up. 
If you have any comments, suggestions on hardware, or would like to collaborate, please reach out to me at romanaguilera94@gmail.com, Twitter, or LinkedIn .
If you have a functional and up-to-date hard real-time operating system set up, please reach out so that we can cite your work and build on top of your existing setup. 
The goal of this is to collaborate and disseminate open-source solutions.

# External Resources

Here are a few links for other rescources:

https://wiki.rdu.im/_pages/Resource/software.html
https://wiki.rdu.im/_pages/Resource/online-course.html

Some other open-source, hard real-time operating systems that are popular:
http://rtxi.org/docs/manual/
https://www.rtai.org/

Papers that have done this sort of work:

"Building XenoBuntuLinux Distribution for Teaching and Prototyping Real-Time Operating Systems" (uses Xenomai 2)
https://arxiv.org/pdf/1103.2336.pdf

"Laboratorio de Practicas para la Ensenanzade Sistemas de Control de Tiempo Real" ("Laboratory of Practices for the Teaching of Real Time Control Systems")
https://core.ac.uk/download/pdf/41776734.pdf

"Towards an Open-Source Integrated Development and Real-Time Control Platform for Robots"
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.1078.5712&rep=rep1&type=pdf

https://reader.elsevier.com/reader/sd/pii/S1474667015375868?token=2D0A083310F68B5A5A99EC3D667E86609F2E57E4B40DBBAB7F29D7082BAC2D91668F7D9660DAE7A39B79F597F9D8956A

# Installing Xenomai3

To get started with installing Xenomai on Ubuntu, follow these instuctions:

## Check which architeture your computer uses

```
uname --all
```
My computer returned x86_64. 
So my computer's CPU is of x86 architecture (my computer runs on an x86 platform). I refer to my target architecture a. 
For me `<target-arch>` is replaced with `x86_64`.


## Downloading the Xenomai repository: 

Method A: Dowlnoad the latest stable version of Xenomai and unzip it 
```
wget https://gitlab.denx.de/Xenomai/xenomai/-/archive/stable/v3.0.x/xenomai-stable-v3.0.x.tar.gz
tar -xz 
```

Method B: 

Go to the xenomai website and manually download it
```
https://gitlab.denx.de/Xenomai/xenomai/-/tree/stable/v3.0.x
```

uunzip it from whereever it is dowloaded
```
tar -xz <xenomai-file>
```


## A note before proceding
Vesrion number: The when downloading the ipipe patch and the linux kernel source tree, the versions must exacly match. 
```
ipipe-core-<version>-<architecture>-<irrelevant_number>.patch
linux-<version>.tar.gz
```
In other words, the `<version>` part of the files must be the same.

Specific use case: make sure which kernels are supported with your target architecture. For example, I want to use librealsense `https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md`, so I obtained the latest patch (`ipipe-core-4.4.71-x86-10.patch`) that was compatible with my use case.

Configuration features: configuring the kernel will be done at a later step. 
Note that certain kernels will not let you freely enable or disable certain features. For example, I was not able to disable the `CPU_FREQ` kernel configuration option when configuring the linux-4.14.89 kernel, but I was ablt to with linux-4.4.182.So you might end up having to restart from this point snd get another kernel or ipipe patch depending on your specific kernel version needs.



## Get the ipipe patch

Method A: you can use the command to get a kernel patch (Note: the patch I provided below is specifically for linux kernel version 4.14.89 running on x86 architecture) 
```
wget https://xenomai.org/downloads/ipipe/v4.x/x86/ipipe-core-4.4.71-x86-10.patch 
wget https://xenomai.org/downloads/ipipe/v4.x/x86/ipipe-core-4.14.89-x86-2.patch

```


Method B: you can find and download a patch for a different linux kernel version and architecture through the follwing website

```
https://xenomai.org/downloads/ipipe/
click the version for your use case (e.g. v.4/x)
click on the target architecture for your use case (e.g. /x86)
Go to where the patch you want is dowloaded and click to download
```


```

```

## Get the linux kernel


Method A: On command line, change the directory to where you want to install the kernel 
Enter the commands to download the kernel and unzip it:
```
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v4.x/linux-4.4.71.tar.gz
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v4.x/linux-4.14.89.tar.gz 

tar -xf linux-4.4.71.tar.gz #extract contents of the tar file
tar -xf linux-4.14.89.tar.gz #extract contents of the tar file

rm  linux-4.4.71.tar.gz# remove the tar file
```


Method B: You can also dowload the specific linux source tree version that you want by visiting the website below
```
https://mirrors.edge.kernel.org/pub/linux/kernel/
Click the verison (e.g. v4.x)
Scoll down until you find the specific kernel version you want that matches with you patch
Click to download it
unzip it

```



## Building the Cobalt kernel

move your patch inside the kernel directory
```
mv ipipe-core-4.4.71-x86-10.patch linux-4.4.71/
```


The directory path for you linux kernel source tree is `<linux-srctree>`. 
For example, mine is `/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/linux-4.14.89`.

The directory path for your xenomai source tree is `<xenomai-root>`. 
For example, mine is `/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/xenomai/`.

The directory path for your xenomai ipipe-patch is `<ipipe-patch>`.
For example, mine is `/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/ipipe-core-4.4.182-x86-15.patch --arch=x86_64`.

Your target architecture is `<target-arch>`. 
For example, mine is `'x86_64'.

Build the Cobalt kernel as follows:
```
cd <linux_tree> 

<xenomai_root>/scripts/prepare-kernel.sh [--linux=<linux-srctree>]
[--ipipe=<ipipe-patch>] [--arch=<target-arch>]
```

an example of building the cobalt tree:
```
cd /home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/linux-4.4.71 

/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/xenomai/scripts/prepare-kernel.sh --linux=/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/linux-4.4.71 --ipipe=/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/ipipe-core-4.4.71-x86-10.patch --arch=x86_64
```

```
cd /home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/linux-4.4.71 

/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/xenomai-stable-v3.0.x/scripts/prepare-kernel.sh --linux=/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/linux-4.4.71 --ipipe=/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/linux-4.4.7/ipipe-core-4.4.71-x86-10.patch --arch=x86_64

```

```
/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/xenomai-stable-v3.0.x/scripts/prepare-kernel.sh --linux=/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/linux-4.4.71 --ipipe=/home/roman/Documents/courses/cs263/RTOS4ROBOTS/xenomai_code/linux-4.4.71/ipipe-core-4.4.71-x86-10.patch --arch=x86_64
```




Install `gconfig`, `xconfig`, and `menuconfig` dependencies respectively:
```
sudo apt install ncurses-dev kernel-package qt4-dev-tools pkg-config build-essential
sudo apt-get install libgtk2.0-dev libglib2.0-dev libglade2-dev
sudo apt-get install libncurses5-dev libncursesw5-dev

``` 


From the same linux source directory `<linux-srctree>`, configure the kernel using `menuconfig` (you can also instead use `xconfig` of `gconfig`):
```
make menuconfig
```
`gconfig` might be more intuituve. I switched back and forth to between `gconfig`'s (using the click-to-enable feature) and `menuconfig` (using the search feature).


You should now be in a `menuconfig` Kernel Configuration window. 
You can disable and enable deisred kernel configuration options. 
The enabled kernel options dictate what modules to install with your linux kernel to enable certain functionalities. 
For example, if you want to be able to use USB 3.0 or 2.0 with your kernel, you can make sure that the resprctive configuration options `usb_xhci_hcd` and `usb_ehci_hcd` are enabled. 
Information about all the linux kernel configuration options can be found here https://cateee.net/lkddb/web-lkddb/ . As you navigate the kernel configuration menu, you can press `/` to bring up search results for information on a specific configuration option you want to find. 
Note than you can pres the arrow buttons to movearound if the search results are cut off.

For example, you can type `/` then a search bar should popup. In the search bar, type in "USB" and it will return search results on configuration options that are related to "USB". The reults should be kernel configuration options with the following information:

`Symbol`: The variable name that the configuration option goes by. This also lists the current setting of the configuration option in brackets (e.g. [=y] means that it is enabled, [=n] means disabled)

`Type`: The possible values the configuration option can take can take. Boolean can either be [=y] or [=n]. Tristate can be [=y], [=n], or [=m] 

`Prompt`: gives information about the configuration option. This is also the "menu name" of the configuration option (how the option is displayed in the navigation menu)

`Location`: tells you where in the configuration menu you can go to in order to enable or disable the feature

`Defined at`: tells you where in the kernel filesytem you can find the feature

`Selects`: If this option is set to true (i.e. [=y]), then the opther options will be set to true

`Selected by`: If other options are set to true, then this feature is automatically set to true

`Depends on`: If other features are set to true, then this feature can be configured manually.
 
Note the current state of the feature (whther it is enabled or disabled). Got to the respective location and enable it if not already enabled.


Ensure the following features are set (with the key commands (`y`, `n`, or `m`) if using `menuconfig` or clicking appopriately if using `gconfig`:
```
#General Setup features
CONFIG_HIGH_RES_TIMERS[=y]  General Setup -> Timers subsystem -> High Resolution Timer Support (CONFIG_HIGH_RES_TIMERS[=y])

# Power Management and ACPI Options
APM - Disabled [=n] . Already disabled by default, don't need to mess with this one  -> Power management and ACPI options 
CPU_FREQ - Disable manually [=n] -> Power management and ACPI options -> CPU Frequency scaling -> CPU Frequency scaling 
INTEL_IDLE - Disable manually [=n] . This will also be automatically disabled when when CPU_IDLE is disabled (done at a later step) -> Power management and ACPI options -> CPU Idle -> CPU Idle Driver for Intel Processors
ACPI_PROCESSOR - Disabled [=n] -> Power management and ACPI options -> ACPI (Advanced Configuration and Power Interface) Support -> Processor
CPU_IDLE - Disable [=n] Power management and ACPI options -> CPU Idle -> CPU Idle PM Support 
i7300_IDLE [=n] Should be slready disabled. -> Power management and ACPI options -> Memory power savings -> Intel chipset idle memory power saving driver (Disable)

# Processsor type and features
Pocessor type and features -> Processor family -> Core 2/newer Xeon (if \"cat /proc/cpuinfo | grep family\" returns 6, set as Generic otherwise)

# Device Drivers
INPUT_PCSPKR - Disable manually [=n]  -> Device Drivers  -> Input device support  -> Generic input layer (needed for keyboard, mouse, ...) (INPUT [=y])  -> Miscellaneous devices (INPUT_MISC[=y]) -> PC Speaker Support 
CC_STACKPROTECTOR - can either be Enabled [=y] or Disabled [=n] for xenomai 3, I left it enabled as is. 

# Bus options
PCI_MSI - Can be enabled [=y] or Disabled [=n]. I left it enabled as is. (if you want to Enable [=y], then the operatios hooking/requesting, releasing, enabling/unmasking, disabling/masking must all be done from the Linux mode and not real-time (not from Cobalt) mode. The requirement translates into calling rtdm_irq_request(), rtdm_irq_free(), rtdm_irq_enable(), rtdm_irq_disable() exclusively from a non-rt handler in any RTDM driver. This includes the →open(), →close() and →ioctl_nrt() handlers.)  -> Bus options (PCI etc.) -> PCI support [=y] -> Message Signaled Interrupts (MSI and MSI-X) (PCI_MSI [= ])


#USB options
USB_SUPPORT [=y]   -> Device Drivers -> USB support (USB_SUPPORT[=y])
USB [=y] - This is host side USB  -> Device Drivers  -> USB support (USB_SUPPORT [=y]) -> Support for Host-side USB (USB [=y])
USB_xHCI_HCD [=y] - This is USB 3.0 host conroller support  -> Device Drivers  -> USB support (USB_SUPPORT [=y]) -> Support for Host-side USB (USB [=y]) -> xHCI HCD (USB 3.0) support (USB_xHCI_HCD [=y]) 
USB_XHCI_PLATFORM [=y]  -> Device Drivers  -> USB support (USB_SUPPORT [=y])  -> Support for Host-side USB (USB [=y]) -> xHCI HCD (USB 3.0) support (USB_XHCI_HCD[=y]) -> Generic xHCI driver for a platform device (USB_XHCI_PLATFORM [=y])
USB_EHCI_HCD [=y] - This is USB 2.0 host controller support -> Device Drivers -> USB support (USB_SUPPORT [=y]) -> Support for Host-side USB (USB [=y]) -> EHCI HCD (USB 2.0) support (USB_EHCI_HCD [=y]) ()
USB_EHCI_ROOT_HUB_TT [=y]  -> Device Drivers -> USB support (USB_SUPPORT [=y])  -> Support for Host-side USB (USB [=y])  -> EHCI HCD (USB 2.0) support (USB_EHCI_HCD[=y]) -> Root Hub Transaction Translators (USB_EHCI_ROOT_HUB_TT [=y])
USB_EHCI_TT_NEWSCHED [=y]   -> Device Drivers -> USB support (USB_SUPPORT [=y])  -> Support for Host-side USB (USB [=y])  -> EHCI HCD (USB 2.0) support (USB_EHCI_HCD[=y]) ->  Improved Transaction Translator scheduling (USB_EHCI_TT_NEWSCHED [=y])
# you might have to enable other USB options in order to compile architecture-specific USB drivers (depending on your use case) into your kernel

# real-time subsystem
XENOMAI [=y] Xenomai/cobalt

## considered the link https://rtt-lwr.readthedocs.io/en/latest/rtpc/xenomai.html , uses xenomai 2 , linux 2.18.20
## considered https://stackoverflow.com/questions/41949678/installation-steps-for-xenomai-3-on-ubuntu-16-04, uses xenomai 3, linux 4.4.43
## considered the link https://rtt-lwr.readthedocs.io/en/latest/rtpc/xenomai.html , uses xenomai 3 , linux 4.9.38
XENO_DRIVERS_NET [=m] -> Xenomai/cobalt (XENOMAI [=y])  -> Drivers -> RTnet -> RTnet, TCP/IP socket interface (XENO_DRIVERS_NET [=y]) 

## considered https://stackoverflow.com/questions/41949678/installation-steps-for-xenomai-3-on-ubuntu-16-04, uses xenomai 3, linux 4.4.43
## possibly the same as --> Nucleus (Enable)--> Pervasive real-time support in user-space (Enable) from  https://rtt-lwr.readthedocs.io/en/latest/rtpc/xenomai.html , uses xenomai 2 , linux 2.18.20 , not holding too much weight
XENO_DRIVERS_UDD [=y] -> Xenomai/cobalt (XENOMAI [=y]) -> Drivers -> UDD support -> User-space device driver framework XENO_DRIVERS_UDD [=y]

## considered the link https://rtt-lwr.readthedocs.io/en/latest/rtpc/xenomai.html , uses xenomai 3 , linux 4.9.38
Xenomai/cobalt --> Drivers --> RTnet --> RTnet, TCP/IP socket interface (Enable) --> Drivers --> New intel(R) PRO/1000 PCIe (Enable),  Realtek 8169 (Enable), Loopback (Enable, already came enabled when RTnet was enabled)
Xenomai/cobalt --> Drivers --> RTnet --> Add-Ons --> Real-Time Capturing Support (Enable)

#sizes and static limits
# i considered the link https://rtt-lwr.readthedocs.io/en/latest/rtpc/xenomai3.html , they use xenomai 3 and linux 4.9.38
Xenomai/Cobalt -> Sizes and Static Limits -> Number of Registry Slots (512 originally --> changed 4096 as susseged by xenomai-3.0.5 link)
Xenomai/Cobalt -> Sizes and Static Limits -> Size of system heap (4096 originally (left alone),  same as suggested xenomai-3.0.5 link)
Xenomai/Cobalt -> Sizes and Static Limits -> Size of private heap (256 originally  (left alone), same as suggested xenomai-3.0.5 link) 
Xenomai/Cobalt -> Sizes and Static Limits -> Size of shared heap (256 originally (left alone), same as as suggested xenomai-3.0.5 link)
Xenomai/Cobalt -> Sizes and Static Limits -> Maximum Number of POSIX timers per process -> (256 originally --> changed to 512 as suggested xenomai-3.0.5 link)

# i considered the link https://rtt-lwr.readthedocs.io/en/latest/rtpc/xenomai3.html , they use xenomai 3 and linux 4.9.38
MAX_SMP[=n] manually disable:  Pocessor type and features --> Enable maximum number of SMP processors and NUMA nodes (Disable)
Pocessor type and features -> Processor family ->Core 2/newer Xeon (if "cat /proc/cpuinfo | grep family" returns 6, set as Generic otherwise)
TRANSPARENT_HUGEPAGE [=n] manually disable: Pocessor type and features --> Transparent Hugepage Support (Disable)
COMPACTION [=n] manually disable:  Pocessor type and features --> Allow for memory compaction (Disable)
CMA [=n] manually disable : Pocessor type and features --> Contiguous Memory Allocation (Disable)
MIGRATION [=n] manually disable (CMA has to be disabled first) : Pocessor type and features --> Allow for memory compaction --> Page Migration (Disable)


###kernel wouldnt compile without disabling this
UNISYSSPAR [=n]  -> Device Drivers -> Stagaging drivers (STAGING

```
Build the Kernel (This might take a while. I started started compilation at 1:25. The compilation ended at 3:50. so about 2.5 hours):
```
make bzImage modules
```








