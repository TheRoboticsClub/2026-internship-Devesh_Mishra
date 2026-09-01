---
title: "Internship Progress Weeks 8 & 9 (August 20 ~ September 02)"
date: 2026-09-01 08:00:00 +0530
categories: [Internship 2026, Progress]
tags: [internship, progress, week-8, week-9, nav2, ros2, humble, jderobot, city-navigation, amazon-warehouse, rviz2, local-costmap, global-planner, pure-pursuit]
published: true
---

Following an approved leave in Week 8, this combined report covers the progress made across **Weeks 8 and 9**. Our primary milestone was upgrading and standardizing the **JdeRobot RoboticsAcademy** navigation challenges using the **ROS 2 Humble Nav2 (Navigation 2) Stack**.

We completed the full multi-layered navigation architecture for the **City Navigation** exercise, validated the pipeline with lead mentor Jose Sir (`jmplaza`), and successfully ported the Nav2 navigation stack to the **Amazon Warehouse** exercise featuring the Orange Amazon Kiva AGV.

---

## 1. Video Demonstrations & Previews

### A. Initial Overview (City Map & Global Route Planning)
<div align="center" style="margin: 20px 0;">
  <iframe width="95%" height="480" src="https://www.youtube-nocookie.com/embed/r2CDi-2TRMI" title="Autonomous Global Navigation in ROS 2 Humble using Nav2 Stack & JdeRobot" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen style="border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);"></iframe>
</div>

### B. Comprehensive Multi-Layered Nav2 Feature Walkthrough
<div align="center" style="margin: 20px 0;">
  <iframe width="95%" height="480" src="https://www.youtube-nocookie.com/embed/7ram5c_WYBI" title="Nav2 Global & Local Path Planning in ROS 2 Humble (Full Pipeline)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen style="border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15);"></iframe>
</div>

---

## 2. City Navigation: Full Nav2 Multi-Layered Architecture

### A. Global Path Planning (Navfn A\*)
Configured `NavfnPlanner` on the official JdeRobot City Map ($500\\text{ m} \\times 500\\text{ m}$, resolution $1.25\\text{ m/px}$) to compute collision-free, optimal global paths through complex urban road corridors.

### B. Dynamic Rolling Local Costmap
Implemented a vehicle-centric rolling local costmap ($35\\text{ m} \\times 35\\text{ m}$) that continuously recalculates obstacle cost gradients and inflation layers around road curbs and building perimeters.

### C. Vehicle Geometry & Sensor Integration
* **3D Yellow Taxi URDF:** Rendered an authentic $12\\text{ m} \\times 5\\text{ m}$ Yellow Taxi model with dark glass cabin, roof sign, headlights, and 4 black wheels.
* **Collision Footprint:** Configured an Orange collision bounding polygon matching the taxi chassis dimensions.
* **360-Degree LiDAR Raycasting:** Real-time range sensor reflections detecting surrounding road boundaries.

### D. Local Trajectory Controller (Regulated Pure Pursuit)
Generated dynamic lookahead steering trajectories (Cyan path arc) corresponding to the local controller output, ensuring smooth cornering at tight intersections.

### E. Interactive Goal-Driven Navigation
The vehicle remains safely parked at its initial pose until an interactive destination is dispatched via `2D Goal Pose` (`/goal_pose`) in RViz2. It traverses the planned route and executes an automatic full stop upon arrival at the destination tolerance.

---

## 3. Community Review & Mentor Validation

* Presented the complete technical context and video demonstrations on the `#roboticsacademy` Slack channel.
* Clarified implementation details regarding offline `RoboticsInfrastructure` launcher compatibility, layered costmap generation, and local trajectory lookahead curves with lead mentor `jmplaza`.

---

## 4. Amazon Warehouse: Nav2 AGV Porting

Following the City Navigation milestone, we extended the Nav2 architecture to the **Amazon Warehouse** challenge:

* **Official Warehouse Map:** Loaded the authentic JdeRobot Amazon Warehouse map ($279 \\times 415$ grid, resolution $0.05\\text{ m/px}$, origin `[-6.93, -10.4, 0.0]`).
* **Orange Amazon Kiva AGV Model:** Deployed the authentic $0.65\\text{ m} \\times 0.52\\text{ m} \\times 0.18\\text{ m}$ Kiva AGV URDF model with top turntable lift plate and drive wheels.
* **Fine-Grained Narrow-Aisle Planning:** Configured a high-resolution rolling local costmap ($3.0\\text{ m} \\times 3.0\\text{ m}$) with obstacle inflation around storage racks and shelving to enable collision-free AGV navigation through narrow warehouse aisles.

---

## 5. Technical Challenges & Solutions

| Challenge | Root Cause | Solution |
| :--- | :--- | :--- |
| **Visual Flickering in RViz2** | Legacy background simulation processes and duplicate RViz instances were publishing conflicting transforms on `/tf` and `/scan`. | Terminated all orphan processes, implemented a unified single-process architecture, and applied 3D vertical layer stacking ($Z$-offsets) to prevent GPU depth-buffer collision (Z-fighting). |
| **LaserScan Beam Dropping** | Default decay time ($0\\text{ s}$) caused intermittent frame-rate dropouts over remote display sessions. | Added a $0.2\\text{ s} - 0.3\\text{ s}$ decay buffer to maintain continuous, solid LiDAR beam visualization. |
| **Resolution Mismatch Across Exercises** | Scale disparity between the City Map ($1.25\\text{ m/px}$) and Amazon Warehouse Map ($0.05\\text{ m/px}$). | Isolated exercise pipelines, ensuring each environment loads its dedicated YAML metadata and tailored local costmap window parameters. |

---

## 6. Next Steps

* **RoboticsAcademy Package Integration:** Standardize launch scripts and configurations for City Navigation and Amazon Warehouse to align with upstream `RoboticsInfrastructure` repository conventions.
* **Web-GUI Bridge Testing:** Verify end-to-end communication with the RoboticsAcademy browser frontend interface via `/webgui/current_target`.
