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

### Yolo, Position Publisher, Occupancy Grid....

```bash
ros2 launch central_launch_pkg multi_package_launch.py
```

### Nav Controller

```bash
ros2 run nav_controller control
```
