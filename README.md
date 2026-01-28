# 🚦 Agent-Based Traffic Optimisation: Static vs. Dynamic Signal Control
### A "Digital Twin" Traffic Simulation of Leicester, UK built with NetLogo & GIS

![Project Status](https://img.shields.io/badge/Status-Completed-success) ![NetLogo](https://img.shields.io/badge/Built%20With-NetLogo%206.3-blue) ![License](https://img.shields.io/badge/License-MIT-green)

## 📖 Overview
This project is an **Agent-Based Model (ABM)** designed to simulate and optimise urban traffic flow in a high-density area of Leicester, England. By integrating real-world **GIS (Geographic Information System)** data, the simulation creates a realistic topology to test two distinct traffic control paradigms: **Static (Fixed-Time)** vs. **Dynamic (Sensor-Based)** signaling.

The core objective is to quantify the environmental impact of traffic logic. Unlike standard distance-based models, this project utilises a custom **Physics Engine** to calculate CO₂ emissions based on vehicle inertia ($a = v_t - v_{t-1}$), proving that "Green Waves" significantly reduce pollution by minimizing stop-and-go driving.

## 🌟 Key Features

### 🗺️ Realistic GIS Environment
* **Digital Twin Construction:** Imports `.shp` (Shapefiles) from **OpenStreetMap** and **bbike.org** to render real roads, buildings, waterways, and traffic signals.
* **Intelligent Map Healing:** Features custom algorithms ("Pothole Filler" and "Gap Healer") to automatically repair disconnected road segments and artifacts common in raw GIS data.

### 🧠 Autonomous Agent Logic
* **Sensor-Based Navigation:** Vehicles utilize a **40° Vision Cone** and Lidar-like raycasting to detect traffic lights and obstacles.
* **Robust Pathfinding:** Replaces expensive A* searches with a lightweight **"Greedy & Penalized Steering"** engine. Agents calculate local utility scores every tick to navigate complex topologies without gridlock.
* **Anti-Looping GPS:** Implementation of a "Stuck Timer" that triggers a destination recalculation if an agent fails to make progress toward its target.

### 🍃 Physics-Based Emissions Model
* **Inertia Tracking:** Calculates CO₂ not by distance, but by **net momentum change**.
* **State Differentiation:**
    * *Cruising:* Low emissions (Efficiency state).
    * *Idling:* High penalty (0.5/tick) for stopped cars.
    * *Acceleration:* Extreme penalty ($Accel \times 2.0$) for regaining momentum.

## 🚦 The Algorithms

| Mode | Description | Logic |
| :--- | :--- | :--- |
| **Static** | Baseline Control | Blindly switches phases on a fixed 30-tick cycle. Causes frequent "Cutoff Effects" where cars brake for empty junctions. |
| **Dynamic** | Smart Control | **Gap-Out:** Switches instantly to Red if the road is empty.<br>**Green Wave:** Extends Green time if a platoon is approaching to preserve momentum.<br>**Anti-Starvation:** Forces signal changes after max-wait times to prevent side-road blockage. |

## 📊 Results
The simulation was validated through a **longitudinal study (1 Month / 43,200 ticks)** using **14-Core Parallel Execution (BehaviorSpace)** to process 250 autonomous agents simultaneously.

| Metric | Improvement with Dynamic Mode |
| :--- | :--- |
| **Emissions** | **-5.3%** total CO₂ reduction (Greener) |
| **Speed** | **+1.6%** higher average network speed (Faster) |
| **Throughput** | **+1.2%** more completed trips (More Flow) |

*Dynamic control achieved a "Clean Sweep" victory across all metrics. The data proves that the "Stop-Start" nature of Static lights is the primary driver of urban traffic pollution.*

## 💻 Tech Stack
* **Language:** NetLogo 7.02
* **Data Sources:** OpenStreetMap (Overpass Turbo), BBBike.org
* **Optimization:** Spatial Hashing, Multi-Threaded BehaviorSpace
* **Analysis:** Python (Pandas/Matplotlib) for CSV post-processing

## 🚀 How to Run
1.  **Clone the repo:** `git clone https://github.com/TNZRalf/Static-vs-Dynamic-Leciester-traffic-light-simulation`
2.  **Open:** Launch **NetLogo 6.3** and open `Traffic_Simulation.nlogo`.
3.  **Setup:** Click `Setup` to load the GIS data and spawn agents.
4.  **Configure:** Select `Static` or `Dynamic` from the "Mode" chooser.
5.  **Run:** Click `Go` (Toggle "View Updates" off for high-speed simulation).

---
*Created by Zakaria Tanani | De Montfort University | 2026*
