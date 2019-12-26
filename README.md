# ME495

*Yi Shen, Northwestern Univerisity*

## Part 1: Inspection

1.
```
$ source /opt/ros/melodic/setup.bash
$ mkdir -p ~/catkin_ws1/src
$ cd ~/catkin_ws1/src
```
2.
```
$ wstool init ~/catkin_ws1/src
$ wstool set --git crazy_turtle https://github.com/m-elwin/crazy_turtle 
$ wstool up
```

3.
```
$ wstool set --git turtle_control https://github.com/YiShen2019/ME495
$ wstool up
```

4.
```
$ catkin_make
```

5.
```
$ source devel/setup.bash
```

6.
```
$ cd ~/catkin_ws1/src/crazy_turtle/launch
$ roslaunch go_crazy_turtle.launch
```

7.
```
$ rosnode list
```
    /mover
    /roaming_turtle
    /rosout

8.
```
$ rostopic list
```
    /rosout
    /rosout_agg
    /turtle1/cmd_vel
    /turtle1/color_sensor
    /turtle1/pose

9.
```
$ rostopic hz /turtle1/cmd_vel
```
    subscribed to [/turtle1/cmd_vel]
    average rate: 99.884
	    min: 0.009s max: 0.011s std dev: 0.00023s window: 95
    average rate: 99.952
	    min: 0.009s max: 0.011s std dev: 0.00024s window: 195
    average rate: 99.974
	    min: 0.009s max: 0.011s std dev: 0.00025s window: 295

THe frequency is about 100Hz.

10.
```
$ rosservice list
```
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
```
$ cd ~/catkin_ws1/
$ catkin_make
$ source devel/setup.bash
$ rosservice type /switch | rossrv show
```
    turtlesim/Pose mixed_up
      float32 x
      float32 y
      float32 theta
      float32 linear_velocity
      float32 angular_velocity
    ---
    float64 x
    float64 y

```
$ rosservice type /switch
```
    crazy_turtle/Switch

```
$ rosservice node /switch
```
    /mover

Its type is 'crazy_turtle/Switch'.

The node is 'mover'.

12.
```
$ rosparam list
```
    /background_b
    /background_g
    /background_r
    /mover/velocity
    /rosdistro
    /roslaunch/uris/host_yi_thinkpad_x1_extreme__39007
    /rosversion
    /run_id

13.
```
$ rqt_graph
```
![rosgraph](https://user-images.githubusercontent.com/55845795/71335170-04939100-2507-11ea-839f-6110a46ead34.png)

14.
```
$ rospack depends1 crazy_turtle 
```
    rospy
    message_runtime
    turtlesim

15.
```
$ rossrv package crazy_turtle
```
    crazy_turtle/Switch

16.
```
$ rosservice call /switch [1,1,0.30,2,5]
```
    x: 5.0
    y: 2.0
    
It returns the new position x and y, which new_x = x*angular velocity and new_y = y*linear velocity.

17.
```
$ rosparam get /mover/velocity
```
    5

18.
```
$ rosparam set /mover/velocity 10
```
```
$ rosparam get /mover/velocity
```
    10
The turtle doesn't change it's velocity.

19.
```
$ rosnode kill /mover
```
    killing /mover
    killed

20.
```
$ rosrun crazy_turtle mover cmd_vel:=/turtle1/cmd_vel
```

21.
Its velocity changed to 10.

The launch file initialized its velocity.

## Part 2: Turtle Control

1.
```
$ cd ~/catkin_ws1/src
$ catkin_create_pkg turtle_control 
```

3.
```
$ cd turtle_control turtlesim rospy std_msgs 
$ git config --global user.email "yishen2019@u.northwestern.edu"
$ git config --global user.name "YiShen2019"
$ git add package.xml
$ git commit -m "edit default"
```

4.
```
$ cd ..
$ rosinstall_generator desktop_full --rosdistro hydro --deps > homework1.rosinstall
```

### Velocity Translator

1.
```
$ cd ~/catkin_ws1/src/turtle_control
$ mkdir msg
$ cd msg
$ gedit TurtleVel.msg
```

2.
```
$ cd ~/catkin_ws1
$ catkin_make
```

3.
```
$ cd ~/catkin_ws1/src/turtle_control
$ mkdir scripts
$ cd scripts
$ gedit turtle_interpret
$ chmod 755 scripts/turtle_interpret
```

4.
```
$ rosrun rqt_console rqt_console
$ rosrun rqt_logger_level rqt_logger_level
```

5.
```
$ rosrun turtle_control turtle_interpret
$ rostopic pub -1 turtle_vel turtle_control/TurtleVel "{linearvel: 1.0, angularvel: 1.0}"
```
[INFO] [1577316162.659775]: Message from turtle_vel: linear 1.0 angular 1.0

6.
```
$ gedit VelTranslate.srv
$ cd ~/catkin_ws1
$ catkin_make
```

7.
```
rosservice call /vel_translate "input_Twist:
  linear:
    x: 1.0
    y: 0.0
    z: 0.0
  angular:
    x: 0.0
    y: 0.0
    z: 1.0"
```
output_TurtleVel: 
  linearvel: 1.0
  angularvel: 1.0
  debug_message: ''

```
rosservice call /vel_translate "input_Twist:
  linear:
    x: 1.0
    y: 0.0
    z: 0.0
  angular:
    x: 0.0
    y: 2.0
    z: 1.0"
```
ERROR: service [/vel_translate] responded with an error: service cannot process request: service handler returned None

### Turtle waypoint

1.
```
$ cd ~/catkin_ws1/src/turtle_control/scripts
$ gedit waypoint
```

2.
```
$ cd ~/catkin_ws1/src/turtle_control
$ mkdir launch
$ cd launch
$ gedit waypoint_follow.launch
```

3.
```
$ 




