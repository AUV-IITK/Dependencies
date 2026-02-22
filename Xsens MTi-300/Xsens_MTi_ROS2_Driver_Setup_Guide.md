
# Xsens MTi ROS2 Driver Setup Guide

This guide documents the complete setup process for the Xsens MTi ROS2 Driver, ensuring that the system is correctly configured and outputs relevant IMU data including Roll, Pitch, and Yaw.

---

## ✅ Step-by-Step Setup Instructions

### 1. Check and Install Dependencies

Ensure these ROS2 packages are installed:

```bash
sudo apt update
sudo apt install ros-humble-nmea-msgs ros-humble-mavros-msgs
```

Replace `humble` with your ROS2 distro if different.

### 2. Create ROS2 Workspace

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
```

### 3. Clone the Repository

```bash
git clone -b ros2 --recurse-submodules https://github.com/xsenssupport/Xsens_MTi_ROS_Driver_and_Ntrip_Client.git
```

### 4. Build the Driver

Build the `xspublic` dependency:

```bash
cd Xsens_MTi_ROS_Driver_and_Ntrip_Client/src/xsens_mti_ros2_driver/lib/xspublic
make
```

Return to root of workspace and build:

```bash
cd ~/ros2_ws
colcon build --symlink-install --packages-up-to xsens_mti_ros2_driver
```

### 5. Source the Workspace

```bash
source install/setup.bash
```

To auto-source every new terminal session, add the line to your `~/.bashrc`:

```bash
echo 'source ~/ros2_ws/install/setup.bash' >> ~/.bashrc
source ~/.bashrc
```

### 6. Run the Driver

#### 🔧 Non-GUI (Headless)
To run the driver without a visualizer (best for running on the AUV):
```bash
ros2 launch xsens_mti_ros2_driver xsens_mti_node.launch.py
```

#### 🖥️ GUI with RViz

In a separate terminal:

```bash
ros2 launch xsens_mti_ros2_driver display.launch.py
```

Add display types:
- TF
- IMU (topic: `/imu/data`)
- Axes

### 7. Verify Roll, Pitch, and Yaw Output

To view orientation data, run below command in separate terminal window:

```bash
ros2 topic echo /imu/data
```

To convert quaternion to RPY:

```python
from tf_transformations import euler_from_quaternion
qx, qy, qz, qw = 0, 0, 0, 1  # replace with values from /imu/data
roll, pitch, yaw = euler_from_quaternion([qx, qy, qz, qw])
```

---


   ```

If you want Roll, Pitch, and Yaw in RPY form, convert the quaternion from `/imu/data` using this Python script:

   ```python
   from tf_transformations import euler_from_quaternion
   qx, qy, qz, qw = 0, 0, 0, 1  # replace with quaternion values from /imu/data
   roll, pitch, yaw = euler_from_quaternion([qx, qy, qz, qw])
   ```


---

## ✅ Summary

You now have:
- Verified and installed necessary dependencies
- Created a ROS2 workspace and cloned the driver repo
- Built and sourced the workspace
- Launched the driver with and without GUI
- Verified IMU data (Roll, Pitch, Yaw) via ROS topic echo
