# RTOS4ROBOTS


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
The when downloading the ipipe patch and the linux kernel source tree, the versions must exacly match. 

```
ipipe-core-<version>-<architecture>-<irrelevant_number>.patch
linux-<version>.tar.gz
```
In other words, the `<version>` part of the files must be the same



## Get the ipipe patch

Method A: you can use the command to get the patch (Note: this patch is specifically for linux kernel version 4.14.89 running on x86 architecture) 
```
wget https://xenomai.org/downloads/ipipe/v4.x/x86/ipipe-core-4.14.89-x86-2.patch 
```


Method B: you can find and download a patch for a different linux kernel version and architecture through the follwing website

```
https://xenomai.org/downloads/ipipe/
Go to where the patch is dowloaded and unzip it
```


```

```

## Get the linux kernel


Method A: On command line, change the directory to where you want to install the kernel 
Enter the commands to download the kernel and unzip it:
```
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v4.x/linux-4.14.89.tar.gz 

tar -xf linux-4.14.89.tar.gz
```


Method B: You can also dowload the specific linux source tree version that you want by visiting the website below
```
https://mirrors.edge.kernel.org/pub/linux/kernel/
Click the verison (e.g. v4.x)
Scoll down until you find the specific kernel version you want
Click to download it
unzip it

```

## Building the Cobalt kernel

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

You should now be in a Kernel Configuration GUI window. You can disable and enable deisred kernel configuration otions. As you navigate the menu, you can press `/` to bring up search results for information on a specific configuration option you want to find. 
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

Esure the following features are set:
```
APM - Disabled [=n]
ACPI_PROCESSOR - Disabled [=n]
USB - Enabled [=y]
CPU_FREQ - Disabled [=n]
CPU_IDLE - Disable [=n]
CC_STACKPROTECTOR - can either be Enabled [=y] or Disabled [=n]
ACPI_PROCESSOR - Disable
INTEL_IDLE - Disable
INPUT_PCSPKR - Disable
PCI_MSI - Disabled [=n], (if you want to Enable [=y], then the operatios hooking/requesting, releasing, enabling/unmasking, disabling/masking must all be done from the Linux mode and not real-time (not from Cobalt) mode. The requirement translates into calling rtdm_irq_request(), rtdm_irq_free(), rtdm_irq_enable(), rtdm_irq_disable() exclusively from a non-rt handler in any RTDM driver. This includes the →open(), →close() and →ioctl_nrt() handlers.)

```









