---
title: "Internship Progress Week 10 (September 03 ~ September 10)"
date: 2026-09-10 15:00:00 +0530
categories: [Internship 2026, Progress]
tags: [internship, progress, week-10, nav2, ros2, humble, jderobot, gazebo-harmonic, xvfb, x11vnc, rviz2, amazon-warehouse, follow-line, macos]
published: true
---

During **Week 10**, our focus centered on completing local testing and development without relying on remote servers. We executed a comprehensive benchmarking of the 3D graphical rendering stack on macOS Apple Silicon Docker, validated the onboard camera pipeline for the FollowLine exercise, and expanded the **ROS 2 Nav2** integration to the **Amazon Warehouse** challenge.

---

## 1. 3D GUI Rendering & VNC Evaluation on macOS Docker (Tasks T31, T32, T33)

To resolve the GUI rendering challenges on macOS Apple Silicon, we designed isolated benchmarks to test X11 virtual framebuffers, VNC protocols, and OpenGL compatibility outside the RoboticsAcademy web wrapper.

### A. Task T33: Xvfb and x11vnc 3D Benchmarking
We evaluated whether Xvfb (`DISPLAY=:99`), Openbox window manager, and `x11vnc` could render 3D ROS 2 applications inside an x86_64 Docker container under Rosetta 2 emulation.

```
+-------------------------------------------------------------------------------+
|                             TASK T33 TEST RESULTS                             |
+---------------------+-------------------------+---------------+---------------+
| Application Tested  | Rendering Pipeline      | Status        | Framerate     |
+---------------------+-------------------------+---------------+---------------+
| OpenGL Diagnostics  | Mesa llvmpipe (SW)      | PASSED        | N/A           |
| RViz2 (ROS 2 GUI)   | OGRE 1 / Standard GL    | PASSED        | 31 FPS        |
| Gazebo Harmonic GUI | QtQuick / QML GL Shaders| FAILED (Black)| 0 FPS (Blank) |
+---------------------+-------------------------+---------------+---------------+
```

* **RViz2 3D OpenGL Test (PASSED):** RViz2 launched cleanly, rendering 3D grids and interactive frames at **31 FPS** over VNC.

<div align="center" style="margin: 20px 0;">
  <img src="/2026-internship-Devesh_Mishra/assets/img/posts/task33_xvfb_gui.png" alt="Task T33 RViz2 3D Grid Rendering at 31 FPS in Xvfb over VNC" style="width: 90%; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);" />
  <p style="margin-top: 8px; font-size: 0.9em; color: #555;">Figure 1: RViz2 3D OpenGL grid and interactive coordinate display rendering at 31 FPS over VNC inside macOS Docker.</p>
</div>

* **Gazebo GUI Viewport Test (FAILED - Black Viewport):** Gazebo started its UI shell and menus, but the 3D scene viewport remained black due to QtQuick/QML shader compositing limitations under Rosetta 2 software OpenGL (`llvmpipe`).

<div align="center" style="margin: 20px 0;">
  <img src="/2026-internship-Devesh_Mishra/assets/img/posts/gazebo_shapes_xvfb.png" alt="Task T33 Gazebo GUI Black Viewport Under Rosetta Software OpenGL" style="width: 90%; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);" />
  <p style="margin-top: 8px; font-size: 0.9em; color: #555;">Figure 2: Gazebo QtQuick UI window opening with an uncomposited 3D viewport under software rasterization.</p>
</div>

---

### B. Task T31: FollowLine Onboard Camera Streaming
We verified that while the Gazebo QtQuick GUI viewport requires hardware-level shaders, the **headless Gazebo simulation engine (`gz sim -s`) generates sensor frames reliably on macOS**.

* **Live Camera Stream Extraction:** Subscribed directly to `/cam_f1_left/image_raw` ($640 \times 480$ RGB) to capture the live track view.
* **WebGUI Streaming Fix:** Patched `MeasuringThreadingGUI` in `gui_interfaces` to stream image payloads immediately over WebSocket without stalling on dropped acknowledgments.

<div align="center" style="margin: 20px 0;">
  <img src="/2026-internship-Devesh_Mishra/assets/img/posts/follow_line_gazebo_camera.png" alt="Task T31 FollowLine Onboard F1 Camera Feed Extracted from Gazebo" style="width: 80%; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);" />
  <p style="margin-top: 8px; font-size: 0.9em; color: #555;">Figure 3: Live 640x480 F1 camera frame extracted from headless Gazebo simulation showing the red racing line and start grid.</p>
</div>

---

### C. Task T32: Standalone Gazebo Jetty / ROS 2 Jazzy Container
We packaged an independent Docker testing image `task32_gazebo_jetty:latest` based on Ubuntu 24.04 (Noble) with `gz-harmonic`, `xvfb`, `x11vnc`, and `openbox` to test upstream Gazebo rendering improvements outside the RADI distribution.

---

## 2. Nav2 Navigation Stack Integration (Tasks T1 & T2)

We implemented ROS 2 Nav2 navigation capabilities inside RoboticsAcademy exercises:

### A. Task T1: Global Navigation Integration
* **Launcher (`global_navigation_nav2.launch.py`):** Configured headless Gazebo simulation, identity transform publisher (`map -> odom`), map server lifecycle manager, and Nav2 navigation nodes.
* **Bridge Node (`nav2_bridge.py`):** Translates WebGUI click targets (`/webgui/current_target`) to `/goal_pose`, returns planned routes (`/plan` to `/webgui/path`), and converts costmap occupancy grids into visual diagnostic images.

### B. Task T2: Amazon Warehouse Nav2 Integration
* **Custom Warehouse Parameters (`nav2_params.yaml`):** Tuned Nav2 parameters specifically for the Amazon Kiva AGV platform, including rolling local costmap ($6\text{ m} \times 6\text{ m}$ at $0.05\text{ m}$ resolution), laser scan obstacle layer (`/amazon_robot/scan`), and circular robot footprint ($0.45\text{ m}$ radius).
* **Launcher & Bridge (`amazon_warehouse_nav2.launch.py` & `nav2_warehouse_bridge.py`):** Brings up the warehouse world (`warehouse1.world`), loads the warehouse occupancy map (`map.yaml`), and provides high-level dispatch interfaces for automated shelf pickup and transit.

---

## 3. Key Findings & Recommended macOS Architecture

1. **Decoupled Simulation & Visualization:** For developers on macOS Apple Silicon, running Gazebo in headless mode (`gz sim -s`) alongside direct ROS 2 topic visualization (RViz2 or WebGUI canvas widgets) provides a stable, high-performance workflow.
2. **Nav2 Compatibility:** Both City Navigation and Amazon Warehouse challenges can operate on standard Nav2 launch descriptions across local and remote container runtimes.
