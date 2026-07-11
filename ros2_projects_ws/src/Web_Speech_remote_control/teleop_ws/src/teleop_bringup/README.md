# teleop_bringup

This package coordinates the nodes used in the Not Aura demo. It
offers repeatable launch paths for simulation and the physical G1, plus secured
and unsecured MQTT modes for development. Keeping orchestration in one place
makes the live presentation easier to start, explain and recover.

## Network diagnostics

```bash
#for testing mqtt/rtps
sudo wireshark
```

## Unsecured setup

launching mosquitto broker unsecured
```bash
# 1. Stop the Background Service
# If you installed Mosquitto via apt, it is likely running as a service. Run this command to stop it:
sudo systemctl stop mosquitto

cd ~/Web_Speech_remote_control/sros2_ws/mqtt_certs
mosquitto -c ~/Web_Speech_remote_control/sros2_ws/mqtt_certs/mosquitto.conf -v
```

launching mqtt_ros2 bridge unsecured
```bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash 
export FASTRTPS_DEFAULT_PROFILES_FILE=$(ros2 pkg prefix teleop_mqtt_client)/share/teleop_mqtt_client/config/fastdds_udp_only.xml
ros2 launch teleop_mqtt_client mqtt_client.launch.py 
```

launching mqtt_ros2 bridge unsecured
```bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash 
export FASTRTPS_DEFAULT_PROFILES_FILE=$(ros2 pkg prefix teleop_mqtt_client)/share/teleop_mqtt_client/config/fastdds_udp_only.xml
ros2 run teleop_mqtt_client iot_sender 
```

## Secured setup

Launching the secured Mosquitto broker:

```bash
cd ~/Web_Speech_remote_control/sros2_ws/mqtt_certs
mosquitto -c ~/Web_Speech_remote_control/sros2_ws/mqtt_certs/sec_mosquitto.conf -v
```

Launching the secured MQTT–ROS 2 bridge:

```bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash 
export FASTRTPS_DEFAULT_PROFILES_FILE=$(ros2 pkg prefix teleop_mqtt_client)/share/teleop_mqtt_client/config/fastdds_udp_only.xml
ros2 launch teleop_mqtt_client sec_mqtt_client.launch.py 
```

Launching the secured IoT sender:

```bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash 
export FASTRTPS_DEFAULT_PROFILES_FILE=$(ros2 pkg prefix teleop_mqtt_client)/share/teleop_mqtt_client/config/fastdds_udp_only.xml
ros2 run teleop_mqtt_client sec_iot_sender 
```
