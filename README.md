# ME495
1.
source /opt/ros/melodic/setup.bash

mkdir -p ~/catkin_ws1/src

cd ~/catkin_ws1/

catkin_make

source devel/setup.bash

source ~/catkin_ws1/devel/setup.bash

2.
wstool init ~/catkin_ws1/src

wstool set --git crazy_turtle https://github.com/m-elwin/crazy_turtle 

wstool up

3.
