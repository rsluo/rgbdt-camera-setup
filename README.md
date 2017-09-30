# ego-rgbdt-camera-setup

## Installing the drivers

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

## Running the setup

To collect data, start by turning on the laptop with the thermal camera connected to a USB port, but the depth camera disconnected. Check with `ls` (e.g. `ls /dev/video*`) to make sure the `video0/video1/video2` folders are all there. If not, run `sudo modprobe v4l2loopback video_nr=1,2`. 

Start the flir camera driver:
```
$ sudo ./flir1
```

Plug in the depth camera to a USB 3.0 port (the port *must* be USB 3.0). Navigate to the `librealsense/librealsense` folder. Compile the `examples/run_rgbdt.cpp` file. Start the depth camera and save the image frames:
```
./bin/run_rgbdt
```

Images are saved in the `librealsense/librealsense` folder.
