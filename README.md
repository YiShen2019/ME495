# ME495

## Part 1: Inspection
1.
```
source /opt/ros/melodic/setup.bash
mkdir -p ~/catkin_ws1/src
cd ~/catkin_ws1/
```
2.
wstool init ~/catkin_ws1/src
wstool set --git crazy_turtle https://github.com/m-elwin/crazy_turtle 
wstool up

3.
wstool set --git turtle_control https://github.com/YiShen2019/ME495
wstool up

4.
catkin_make

5.
source devel/setup.bash

6.
cd ~/catkin_ws1/src/crazy_turtle/launch
roslaunch go_crazy_turtle.launch

7.
rosnode list
    /mover
    /roaming_turtle
    /rosout

8.
rostopic list
    /rosout
    /rosout_agg
    /turtle1/cmd_vel
    /turtle1/color_sensor
    /turtle1/pose

9.
rostopic hz /turtle1/cmd_vel
    subscribed to [/turtle1/cmd_vel]
    average rate: 99.884
	    min: 0.009s max: 0.011s std dev: 0.00023s window: 95
    average rate: 99.952
	    min: 0.009s max: 0.011s std dev: 0.00024s window: 195
    average rate: 99.974
	    min: 0.009s max: 0.011s std dev: 0.00025s window: 295

THe frequency is about 100Hz.

10.
rosservice list
    /clear
    /kill
    /mover/get_loggers
    /mover/set_logger_level
    /reset
    /roaming_turtle/get_loggers
    /roaming_turtle/set_logger_level
    /rosout/get_loggers
    /rosout/set_logger_level
    /spawn
    /switch
    /turtle1/set_pen
    /turtle1/teleport_absolute
    /turtle1/teleport_relative

11.
cd ~/catkin_ws1/
catkin_make
source devel/setup.bash
rosservice type /switch | rossrv show
    turtlesim/Pose mixed_up
      float32 x
      float32 y
      float32 theta
      float32 linear_velocity
      float32 angular_velocity
    ---
    float64 x
    float64 y

rosservice type /switch
    crazy_turtle/Switch

rosservice node /switch
    /mover

Its type is 'crazy_turtle/Switch'.

The node is 'mover'.

12.
rosparam list
    /background_b
    /background_g
    /background_r
    /mover/velocity
    /rosdistro
    /roslaunch/uris/host_yi_thinkpad_x1_extreme__39007
    /rosversion
    /run_id

13.
rqt_graph

14.
rospack depends1 crazy_turtle 
    rospy
    message_runtime
    turtlesim

15.
rossrv package crazy_turtle
    crazy_turtle/Switch

16.
rosservice call /switch [1,1,0.30,2,5]
    x: 5.0
    y: 2.0
It returns the new position x and y, which new_x = x*angular velocity and new_y = y*linear velocity.

17.
rosparam get /mover/velocity
    5

18.
rosparam set /mover/velocity 10

rosparam get /mover/velocity
    10
The turtle doesn't change it's velocity.

19.
rosnode kill /mover
    killing /mover
    killed

20.
rosrun crazy_turtle mover cmd_vel:=/turtle1/cmd_vel

21.
Its velocity changed to 10.
The launch file initialized its velocity.
