# Launch-File-in-ROS2

# ROS 2 Launch Files Project Report

## Project Overview
Creating a ROS 2 launch file to simultaneously start temperature publisher and subscriber nodes with a single command.

---

## 1. Introduction

### Purpose of Launch Files in ROS 2
Launch files enable efficient management of multi-node systems by:
- Starting multiple nodes with a single command
- Configuring parameters at runtime
- Managing topic remappings and namespaces
- Ensuring reproducible system startup

### Project Scope
**Launch File:** `main.launch.py`  
**Nodes Launched:**
- Temperature Publisher (`temp_pub.py`)
- Temperature Subscriber (`temp_sub.py`)

---

## 2. Materials & Tools Used

| Component | Details |
|-----------|---------|
| **ROS 2 Distro** | Humble Hawksbill |
| **OS** | Ubuntu 22.04 LTS |
| **Language** | Python 3.10 |
| **Build Tool** | colcon |
| **Testing Tools** | `ros2 launch`, `ros2 node`, `ros2 topic` |

---

## 3. Methodology

### 3.1 Launch File Creation

**Directory Structure:**
```
task1_ws/
└── src/
    └── temp_pkg/
        ├── CMakeLists.txt
        ├── package.xml
        ├── launch/
        │   └── main.launch.py
        └── scripts/
            ├── temp_pub.py
            └── temp_sub.py
```

### 3.2 Launch File Code

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/d2c1992e-2019-4f86-b8f0-74e0df8f5fdb" />

### 3.3 Key Components Explained

#### `launch.LaunchDescription()`
- Container for all launch actions
- Returns list of nodes and configurations
- Entry point for every launch file

#### `launch_ros.actions.Node()`
- Launches a ROS 2 node
- **Parameters:**
  - `package`: Package name containing the node
  - `executable`: Node executable name
  - `name`: Runtime node name
  - `output`: Where to display output (`'screen'` or `'log'`)

### 3.4 CMakeLists.txt Configuration

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/a64610bf-1cd6-4a2b-8a06-8d53e37c8fed" />

### 3.5 Build and Run

```bash
# Build
cd task1_ws/
colcon build 
source install/setup.bash

# Run launch file
ros2 launch temp_pkg main.launch.py
```

---

### Debugging Tools

| Tool | Purpose | Command |
|------|---------|---------|
| `ros2 node list` | List running nodes | `ros2 node list` |
| `ros2 topic list` | Show active topics | `ros2 topic list` |
| `ros2 topic echo` | Monitor topic data | `ros2 topic echo /temperature` |
| `rqt_graph` | Visualize node graph | `rqt_graph` |

---

## 6. Testing & Results

### Test 1: Basic Launch

**Command:**
```bash
ros2 launch temp_pkg main.launch.py
```

**Output:**
```
[INFO] [temperature_publisher]: Temperature Publisher Node Started
[INFO] [temperature_subscriber]: Temperature Subscriber Node Started
[INFO] [temperature_publisher]: Publishing: 24.56°C
[INFO] [temperature_subscriber]: Temperature OK: 24.56°C
[INFO] [temperature_publisher]: Publishing: 29.34°C
[WARN] [temperature_subscriber]: Temperature TOO HIGH: 29.34°C
[INFO] [temperature_publisher]: Publishing: 21.45°C
[WARN] [temperature_subscriber]: Temperature TOO LOW: 21.45°C
```
<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/c448dffa-cb51-4fbc-ae77-cecca20a9fd7" />

### Test 2: Verification

**Check Running Nodes:**
```bash
$ ros2 node list
/temperature_publisher
/temperature_subscriber
```

**Check Topics:**
```bash
$ ros2 topic list
/parameter_events
/rosout
/temperature

$ ros2 topic info /temperature
Type: std_msgs/msg/Float32
Publisher count: 1
Subscription count: 1
```

**Monitor Data:**
```bash
$ ros2 topic echo /temperature
data: 24.56
---
data: 29.34
---
```

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/fc850ab1-78f5-44fe-8bfe-51140a5c5161" />


### Results Summary

| Test | Status | Notes |
|------|--------|-------|
| Both nodes launch | ✅ PASSED | Nodes start simultaneously |
| Communication | ✅ PASSED | Data flows correctly |
| Output display | ✅ PASSED | Messages visible in terminal |

---

## 7. Conclusion

### Key Learnings

1. **Launch File Benefits:**
   - Single command replaces multiple terminal windows
   - Simplified system deployment
   - Easier team collaboration

2. **Critical Components:**
   - `LaunchDescription()` - Container for launch entities
   - `Node()` - Launches individual nodes
   - `output='screen'` - Displays node output

3. **Common Error:**
   - Missing callback function in subscription (most frequent issue)
   - Solution: Always provide 4 parameters to `create_subscription()`

4. **Best Practices:**
   - Install launch files in CMakeLists.txt
   - Always rebuild after changes
   - Source workspace after build

### Skills Acquired

- ✅ Launch file creation and syntax
- ✅ Multi-node system management
- ✅ ROS 2 build system (colcon)
- ✅ Debugging with ROS 2 CLI tools
- ✅ System integration and testing

---

## Structure

<img width="508" height="762" alt="image" src="https://github.com/user-attachments/assets/9e3fc86b-86ca-414f-a2f8-2192ea6eb15d" />

---

**Author:** Akhileshwar Pratap Singh  
**Date:** December 2025 
**License:** MIT
