# Weekly Update - Week 10

## Overview
During **Week 10**, we focused on executing and validating tasks locally on macOS without requiring remote server infrastructure. We tested 3D GUI rendering over VNC, validated the FollowLine onboard camera pipeline in headless simulation, and expanded the **ROS 2 Nav2** integration to the **Amazon Warehouse** challenge.

---

## Tasks Completed

### 1. 3D GUI & VNC Testing on macOS Docker
To evaluate graphical rendering on macOS Apple Silicon, we tested an isolated Xvfb virtual framebuffer and x11vnc server inside Docker.

* **RViz2 3D OpenGL Rendering (Passed):** Successfully launched RViz2 inside the container and connected via VNC. The 3D grid and interactive coordinate displays rendered smoothly at a steady **31 FPS** under software OpenGL rasterization.

<p align="center">
  <img src="../docs/assets/img/posts/task33_xvfb_gui.png" alt="RViz2 3D Grid at 31 FPS in Xvfb over VNC" width="85%" style="border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);">
</p>

* **Gazebo Standalone GUI (Black Viewport):** Tested the standalone Gazebo GUI viewer inside the same VNC environment. The application window and menus loaded, but the 3D scene viewport remained black due to QtQuick/QML shader compositing limitations under Rosetta 2 software OpenGL.

<p align="center">
  <img src="../docs/assets/img/posts/gazebo_shapes_xvfb.png" alt="Gazebo GUI Viewport under Software OpenGL" width="85%" style="border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);">
</p>

---

### 2. FollowLine Onboard Camera Streaming
We verified that while the Gazebo GUI viewport requires hardware-level shaders, the **headless Gazebo simulation engine generates sensor frames reliably on macOS**.

* **Live Camera Stream Extraction:** Subscribed directly to the F1 car onboard camera topic to extract real-time frames ($640 \times 480$ RGB).
* **WebGUI Image Delivery:** Streamlined WebSocket image delivery in the web interface to display live camera feeds without latency.

<p align="center">
  <img src="../docs/assets/img/posts/follow_line_gazebo_camera.png" alt="FollowLine Onboard Camera Feed from Gazebo" width="75%" style="border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);">
</p>

---

### 3. Nav2 Integration for Amazon Warehouse
Following our work on the City Navigation challenge, we adapted the **Nav2 Navigation Stack** for the **Amazon Warehouse** exercise:

* **Custom Costmaps & Footprint:** Configured local and global costmaps with obstacle inflation tailored for the Amazon Kiva AGV platform.
* **Warehouse Map & Lifecycle Management:** Set up the Nav2 map server to load the warehouse layout and manage node lifecycle transitions.
* **Navigation Bridge:** Created a bridge node to handle goal dispatch and return planned paths to the web interface.

---

## Key Conclusions

* **Recommended macOS Workflow:** Running Gazebo in headless mode alongside direct ROS 2 topic visualization (RViz2 or WebGUI canvas) provides a stable and performant simulation environment on macOS Apple Silicon.
* **Standardized Nav2 Pipeline:** Both City Navigation and Amazon Warehouse exercises can operate reliably using standard Nav2 launch configurations.
