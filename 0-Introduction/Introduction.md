# What is a Device Driver?
Device driver is a piece of code that configures and manages a hardware device.
It exposes interfaces to the user-space so that the user application can communicate with the device.

Device driver lies in the Kernel space.
When we load the device driver, it connects to the kernel to talk to the user space.

We access the device by using a file access technique (read/write/open/echo/…).

## Device File
The device driver has to create a device file interface in the user-space, so that user-level programs can communicate with hardware using traditional file access system calls operations (read/write/…).


<img align="center" src="https://github.com/Amir-Mansoori/Embedded-Linux-Device-Drivers/blob/main/Images/Basic3.png" width="700" height="500">

The above picture shows how the user-space interacts with the hardware through the device file which is managed by the device driver.

## Connection of a system call from user-space to driver
When user-space program uses the open system call, it must connect to the open system call implementation of the device driver (same for read/write/…)

**How to establish such a connection between user-space system calls and device drivers?**

**VFS (Virtual File System)** of the kernel. Device driver must be registered with the VFS by using VFS kernel APIs.

### Device number (Major and Minor):

We might have several instances of the device driver in our device files.

Device file: 

/dev/rtc0	4:0

/dev/rtc1	4:1

/dev/rtc2	4:2

/dev/rtc3	4:3

Device Driver: rtc	4

4 is the Major number.

0 to 3 are the minor numbers (refers to the device instance)

VFS gets the device number and compares it with its driver registration list to find and match the connection.
Character Device Add (CDEV_Add): registration of device driver number in VFS.

Connection establishment between device file access and the driver:
-	Create device number
-	Create device files
-	Make a device driver registration with the VFS (CDEV_ADD)
-	Implement the driver’s file operation methods for open, read/write/…

For the character devices:

1.	Create device number:
	-	alloc_chrdev_region();
2.	Make a char device registration with the VFS:
	-	cdev_init();
	-	cdev_add();
3.	Create device files:
	-	class_create();
	-	device_create();
4.	Implement the driver’s file operation methods for open/read/write/…

In **MODULE_INIT** initialization of the module in the device driver, we use the creation file APIs

In **MODULE_EXIT**, we delete the device files (unregister_chrdev_region(), cdev_del, class_destroy, device_destroy).


Add appropriate header files:

	Include/linux/fs.h, cdev.h, device.h, uaccess.h


Example:
```
Dev_t device_number;
alloc_chrdev_region(&device_number, 0, 7, “eeprom”);
alloc_chrdev_region(dynamically allocated major number , first minor, range (total minor numbers), name);
127:0, 127:1, 127:2, 127:3, 127:4, 127:5, 127:6
```
Useful APIs to interact with device numbers:
```
	Int minor_no = MINOR(device_number);
	Int major_no = MAJOR(device_number);
	MKDEV(int major, int minor);
```
