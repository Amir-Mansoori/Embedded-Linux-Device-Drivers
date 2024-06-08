# What is a Device Driver?
Device driver is a piece of code that configures and manages a hardware device.
It exposes interfaces to the user-space so that the user application can communicate with the device.

Device driver lies in the Kernel space.
When we load the device driver, it connects to the kernel to talk to the user space.

We access the device by using a file access technique (read/write/open/echo/…).

The device driver has to create a device file interface in the user-space, so that user-level programs can communicate with hardware using traditional file access system calls operations (read/write/…).
