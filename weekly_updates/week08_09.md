# Weekly Update - Weeks 8 & 9

## Overview
Following an approved leave in Week 8, this combined report covers the progress made across **Weeks 8 and 9**. Our primary milestone was upgrading and standardizing the **JdeRobot RoboticsAcademy** navigation challenges using the **ROS 2 Humble Nav2 (Navigation 2) Stack**.

---

## Tasks Completed

### 1. City Navigation: Full Nav2 Multi-Layered Architecture
* **Global Path Planning (Navfn A*):** Configured collision-free A* routing on the official $500\\text{ m} \\times 500\\text{ m}$ City Map ($1.25\\text{ m/px}$).
* **Dynamic Rolling Local Costmap:** Implemented a $35\\text{ m} \\times 35\\text{ m}$ rolling window with obstacle inflation layers around road curbs and building boundaries.
* **3D Yellow Taxi Model & Sensors:** Integrated the $12\\text{ m} \\times 5\\text{ m}$ Yellow Taxi URDF, matching collision footprint, and 360-degree LiDAR raycasting.
* **Local Controller Steering Arc:** Configured dynamic lookahead trajectory visualization (Regulated Pure Pursuit).
* **Interactive 2D Goal Navigation:** Starts stationary, navigates on goal dispatch, and executes automatic complete stop upon destination arrival.

#### Video Demonstrations:
<p align="center">
  <a href="https://www.youtube.com/watch?v=r2CDi-2TRMI">
    <img src="https://img.youtube.com/vi/r2CDi-2TRMI/maxresdefault.jpg" alt="Initial Overview Demo" width="90%" style="border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.2);">
  </a>
</p>

<p align="center">
  <a href="https://www.youtube.com/watch?v=7ram5c_WYBI">
    <img src="https://img.youtube.com/vi/7ram5c_WYBI/maxresdefault.jpg" alt="Comprehensive Nav2 Multi-Layer Feature Walkthrough" width="90%" style="border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.2);">
  </a>
</p>

---

### 2. Community Review & Mentor Validation
* Presented technical context and video demonstrations on `#roboticsacademy` Slack channel.
* Clarified architecture details (offline RI launchers, costmap generation, lookahead arcs) with lead mentor Jose Sir (`jmplaza`).

---

### 3. Amazon Warehouse: Nav2 Integration (In Progress)
* Work is currently in progress to port and set up the Nav2 navigation stack for the Amazon Warehouse exercise.

---

## Technical Challenges & Fixes
* **RViz2 Flickering:** Eliminated zombie processes and applied 3D vertical layer stacking ($Z$-offsets) to prevent GPU Z-fighting.
* **LaserScan Point Persistence:** Applied $0.2\\text{ s} - 0.3\\text{ s}$ decay buffer for continuous laser beam rendering.

---

## Next Steps
* **Amazon Warehouse Nav2 Integration:** Complete and test the Nav2 pipeline for the Amazon Warehouse exercise.
* **RoboticsAcademy Package Integration:** Standardize launch scripts and configurations for upstream `RoboticsInfrastructure`.
* **Web-GUI Bridge Testing:** Verify end-to-end communication with the RoboticsAcademy browser frontend interface (`/webgui/current_target`).
