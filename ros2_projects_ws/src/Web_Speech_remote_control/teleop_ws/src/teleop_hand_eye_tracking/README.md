# teleop_hand_eye_tracking

This package provides the gesture-first input used in the prototype.
Hand landmarks become G1 arm goals, while head orientation can control the
robot's torso. The result is an approachable interaction model based on natural
movement rather than a specialist robot controller.

## Instalacja zależności

### 1. Usuń obecne wersje konfliktujących pakietów

```bash
pip uninstall -y mediapipe protobuf opencv-python opencv-contrib-python numpy matplotlib
```

### 2. Zainstaluj stabilny zestaw z pliku

```bash
pip install --ignore-installed -r requirements.txt

source install/setup.bash

pip3 install -e ./$(colcon list --packages-select teleop_hand_eye_tracking --paths-only)/teleop_hand_eye_tracking/unitree_sdk2_python
```

