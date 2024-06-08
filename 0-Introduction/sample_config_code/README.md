# How to Cross-compile an embedded device driver in Buildroot
The following commands are used when you want to manually write the compilation commands on a script. Considering that you have a main.c and Makefile, you can cross-compile your device driver by the make command. First, let's assume that the Makefile only contains the configuration of the main.o object file to be built as a loadable kernel module (``obj-m := main.o``). You can use this make command to cross-compile your driver:

```
make ARCH=arm CROSS_COMPILE=arm-buildroot-linux-gnueabihf- -C <path-to-buildroot>/buildroot/output/build/linux-custom/ M=$PWD modules
```

If you want to compile the code in your host instead, you can use the linux kernel path of your host (``/lib/modules/<version>/build/``).

When you use the -C option with the make command, it changes the directory to the specified kernel source directory and invokes the top-level Makefile there. 
This top-level Makefile in the kernel source tree will then call the Makefile in your module's local directory specified by ``M=$(PWD)``.

To automate the process, we can use the Makefile provided in this repository. It can be used for the compilation for our host or the target hardware.
