# Navida Yazılım Doc

### Colcon Workspace'i Ayağa Kaldırma
1. Workspace'i klonla.

    ```bash
    git clone https://github.com/Navi-IDA/ros2-ws.git 
    ```


    veya ssh keyin varsa 

    ```bash    
    git clone git@github.com:Navi-IDA/ros2-ws.git
    ```


2. Colconu buildle ve sourcela.

    ```bash
    cd ros2-ws
    colcon build --merge-install --cmake-args=-DCMAKE_BUILD_TYPE=Release
    source install/setup.bash
    ```
    --cmake-args kısmını sadece cmake buildlerken verseniz yeterli. Hızlı build almak için bir kere yaptıktan sonra kullanmayabilirsiniz.

### Realsense
```bash
ros2 launch realsense2_camera rs_launch.py depth_module.depth_profile:=1280x720x30 pointcloud.enable:=true pointcloud.ordered_pc:=true
```

### Yolo Node
```bash
ros2 run yolov4_detection detector
```

### Center and Distance Publisher Node

```bash
ros2 run zed_distance_publisher distance_publisher
```

### TF2 Publisher

```bash
ros2 run tf2_listener tf2_broadcaster
```

### Static World TF Publisher

```bash
ros2 run relative_position_node position_publisher
```

### Map and A*

```bash
ros2 run gps_to_occupancygrid gps_to_occupancygrid_node
```

### Rviz

```bash
rviz2 rviz2
```
Ardından istediğiniz topicleri ekleyerek görüntü alabilirsiniz.