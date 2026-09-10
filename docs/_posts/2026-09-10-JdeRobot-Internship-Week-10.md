---
title: "Internship Progress Week 10 (September 03 ~ September 10)"
date: 2026-09-10 15:00:00 +0530
categories: [Internship 2026, Progress]
tags: [internship, progress, week-10, nav2, ros2, humble, jderobot, gazebo, xvfb, x11vnc, rviz2, amazon-warehouse, follow-line, macos]
published: true
---

During **Week 10**, we focused on executing and validating tasks locally on macOS without relying on remote servers. We conducted tests on 3D GUI rendering over VNC, validated the onboard camera feed for the FollowLine exercise, and expanded the **ROS 2 Nav2** integration to the **Amazon Warehouse** challenge.

---

## 1. 3D GUI & VNC Testing on macOS Docker

To evaluate graphical rendering on macOS Apple Silicon, we tested an isolated Xvfb virtual framebuffer and x11vnc server inside Docker.

### A. RViz2 3D OpenGL Rendering (Passed)
* Successfully launched RViz2 inside the container and connected via VNC.
* The 3D grid and interactive coordinate displays rendered smoothly at a steady **31 FPS** under software OpenGL rasterization.

<div align="center" style="margin: 20px 0;">
  <img src="/2026-internship-Devesh_Mishra/assets/img/posts/task33_xvfb_gui.png" alt="RViz2 3D Rendering at 31 FPS in Xvfb over VNC" style="width: 85%; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);" />
  <p style="margin-top: 8px; font-size: 0.9em; color: #555;">Figure 1: RViz2 3D grid and coordinate display rendering smoothly at 31 FPS over VNC.</p>
</div>

### B. Gazebo Standalone GUI (Black Viewport)
* Tested the standalone Gazebo GUI viewer inside the same VNC environment.
* The application window and menus loaded, but the 3D scene viewport remained black due to QtQuick/QML shader compositing limitations under Rosetta 2 software OpenGL.

<div align="center" style="margin: 20px 0;">
  <img src="/2026-internship-Devesh_Mishra/assets/img/posts/gazebo_shapes_xvfb.png" alt="Gazebo GUI Viewport under Software OpenGL" style="width: 85%; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);" />
  <p style="margin-top: 8px; font-size: 0.9em; color: #555;">Figure 2: Gazebo interface window opening with an uncomposited 3D viewport under software rasterization.</p>
</div>

---

## 2. FollowLine Onboard Camera Streaming

We verified that while the Gazebo GUI viewport requires hardware-level shaders, the **headless Gazebo simulation engine generates sensor frames reliably on macOS**.

* **Live Camera Stream Extraction:** Subscribed directly to the F1 car onboard camera topic to extract real-time frames ($640 \times 480$ RGB).
* **WebGUI Image Delivery:** Streamlined WebSocket image delivery in the web interface to display live camera feeds without latency.

<div align="center" style="margin: 20px 0;">
  <img src="/2026-internship-Devesh_Mishra/assets/img/posts/follow_line_gazebo_camera.png" alt="FollowLine Onboard Camera Feed from Gazebo" style="width: 75%; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);" />
  <p style="margin-top: 8px; font-size: 0.9em; color: #555;">Figure 3: Live F1 onboard camera frame captured from headless Gazebo simulation showing the red racing line and start grid.</p>
</div>

---

## 3. Nav2 Integration for Amazon Warehouse

Following our work on the City Navigation challenge, we adapted the **Nav2 Navigation Stack** for the **Amazon Warehouse** exercise:

* **Custom Costmaps & Footprint:** Configured local and global costmaps with obstacle inflation tailored for the Amazon Kiva AGV platform.
* **Warehouse Map & Lifecycle Management:** Set up the Nav2 map server to load the warehouse layout and manage node lifecycle transitions.
* **Navigation Bridge:** Created a bridge node to handle goal dispatch and return planned paths to the web interface.

---

## 4. Key Conclusions

* **Recommended macOS Workflow:** Running Gazebo in headless mode alongside direct ROS 2 topic visualization (RViz2 or WebGUI canvas) provides a stable and performant simulation environment on macOS Apple Silicon.
* **Standardized Nav2 Pipeline:** Both City Navigation and Amazon Warehouse exercises can operate reliably using standard Nav2 launch configurations.
