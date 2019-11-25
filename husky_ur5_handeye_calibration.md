# Husky Dual UR5 Hand Eye Calibration

We use [easy_handeye](https://github.com/IFL-CAMP/easy_handeye) package to do the hand-eye calibration. Another reference package  is [easy_handeye_demo](https://github.com/marcoesposito1988/easy_handeye_demo).

There is a example of ur5 and Kinect in [ur5_kinect_calibration.launch](https://github.com/wangcongrobot/easy_handeye/blob/master/docs/example_launch/husky_ur5_stereo_calibration.launch), and we will change it to adapt our robot. We need to change some items as follow.


1. copy the `ur5_kinect_calibration.launch` to launch folder, name it `husky_ur5_stereo_calibration.launch`

2. change the `namespace_prefix` to `husky_ur5_stereo_handeyecalibration`

3. add the values of `marker_size` and `marker_id`

```
<arg name="marker_size" value="0.095" />
<arg name="marker_id" value=100 />
```

4. comment `start the Kinect` because we use our camera

5. comment `start the robot` because we launch the husky dual ur5 moveit instead
   
6. change `aruco_ros` part

We use aruco_ros package to caculate the position from camera to aruco marker. It will publish topics as below:
```
<remap from="/camera_info" to="/stereo_camera/left/camera_info" />
<remap from="image" to="/stereo_camera/left/image_rect_color" />
<param name="reference_frame" value="camera_link" />
<param name="camera_frame" value="stereo_camera" />
<param name="marker_frame" value="camera_marker" />
```

6. change the frame in `calibration` part

```
<arg name="tracking_base_frame" value="camera_link" />
<arg name="tracking_marker_frame" value="camera_marker" />
<arg name="robot_base_frame" value="r_ur5_arm_base_link" />
<arg name="robot_effector_frame" value="r_ur5_arm_ee_link" />
```

7. change `move_group` value in `calibration`
```
<arg name="move_group" value="right_arm" />
```

8. manually choose sample

The automatic choosing sample using MoveIt! is hard, and we get a lot of motion failed error. So We use manually choose sample: change the position using UR5 GUI Interface to sample depend on the image you can see in the video. 

9. result

The result will be saved in ~/.ros/husky_ur5_stereo_handeyecalibration/

10. evaluation

You can calculate the result when you get some samples. With the increase of samples, if the result is stable, I think it's a good results.
