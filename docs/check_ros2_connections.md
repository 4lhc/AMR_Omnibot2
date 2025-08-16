
To ensure that ros nodes running in my rpi4 are discoverable by nodes in my ubuntu 24.04 master, I did the following.


```
bash
ssh <user>@rpi4.local
#In rpi bash 
source /opt/ros/kilted/setup.bash
ros2 run demo_nodes_cpp listener
```


```
bash
# in master
ros2 run demo_nodes_cpp talker
```

