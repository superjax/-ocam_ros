# ocam_ros

A ROS wrapper for the OCAM Global Shutter Camera

This is basically a ROS wrapper around the withrobot [opencv examples](http://withrobot.com/en/camera/ocam-1cgn-u/?ckattempt=1). It has been tested primarily with the 1CGN-U color global shutter camera, but will likely work with the mono version 1MGN-U.

## Important
Different versions of the oCam firmware have different API, so they are not all compatible with this ROS node.  I can confirm that the firwmare [1CGN-U_R1803_180404.img](https://github.com/withrobot/oCam/blob/master/Firmware/oCam-1CGN-U_R1803_180404.img) works on my device. Please update (or roll back) your firmware to this version.

## Functionality
- Display Image and publish to ROS network, configure settings
- Use the `~show_image` parameter to adjust brightness and exposure on the fly with keyboard shortcuts (similar to oCam linux examples).

## Supported Image Formats
* USB 3.0
  * 8-bit Greyscale 1280 x 960 @ 45 fps
  * 8-bit Greyscale 1280 x 720 @ 60 fps
  * 8-bit Greyscale 640 x 480 @ 100 fps
  * 8-bit Greyscale 320 x 240 @ 160 fps
* USB 2.0
  * 8-bit Greyscale 1280 x 960 @ 22.5 fps
  * 8-bit Greyscale 1280 x 720 @ 30 fps
  * 8-bit Greyscale 640 x 480 @ 100 fps
  * 8-bit Greyscale 320 x 240 @ 160 fps

## Installation
Install dependencies:
``` bash
sudo apt install libudev-dev libv4l-dev
```
Compile the ROS package:

``` bash
mkdir -p catkin_ws/src
cd catkin_ws/src
catkin_init_workspace
git clone https://github.com/superjax/ocam_ros
cd ..
catkin_make
```

## Running the Node

```bash
rosrun ocam_ros ocam_ros_node
```

For changing parameter values and topic remapping from the command line using `rosrun` refer to the [Remapping Arguments](http://wiki.ros.org/Remapping%20Arguments) page.

For setting parameters and topic remappings from a launch file, refer to the [Roslaunch for Larger Projects](http://wiki.ros.org/roslaunch/Tutorials/Roslaunch%20tips%20for%20larger%20projects) page.

## Parameters
* `~device_path` (string, default: "/dev/video0")
    - Serial port to connect to
* `~frame_id` (string, default: "camera")
   - Frame id of camera measurements
* `~camera_info_url` (string, default: "")
   - Path to camera intrinsic parameters file
* `~width` (int, default: 640)
   - Image width in pixels
* `~height` (int, default: 480)
   - Image height in pixels
* `~fps` (int, default: 80)
   - Camera frame rate
* `~color` (bool, default: false)
   - Publish a color image (only if using 1CGN-U)
* `~image_topic` (string, default: "image")
   - Image topic name
* `~show_image` (bool, default: false)
   - Shows image and enables keyboard shortcuts to adjust image brightness and exposure
* `~rescale_camera_info` (bool, default: false)
   - Whether or not rescale the camera intrinsics
* `~brightness` (int, default: 64, min/max: 1/127)
   - Adjusts image brightness
* `~exposure` (int, default: 39, min/max: 1/625)
   - Adjusts camera shutter speed
* `~auto_exposure` (bool, default: true)
   - Whether or not to automatically adjust exposure

## Topics
- `camera/image`(sensor_msgs/Image)
    - Image measurements from oCam

## ToDo
- Finish documentation
- Adjusting rate of streaming and configuration on the fly with rqt_reconfigure
