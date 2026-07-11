# Not Aura

Not Aura is a prototype for intuitive humanoid telepresence.
It combines natural head and hand input, live robot vision and an augmented
operator view so that controlling a remote Unitree G1 feels closer to moving
your own body than operating a conventional robot console.

## The problem

Traditional robot control is fragmented across joysticks, dashboards and expert
tools. That makes remote operation hard to learn and limits who can participate.
Our prototype unifies sensing, control and feedback in one ROS 2 system while
keeping each motion capability behind an explicit on/off service.

## The experience

- **Look:** XREAL IMU data drives the robot torso and the virtual camera.
- **Reach:** hand tracking maps human gestures to G1 arm goals.
- **Understand:** OAK depth and person detection add spatial awareness.
- **See:** camera and desktop streams are composed into an AR-style scene.
- **Connect safely:** WebRTC/MQTT, SROS 2 and simulation support remote testing
  before control is handed to physical hardware.

## Judging criteria

| Criterion | Evidence in this repository |
|---|---|
| **Creativity** | A wearable, gesture-first interface combines XR and humanoid robotics in a single operator experience. |
| **Practicality** | The same launch flow supports simulation and a physical G1, with independently enabled features. |
| **Presentation** | Human motion, robot response and AR feedback form an immediate, easy-to-follow demo. |
| **Technical complexity** | Multiple hardware inputs, perception models, streaming transports and secure ROS 2 nodes work together in real time. |
| **Design** | Natural input, visual feedback and modular controls keep the interaction approachable. |

## System overview

The repository combines Python, C++, ROS 2, web technologies and embedded
integrations to enable real-time robot control, perception and remote
collaboration. The diagram below shows the broader communication architecture.

<img width="1831" height="1032" alt="image" src="https://github.com/user-attachments/assets/01a11f1f-c1e7-4e91-ad73-cbb7b1e43815" />


## Key components

### G1 teleoperation

- Maps tracked hands to `/g1pilot/hand_goal/left` and
  `/g1pilot/hand_goal/right`.
- Maps XREAL head orientation to torso control.
- Supports a simulation-first flow and physical G1 operation from the same
  bringup package.

### XR and perception

- Reads XREAL IMU data over TCP and estimates orientation with a Madgwick
  filter.
- Integrates OAK RGB/depth data and YOLO person detection.
- Renders camera feeds, desktop streams and spatial markers in a virtual scene.

### Remote communication

- Uses ROS 2 topics and services for modular, observable control.
- Supports WebRTC/MQTT bridges and optional web clients for remote operation.
- Uses Tailscale in the extended stack to share services across locations.

### Security and reliability

- Provides SROS 2 enclaves and signed permissions for ROS communication.
- Provides mutual-TLS certificates for secured MQTT transport.
- Keeps hand, IMU and torso control disabled until the operator explicitly
  enables each capability.

## Potential use cases

- **Remote assistance:** let a human operator see, point and interact through a
  humanoid robot.
- **Inspection:** combine depth perception and AR markers in spaces that are
  inconvenient or unsafe for a person.
- **Training and research:** compare natural gesture control in simulation and
  on physical hardware.
- **Accessible interfaces:** explore alternatives to joystick-heavy robot
  control for users with different abilities.

# Testing (after initial configuration of all project components available in appropriate subfolders)

1. setup django server
```bash
cd ~/Web_Speech_remote_control/api
poetry run python manage.py runserver 0.0.0.0:8000
```
2. setup react frontend
```bash
cd ~/Web_Speech_remote_control/frontend_ws
npm run dev
```

3. setup teleop
```bash
source ~/ros2_projects_ws/install/setup.bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash

# run
ros2 launch teleop_bringup teleop_system.launch.py security:=True # or False for unsecured use_google_stun:=False

```

4. run unity simulation
```bash
source ~/ros2_projects_ws/install/setup.bash
ros2 launch unity_sim unity_sim.launch.py 
```

5. setup tailscale
```bash
sudo tailscale serve reset

sudo tailscale funnel --bg --set-path /ws http://127.0.0.1:8000
sudo tailscale funnel --bg --set-path /api/login/ http://127.0.0.1:8000/api/login/
sudo tailscale funnel --bg --set-path /api/register/ http://127.0.0.1:8000/api/register/
sudo tailscale funnel --bg --set-path / http://127.0.0.1:5173


# to turn off 
# tailscale funnel --https=443 off

# turn tailscale on device to control the robot
```
go to website using generated address from url with react port ...etc. https://name.tail123g3a.ts.net/

6. setup electron app or ros2_webrtc_bridge
```bash
#electron
cd ~/Web_Speech_remote_control/electron
npm run start
```

7. run teleop_bringup
```bash
cd ~/Web_Speech_remote_control/teleop_bringup/
source ~/ros2_projects_ws/install/setup.bash

source install/setup.bash
ros2 launch teleop_bringup twist_joy_g1.launch.py 
```

8. run unity simulation
```bash
## automated distrobox command
cd ~/ros2_projects_ws
./scripts/distrobox
## in new distrobox terminal run simulation
ros2 launch knml_bringup sim_basic.launch.py 

#simple run simulation
cd ~/ros2_projects_ws && distrobox enter kalman_ws -- bash -c "source install/setup.bash && ros2 launch knml_bringup sim_basic.launch.py"

#or only simulation without teleoperation
ros2 launch unity_sim unity_sim.launch.py 
```

9. run teleop
```bash
# source
source ~/ros2_projects_ws/install/setup.bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash
ros2 launch teleop_bringup teleop_system.launch.py security:=True # or False for unsecured

# or separatelly
source ~/ros2_projects_ws/install/setup.bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash
ros2 launch teleop_webrtc_joy webrtc_client.launch.py

source ~/ros2_projects_ws/install/setup.bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash
ros2 launch teleop_joy_cmd joy_cmd_g1.launch.py 

source ~/ros2_projects_ws/install/setup.bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash
ros2 launch teleop_cmd_unity unity_sim_wheel.launch.py 

# or separatelly secured version
source ~/ros2_projects_ws/install/setup.bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash
ros2 launch teleop_webrtc_joy sec_webrtc_client.launch.py

source ~/ros2_projects_ws/install/setup.bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash
ros2 launch teleop_joy_cmd sec_joy_cmd_g1.launch.py 

source ~/ros2_projects_ws/install/setup.bash
source ~/Web_Speech_remote_control/teleop_ws/install/setup.bash
ros2 launch teleop_cmd_unity sec_unity_sim_wheel.launch.py 
```
