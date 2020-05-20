# RTOS4ROBOTS


## Download Xenomai

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


##check which architeture your computer uses

```
uname --all
```

My computer returned x86_64. 
So my computer's CPU is of x86 architecture (my computer runs on an x86 platform)


## A note before proceding
The when downloading the ipipe patch and the linux kernel source tree, the versions must exacly match. 

```
ipipe-core-<version>-<architecture>-<irrelevant_number>.patch
linux-<version>.tar.gz
```
In other words, the <version> part of the files must be the same



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





