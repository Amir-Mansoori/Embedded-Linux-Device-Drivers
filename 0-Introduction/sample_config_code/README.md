# How to Cross-compile an embedded device driver in Buildroot
After writing the main.c and Makefile, you can cross-compile your device driver using this command:

```
make ARCH=arm CROSS_COMPILE=arm-buildroot-linux-gnueabihf- -C <path-to-buildroot>/buildroot/output/build/linux-custom/ M=$PWD modules
```

If you want to compile the code in your host instead, you can use the linux kernel path of your host (``/lib/modules/<version>/build/``).

When you use the -C option with the make command, it changes the directory to the specified kernel source directory and invokes the top-level Makefile there. 
This top-level Makefile in the kernel source tree will then call the Makefile in your module's local directory specified by ``M=$(PWD)``.
