

## Steps to install joystick in rpi4

Ubuntu 24.04 Server edition is installed in the rpi4.

### Install updated kernal modules for the controller
Install dynmaic kernal module support and appropriate linux headers

```bash
sudo apt install dkms linux-headers-`uname -r` 
```

Install xpadneo - Advanced Linux Driver for Xbox One Wireless Gamepad

```bash
git clone https://github.com/atar-axis/xpadneo.git
cd xpadneo && sudo bash ./install.sh
```

### Connect with the controller via bluetooth

Install bluez if already not installed

```bash
sudo apt install bluez
```

Enable and start bluetooth systemd service.

```bash
sudo systemctl enable bluetooth.service
sudo systemctl start bluetooth.service
```

Connect to the controller

```bash
sudo bluetoothctl
[bluetooth]# power on
[bluetooth]# scan on
# Wait for the device to appear
[bluetooth]# pair <MAC_Address>

# To enable automatic connection after successful pairing
[bluetooth]# trust <MAC_Address>
[bluetooth]# connect <MAC_Address>
```

## Running teleop_twist_joy

Edit the teleop joy configs for xbox controller with `vim $(ros2 pkg prefix teleop_twist_joy)/share/teleop_twist_joy/config/xbox.config.yaml`

By installing `jstest-gtk` and `joystick` apt packages you will be able to identify the stick axes and button numbers. Edit the `xbox.config.yaml` file to configure.

```yaml
teleop_twist_joy_node:
  ros__parameters:
    axis_linear:  # Left thumb stick vertical
      x: 1        # Axis 1 is Left Thumbg Stick Vertical
    scale_linear:
      x: 0.7
    scale_linear_turbo:
      x: 1.5

    axis_angular:  # Right thumb stick horizontal
      yaw: 2       # Axis '2' is right thumbstick horizontal  
    scale_angular:
      yaw: 0.4

    enable_button: 8  # Left trigger button
    enable_turbo_button: 9  # Right trigger button

```


Launh teleop node by running the below command. In kilted, I had to make sure `geometry_msgs/msg/TwistStamped` message is used with the arg `stamped_twist:='true'`

```bash
 ros2 launch teleop_twist_joy teleop-launch.py joy_config:='xbox' publish_stamped_twist:='true'

```





