<div align="center">
  <h1>QRB ROS System Monitor</h1>
  <p align="center">
  </p>
  <p>ROS Packages for providing system informations for robotics system</p>
  
  <a href="https://ubuntu.com/download/qualcomm-iot" target="_blank"><img src="https://img.shields.io/badge/Qualcomm%20Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Qualcomm Ubuntu"></a>
  <a href="https://docs.ros.org/en/jazzy/" target="_blank"><img src="https://img.shields.io/badge/ROS%20Jazzy-1c428a?style=for-the-badge&logo=ros&logoColor=white" alt="Jazzy"></a>
  
</div>

---

## 👋 Overview

The [QRB ROS System Monitor](https://github.com/qualcomm-qrb-ros/qrb_ros_system_monitor) is a ROS package to privide system informations for robotics. It provides:

- /cpu: Topic for CPU usage informations
- /memory: Topic for memory usage informations
- /disk: Topic for disk usage informations
- /swap: Topic for swap partition informations
- /temperature: Topic for CPU temperature
- /battery: Topic for device battery level
- /system_info_server: Service for providing system informations.

<div align="center">
<!--   <img src="./docs/assets/architecture.png" alt="architecture"> -->
</div>

<br>

## 🔎 Table of contents
  * [APIs](#-apis)
     * [`qrb_ros_system_monitor` APIs](#-qrb_ros_system_monitor-apis)
     * [`qrb_system_monitor` APIs](#-qrb_system_monitor-apis)
  * [Supported targets](#-supported-targets)
  * [Installation](#-installation)
  * [Usage](#-usage)
     * [Starting the system_monitor node](#start-the-system_monitor-node)
     * [Enable mutiple streams](#enable-multiple-streams)
     * [Enable zero copy transport](#enable-zero-copy-transport)
  * [Build from source](#-build-from-source)
  * [Contributing](#-contributing)
  * [Contributors](#%EF%B8%8F-contributors)
  * [FAQs](#-faqs)
  * [License](#-license)

## ⚓ APIs

### 🔹 `qrb_ros_system_monitor` APIs

#### ROS interfaces

<table>
  <tr>
    <th>Interface</th>
    <th>Name</th>
    <th>Type</th>
    <td>Description</td>
  </tr>
  <tr>
    <td>Publisher</td>
    <td>/cam${system_monitor_id}_${stream_name}</td>
    <td>sensor_msgs/msg/Image</td>
    <td>output image</td>
  </tr>
  <tr>
    <td></td>
    <td>/cam${system_monitor_id}_system_monitor_info</td>
    <td>sensor_msgs/msg/system_monitorInfo</td>
    <td>system_monitor information</td>
  </tr>
</table>

#### ROS parameters

<table>
  <tr>
    <th>Name</th>
    <th>Type</th>
    <th>Description</td>
    <th>Default Value</td>
  </tr>
  <tr>
    <td>system_monitor_id</td>
    <td>int64</td>
    <td>The system_monitor device ID</td>
    <td>0</td>
  </tr>
  <tr>
    <td>stream_size</td>
    <td>uint64</td>
    <td>Count of system_monitor stream</td>
    <td>1</td>
  </tr>
  <tr>
    <td>stream_name</td>
    <td>string[]</td>
    <td>system_monitor stream names</td>
    <td>["stream1"]</td>
  </tr>
  <tr>
    <td>${stream_name}.width</td>
    <td>uint32</td>
    <td>image width</td>
    <td>1920</td>
  </tr>  
  <tr>
    <td>${stream_name}.height</td>
    <td>uint32</td>
    <td>image height</td>
    <td>1080</td>
  </tr>
  <tr>
    <td>${stream_name}.fps</td>
    <td>uint32</td>
    <td>output image frequency(Hz)</td>
    <td>30</td>
  </tr>
  <tr>
    <td>system_monitor_info_path</td>
    <td>string</td>
    <td>system_monitor metadata file path</td>
    <td>config/system_monitor_info_imx577.yaml</td>
  </tr>
</table>

> [!Note]
> The parameter values should be set accroding to the actual system_monitor hardware capabilities.

### 🔹 `qrb_system_monitor` APIs

<table>
  <tr>
    <th>Function</th>
    <th>Parameters</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>int create_system_monitor(system_monitorType type, uint32_t system_monitor_id)</td>
    <td>type: system_monitor type, system_monitor_id: system_monitor id</td>
    <td>Creates a system_monitor and returns the system_monitor index on success.</td>
  </tr>
  <tr>
    <td>bool set_system_monitor_parameter(int index, system_monitorConfigure & param)</td>
    <td>index: system_monitor index,  param: system_monitor parameters</td>
    <td>Returns true when the system_monitor parameters are set successfully.</td>
  </tr>
  <tr>
    <td>bool start_system_monitor(int index)</td>
    <td>index: system_monitor index </td>
    <td>Starts the system_monitor. Returns true when started successfully.</td>
  </tr>
  <tr>
    <td>void stop_system_monitor(int index)</td>
    <td>index: system_monitor index </td>
    <td>Stop the system_monitor</td>
  </tr>
  <tr>
    <td>bool register_callback(int index, ImageCallback image_cb, PointCloudCallback point_cloud_cb)</td>
    <td>index: system_monitor index，Image_cb: image callback，Point_cloud_cb: point cloud msg callback </td>
    <td>Registers the callback for image and point cloud messages.</td>
  </tr>
</table>

> [!Note]
> The parameter values should be set accroding to the actual system_monitor hardware capabilities.

## 🎯 Supported targets

<table >
  <tr>
    <th>Development Hardware</th>
    <td>Qualcomm Dragonwing™ RB3 Gen2</td>
    <td>Qualcomm Dragonwing™ IQ-9075 EVK</td>
  </tr>
  <tr>
    <th>Hardware Overview</th>
    <th><a href="https://www.qualcomm.com/developer/hardware/rb3-gen-2-development-kit"><img src="https://s7d1.scene7.com/is/image/dmqualcommprod/rb3-gen2-carousel?fmt=webp-alpha&qlt=85" width="180"/></a></th>
    <th><a href="https://www.qualcomm.com/products/internet-of-things/industrial-processors/iq9-series/iq-9075"><img src="https://s7d1.scene7.com/is/image/dmqualcommprod/dragonwing-IQ-9075-EVK?$QC_Responsive$&fmt=png-alpha" width="160"></a></th>
  </tr>
  <tr>
    <th>MIPI-CSI system_monitor Support</th>
    <td><li>IMX577(12MP)</li><li>OV9282(1MP)</li></td>
    <td><li>IMX577(12MP)</li><li>OV9282(1MP)</li></td>
  </tr>
  <tr>
    <th>GMSL system_monitor Support</th>
    <td>Leopard Imaging AR0231 GMSL2</td>
    <td><li>LI-VENUS-OX03F10-96717-120H(Bayer)</li><li>LI-VENUS-OX03F10-OAX40-GM2A-118H(YUV)</li></td>
  </tr>
</table>

---

## ✨ Installation

> [!IMPORTANT]
> **PREREQUISITES**: The following steps need to be run on **Qualcomm Ubuntu** and **ROS Jazzy**.<br>
> Reference [Install Ubuntu on Qualcomm IoT Platforms](https://ubuntu.com/download/qualcomm-iot) and [Install ROS Jazzy](https://docs.ros.org/en/jazzy/index.html) to setup environment. <br>
> For Qualcomm Linux, please check out the [Qualcomm Intelligent Robotics Product SDK](https://docs.qualcomm.com/bundle/publicresource/topics/80-70018-265/introduction_1.html?vproduct=1601111740013072&version=1.4&facet=Qualcomm%20Intelligent%20Robotics%20Product%20(QIRP)%20SDK) documents.

Add Qualcomm IOT PPA for Ubuntu:

```bash
sudo add-apt-repository ppa:ubuntu-qcom-iot/qcom-noble-ppa
sudo add-apt-repository ppa:ubuntu-qcom-iot/qirp
sudo apt update
```

Install Debian package:

```bash
sudo apt install ros-jazzy-qrb-ros-system_monitor
```

## 🚀 Usage

### Start the system_monitor node

```bash
source /opt/ros/jazzy/setup.bash
ros2 run qrb_ros_system_monitor qrb_ros_system_monitor
```

These commands will start all system monitor ROS nodes. The output for these commands:

```bash
root@kalama:~/ros_ws# ros2 run qrb_ros_system_monitor qrb_ros_system_monitor 
[INFO] [1754041095.755470762] [cpu_monitor]: CPU Monitor start
[INFO] [1754041095.775654235] [memory_monitor]: Memory Monitor start
[INFO] [1754041095.783700187] [temperature_monitor]: Temperature Monitor start
[INFO] [1754041095.791240297] [disk_monitor]: DISK Monitor start
[INFO] [1754041095.798755668] [swap_monitor]: Swap Monitor start
[INFO] [1754041095.804467314] [battery_monitor]: Battery Monitor start
[INFO] [1754041095.809501190] [system_info_server]: System info server start
...
```

Then you can check ROS topics with `ros2 topic list`.

```bash
/battery
/cpu
/disk
/memory
/parameter_events
/rosout
/swap
/temperature
```

This is the output for `/cpu` topic:
```
usage: 8.870357513427734
user: 2263131
nice: 17852
system: 12022972
idle: 3165299899
iowait: 235972
irq: 11140524
softirq: 4153651
steal: 294225
guest: 0
guest_nice: 0
---
```

### Use single monitor node only

The `qrb_ros_system_monitor` supports directly sharing image `dmabuf_fd` between nodes, which can avoid image data memory copy with DDS.

For detail about this feature, see https://docs.ros.org/en/rolling/Concepts/Intermediate/About-Composition.html

We recommend using `launch` to compose multiple nodes:

```python
def generate_launch_description():
    container = ComposableNodeContainer(
        name='my_container',
        namespace='',
        package='rclcpp_components',
        executable='component_container',
        composable_node_descriptions=[
            ComposableNode(
                package='qrb_ros_system_monitor',
                plugin='qrb_ros::system_monitor::system_monitorNode',
                name='system_monitor_node',
                parameters=[{
                    # ...
                }]
            ),
            ComposableNode(
                package='qrb_ros_system_monitor',
                plugin='qrb_ros::system_monitor::TestNode',
                name='sub_node',
            )
        ],
        output='screen',
    )

    return launch.LaunchDescription([container])
```

---

## 👨‍💻 Build from source

Download the source code and build with colcon
```bash
source /opt/ros/jazzy/setup.bash
git clone https://github.com/qualcomm-qrb-ros/qrb_ros_system_monitor.git
colcon build
```

Run and debug

```bash
source install/setup.bash
ros2 run qrb_ros_system_monitor qrb_ros_system_monitor
```

## 🤝 Contributing

We love community contributions! Get started by reading our [CONTRIBUTING.md](CONTRIBUTING.md).<br>
Feel free to create an issue for bug report, feature requests or any discussion💡.

## ❤️ Contributors

Thanks to all our contributors who have helped make this project better!

<table>
  <tr>
    <td align="center"><a href="https://github.com/penww"><img src="https://avatars.githubusercontent.com/u/97950764?v=4" width="100" height="100" alt="penww"/><br /><sub><b>penww</b></sub></a></td>
    <td align="center"><a href="https://github.com/jiaxshi"><img src="https://avatars.githubusercontent.com/u/147487233?v=4" width="100" height="100" alt="jiaxshi"/><br /><sub><b>jiaxshi</b></sub></a></td>
  </tr>
</table>

## 📜 License

Project is licensed under the [BSD-3-Clause](https://spdx.org/licenses/BSD-3-Clause.html) License. See [LICENSE](./LICENSE) for the full license text.
