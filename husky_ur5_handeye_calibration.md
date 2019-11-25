# Husky Dual UR5 Hand Eye Calibration

We use [easy_handeye](https://github.com/IFL-CAMP/easy_handeye) package to do the hand-eye calibration. Another reference package  is [easy_handeye_demo](https://github.com/marcoesposito1988/easy_handeye_demo).



need to change:

    <!-- The input reference frames -->
    <arg name="robot_base_frame" default="l_ur5_arm_base_link" />
    <arg name="robot_effector_frame" default="l_ur5_arm_ee_link" />
    <arg name="tracking_base_frame" default="tracking_origin" />
    <arg name="tracking_marker_frame" default="tracking_marker" />

change ur5_kinect_calibration.launch

## How to use easy_handeye package to calibrate the husky ur5 

We will change the example launch `ur5_kinect_calibration.launch` to adapt our robot.

- <arg name="namespace_prefix" default="husky_ur5_stereo_handeyecalibration" />

- add value of marker_size and marker_id

```
<arg name="marker_size" value="0.095" />
<arg name="marker_id" value=100 />
```
- comment `start the Kinect` because we use our camera 

- comment `start the robot` because we launch the husky dual ur5 moveit seperated
- change the frame

```
<arg name="tracking_base_frame" value="camera_link" />
<arg name="tracking_marker_frame" value="camera_marker" />
<arg name="robot_base_frame" value="r_ur5_arm_base_link" />
<arg name="robot_effector_frame" value="r_ur5_arm_ee_link" />
```

- aruco_ros

We use aruco_ros package to caculate the position from camera to aruco marker. It will publish topics as below:
```
<remap from="/camera_info" to="/stereo_camera/left/camera_info" />
<remap from="image" to="/stereo_camera/left/image_rect_color" />
<param name="reference_frame" value="camera_link" />
<param name="camera_frame" value="stereo_camera" />
<param name="marker_frame" value="camera_marker" />
```

- calibrate.launch

change the `move_group` from the default value `manipulator` to `right_arm`
```
<arg name="move_group" default="right_arm" />
```
Add <arg name="move_group" value="right_arm" />

- choose sample

We use manually choose sample: change the position using UR5 GUI Interface to sample.


- The result will be saved in ~/.ros/husky_ur5_stereo_handeyecalibration/
