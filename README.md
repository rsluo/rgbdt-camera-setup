# ego-rgbdt-camera-setup

After installing a version of Ubuntu, you will need to install two sets of drivers. 

### 1. Intel Realsense SR300 (the depth camera)

To install this driver, you can use the Intel Realsense Cross-Platform API, following the instructions [here](https://github.com/rsluo/librealsense). This is the original [Intel API](https://github.com/IntelRealSense/librealsense) with one additional file for running our setup. 

### 2. Flir One (the thermal camera)

First, you need the header file libusb.h:
```
$ sudo apt-get install libusb-1.0-0-dev
```

Then, check that there is valid v412 driver:
```
$ sudo modprobe v4l2loopback
```

If you see an error like this: `modprobe: FATAL: Module v4l2loopback not found.`, then you will need to compile the driver for your kernel. You can load only the kernel headers (as part of the packet linux-generic):
```
$ sudo apt-get install linux-headers-generic
```

Then, you can compile v412:
```
$ git clone https://github.com/umlaeute/v4l2loopback
$ cd v4l2loopback/
$ make
$ sudo make install
```

Now load video1 and video2 (because your notebook camera is probably video0):
```
$ sudo modprobe v4l2loopback video_nr=1,2
```

Finally, run this command:
```
$ /usr/bin/gcc '-I/usr/include/libusb-1.0'  -o flir1 flir8g-mint.c -lusb-1.0 -lm
```
