# Navida Yazılım Doc

### Ros2 Kurulumu
    https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html

### Gazebo Garden Kurulumu
    https://gazebosim.org/docs/garden/install_ubuntu/

### Mavros Kurulumu
https://github.com/mavlink/mavros/blob/ros2/mavros/README.md

```bash
sudo apt install ros-humble-mavros
ros2 run mavros install_geographiclib_datasets.sh
```

### Ardupilot Kurulumu
https://ardupilot.org/dev/docs/building-setup-linux.html#building-setup-linux
https://ardupilot.org/dev/docs/setting-up-sitl-on-linux.html

```bash
git clone --recurse-submodules https://github.com/ArduPilot/ardupilot.git
cd ardupilot
Tools/environment_install/install-prereqs-ubuntu.sh -y
. ~/.profile
```

Now from the ardupilot directory we just created start the simulation once to finish its setup, you can close it afterwards.

```bash
cd ArduCopter
sim_vehicle.py --console --map -w
```

### Colcon Workspace'i Ayağa Kaldırma
1. Workspace'i klonla.

    ```bash
    git clone https://github.com/Navi-IDA/ros2-ws.git 
    ```


    veya ssh keyin varsa 

    ```bash    
    git clone git@github.com:Navi-IDA/ros2-ws.git
    ```

2. ZED SDK ve ROS Wrapper kurulumu.

    Bu adımı sadece gerçek araçta kurulum yaparken yapmamız gerekiyor, simulasyonda için gerçek zed kullanılmadığı için bu adım atlanabilir. Eğer simulasyon için kurulum yapıyorsanız **src/zed_ros2_examples ve src/zed-ros2-wrapper** dosyalarını localinizden silin. **Ancak Gite pushlarken bu sildiğiniz dosyaları commitlemeyin, sadece diğer değiştirdiğiniz dosyaları stage edip commitleyin.**

    https://www.stereolabs.com/docs/ros2

3. Colconu buildle ve sourcela.

    ```bash
    sudo apt install python3-sdformat13 ros-humble-ros-gzgarden ros-humble-xacro
    cd ros2-ws
    git checkout -b develop
    pip3 install -r src/yolo_ros/requirements.txt
    rosdep install --from-paths src --ignore-src -r -y
    colcon build --merge-install --cmake-args=-DCMAKE_BUILD_TYPE=Release
    source install/setup.bash
    ```


    --cmake-args kısmını sadece cmake buildlerken verseniz yeterli. Hızlı build almak için bir kere yaptıktan sonra kullanmayabilirsiniz.


### Ardupilot_gazebo Plugini
https://github.com/ArduPilot/ardupilot_gazebo

```bash
sudo apt update
sudo apt install libgz-sim7-dev rapidjson-dev
sudo apt install libopencv-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl
```

```bash
export GZ_VERSION=garden
sudo bash -c 'wget https://raw.githubusercontent.com/osrf/osrf-rosdep/master/gz/00-gazebo.list -O /etc/ros/rosdep/sources.list.d/00-gazebo.list'
rosdep update
rosdep resolve gz-garden
# Navigate to your ROS workspace before the next command.
rosdep install --from-paths src --ignore-src -y
```

```bash
cd ~/
git clone https://github.com/ArduPilot/ardupilot_gazebo
cd ardupilot_gazebo
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo
make -j4
```

```bash
export GZ_SIM_SYSTEM_PLUGIN_PATH=$HOME/ardupilot_gazebo/build:$GZ_SIM_SYSTEM_PLUGIN_PATH
export GZ_SIM_RESOURCE_PATH=$HOME/ardupilot_gazebo/models:$HOME/ardupilot_gazebo/worlds:$GZ_SIM_RESOURCE_PATH
echo 'export GZ_SIM_SYSTEM_PLUGIN_PATH=$HOME/ardupilot_gazebo/build:${GZ_SIM_SYSTEM_PLUGIN_PATH}' >> ~/.bashrc
echo 'export GZ_SIM_RESOURCE_PATH=$HOME/ardupilot_gazebo/models:$HOME/ardupilot_gazebo/worlds:${GZ_SIM_RESOURCE_PATH}' >> ~/.bashrc
source ~/.bashrc 
```

### Mavros Yaml
/opt/ros/humble/share/mavros/apm_config.yaml dosyasını bunla değiştir.

```bash
# Common configuration for APM2 autopilot
#
/**:
  ros__parameters:
    startup_px4_usb_quirk: false

# --- system plugins ---

# sys_status
/**/sys:
  ros__parameters:
    min_voltage: [10.0]   # diagnostics min voltage, use a vector i.e. [16.2, 16.0] for multiple batteries, up-to 10 are supported
                          # to achieve the same on a ROS launch file do: <rosparam param="sys/min_voltage">[16.2, 16.0]</rosparam>
    disable_diag: false   # disable all sys_status diagnostics, except heartbeat
    heartbeat_rate: 1.0   # send heartbeat rate in Hertz
    heartbeat_mav_type: "ONBOARD_CONTROLLER"
    conn_timeout: 10.0    # heartbeat timeout in seconds

# sys_time
/**/time:
  ros__parameters:
    time_ref_source: "fcu"    # time_reference source
    timesync_mode: MAVLINK
    timesync_avg_alpha: 0.6   # timesync averaging factor
    timesync_rate: 10.0       # TIMESYNC rate in Hertz (feature disabled if 0.0)
    system_time_rate: 1.0     # send system time to FCU rate in Hertz (disabled if 0.0)

# --- mavros plugins (alphabetical order) ---

# 3dr_radio
/**/tdr_radio:
  ros__parameters:
    low_rssi: 40  # raw rssi lower level for diagnostics

# actuator_control
# None

# command
/**/cmd:
  ros__parameters:
    use_comp_id_system_control: false   # quirk for some old FCUs

# dummy
# None

# ftp
# None

# global_position
/**/global_position:
  ros__parameters:
    frame_id: "map"               # origin frame
    child_frame_id: "base_link"   # body-fixed frame
    rot_covariance: 99999.0       # covariance for attitude?
    gps_uere: 1.0                 # User Equivalent Range Error (UERE) of GPS sensor (m)
    use_relative_alt: true        # use relative altitude for local coordinates
    tf.send: false                # send TF?
    tf.frame_id: "map"            # TF frame_id
    tf.global_frame_id: "earth"   # TF earth frame_id
    tf.child_frame_id: "base_link"  # TF child_frame_id

# imu_pub
/**/imu:
  ros__parameters:
    frame_id: "wamv/wamv/base_link"
    # need find actual values
    linear_acceleration_stdev: 0.0003
    angular_velocity_stdev: 0.0003490659  # 0.02 degrees
    orientation_stdev: 1.0
    magnetic_stdev: 0.0

# local_position
/**/local_position:
  ros__parameters:
    frame_id: "map"
    tf.send: true
    tf.frame_id: "map"
    tf.child_frame_id: "wamv/wamv/base_link"
    tf.send_fcu: false

# param
# None, used for FCU params

# rc_io
# None

# setpoint_accel
/**/setpoint_accel:
  ros__parameters:
    send_force: false

# setpoint_attitude
/**/setpoint_attitude:
  ros__parameters:
    reverse_thrust: false       # allow reversed thrust
    use_quaternion: false       # enable PoseStamped topic subscriber
    tf.listen: false            # enable tf listener (disable topic subscribers)
    tf.frame_id: "map"
    tf.child_frame_id: "target_attitude"
    tf.rate_limit: 50.0

# setpoint_raw
/**/setpoint_raw:
  ros__parameters:
    thrust_scaling: 1.0         # specify thrust scaling (normalized, 0 to 1) for thrust (like PX4)

# setpoint_position
/**/setpoint_position:
  ros__parameters:
    tf.listen: false            # enable tf listener (disable topic subscribers)
    tf.frame_id: "map"
    tf.child_frame_id: "target_position"
    tf.rate_limit: 50.0
    mav_frame: LOCAL_NED

# guided_target
/**/guided_target:
  ros__parameters:
    tf.listen: false           # enable tf listener (disable topic subscribers)
    tf.frame_id: "map"
    tf.child_frame_id: "target_position"
    tf.rate_limit: 50.0

# setpoint_velocity
/**/setpoint_velocity:
  ros__parameters:
    mav_frame: LOCAL_NED

# vfr_hud
# None

# waypoint
/**/mission:
  ros__parameters:
    pull_after_gcs: true        # update mission if gcs updates
    use_mission_item_int: true  # use the MISSION_ITEM_INT message instead of MISSION_ITEM
                                # for uploading waypoints to FCU

# --- mavros extras plugins (same order) ---

# adsb
# None

# debug_value
# None

# distance_sensor
## Currently available orientations:
#    Check http://wiki.ros.org/mavros/Enumerations
##
/**/distance_sensor:
  ros__parameters:
    config: |
      rangefinder_pub:
        id: 0
        frame_id: "lidar"
        # orientation: PITCH_270  # sended by FCU
        field_of_view: 0.0        # XXX TODO
        send_tf: false
        sensor_position: {x:  0.0, y:  0.0, z:  -0.1}
      rangefinder_sub:
        subscriber: true
        id: 1
        orientation: PITCH_270  # only that orientation are supported by APM 3.4+

# image_pub
/**/image:
  ros__parameters:
    frame_id: "px4flow"

# fake_gps
/**/fake_gps:
  ros__parameters:
    # select data source
    use_mocap: true               # ~mocap/pose
    mocap_transform: false        # ~mocap/tf instead of pose
    mocap_withcovariance: false   # ~mocap/pose uses PoseWithCovarianceStamped Message
    use_vision: false             # ~vision (pose)
    use_hil_gps: true
    gps_id: 4
    # origin (default: Zürich)
    geo_origin.lat: 47.3667       # latitude [degrees]
    geo_origin.lon: 8.5500        # longitude [degrees]
    geo_origin.alt: 408.0         # altitude (height over the WGS-84 ellipsoid) [meters]
    eph: 2.0
    epv: 2.0
    horiz_accuracy: 0.5
    vert_accuracy: 0.5
    speed_accuracy: 0.0
    satellites_visible: 6     # virtual number of visible satellites
    fix_type: 3               # type of GPS fix (default: 3D)
    tf.listen: false
    tf.send: false            # send TF?
    tf.frame_id: "map"        # TF frame_id
    tf.child_frame_id: "fix"  # TF child_frame_id
    tf.rate_limit: 10.0       # TF rate
    gps_rate: 5.0             # GPS data publishing rate

# landing_target
/**/landing_target:
  ros__parameters:
    listen_lt: false
    mav_frame: "LOCAL_NED"
    land_target_type: "VISION_FIDUCIAL"
    image.width: 640              # [pixels]
    image.height: 480
    camera.fov_x: 2.0071286398    # default: 115 [degrees]
    camera.fov_y: 2.0071286398
    tf.send: true
    tf.listen: false
    tf.frame_id: "landing_target"
    tf.child_frame_id: "camera_center"
    tf.rate_limit: 10.0
    target_size: {x: 0.3, y: 0.3}

# mocap_pose_estimate
/**/mocap:
  ros__parameters:
    # select mocap source
    use_tf: false   # ~mocap/tf
    use_pose: true  # ~mocap/pose

# mount_control
/**/mount:
  ros__parameters:
    debounce_s: 4.0
    err_threshold_deg: 10.0
    negate_measured_roll: false
    negate_measured_pitch: false
    negate_measured_yaw: false

# odom
/**/odometry:
  ros__parameters:
    fcu.odom_parent_id_des: "map"   # desired parent frame rotation of the FCU's odometry
    fcu.odom_child_id_des: "map"    # desired child frame rotation of the FCU's odometry

# px4flow
/**/px4flow:
  ros__parameters:
    frame_id: "px4flow"
    ranger_fov: 0.118682      # 6.8 degrees at 5 meters, 31 degrees at 1 meter
    ranger_min_range: 0.3     # meters
    ranger_max_range: 5.0     # meters

# vision_pose_estimate
/**/vision_pose:
  ros__parameters:
    tf.listen: false           # enable tf listener (disable topic subscribers)
    tf.frame_id: "map"
    tf.child_frame_id: "vision_estimate"
    tf.rate_limit: 10.0

# vision_speed_estimate
/**/vision_speed:
  ros__parameters:
    listen_twist: true    # enable listen to twist topic, else listen to vec3d topic
    twist_cov: true       # enable listen to twist with covariance topic

# vibration
/**/vibration:
  ros__parameters:
    frame_id: "base_link"

# wheel_odometry
/**/wheel_odometry:
  ros__parameters:
    count: 2                      # number of wheels to compute odometry
    use_rpm: false                # use wheel's RPM instead of cumulative distance to compute odometry
    wheel0: {x: 0.0, y: -0.15, radius: 0.05}  # x-, y-offset (m,NED) and radius (m)
    wheel1: {x: 0.0, y:  0.15, radius: 0.05}  # x-, y-offset (m,NED) and radius (m)
    send_raw: true                # send wheel's RPM and cumulative distance (~/wheel_odometry/rpm, ~/wheel_odometry/distance)
    send_twist: false             # send geometry_msgs/TwistWithCovarianceStamped instead of nav_msgs/Odometry
    frame_id: "map"               # origin frame
    child_frame_id: "base_link"   # body-fixed frame
    vel_error: 0.1                # wheel velocity measurement error 1-std (m/s)
    tf.send: true
    tf.frame_id: "map"
    tf.child_frame_id: "base_link"

# camera
/**/camera:
  ros__parameters:
    frame_id: "base_link"

```



### Vrx Sim
```bash
ros2 launch vrx_gz njord.launch.py world:=sydney_regatta
```

### Ardupilot
```bash
sim_vehicle.py -v Rover -f rover-skid --model JSON  --console --map --custom-location='-33.724223,150.679736,0.0,0.0'
```

### Mavros

```bash
ros2 launch mavros apm.launch fcu_url:=udp://:14550@127.0.0.1:14555
```

### NAV2

```bash
ros2 launch my_nav2_node gps_waypoint_follower.launch.py
```

### Whatsapp kodu

```bash
ros2 run topic_tools relay /cmd_vel /mavros/setpoint_velocity/cmd_vel_unstamped
```

### Yolo, Position Publisher, Occupancy Grid....

```bash
ros2 launch central_launch_pkg multi_package_launch.py
```

### Gps Waypoint to Local Waypoint Transformer

```bash
ros2 run gps_position_transformer transformer
```

