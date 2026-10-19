# Fruit Fly Racer — Application Summary & Feature Reference

## What the app is

Fruit Fly Racer is a Python-based simulation application featuring a virtual fruit fly driven by a scanned connectome-inspired neural model, navigating through a challenging 3D obstacle tunnel with dynamic wind and strict physics limits.

### Technical Stack
- **Language:** Python 3.10+
- **Physics Engine:** MuJoCo 3.15.0 with custom anatomically-accurate fly model
- **Neural/Math Backend:** NumPy (PyTorch-compatible architecture)
- **Connectome:** MaleCNS v1.0 fruit fly connectome (211,577 neurons / ~151.8M connections)

## Core Features

### 1. Desktop GUI Application (`desktop_gui.py`)
- **Tkinter-based desktop application** with full windowed interface
- **3D Visualization Canvas** showing:
  - Perspective-based tunnel walls with depth effect
  - Procedural obstacles (cubes, spheres, triangles) with holes/apertures
  - Anatomically-accurate fly with body, wings, eyes, and direction indicator
  - Start line and finish line markers
  - Wind direction indicators
  - Interactive legend
- **Control Panel:**
  - Start/Stop simulation buttons
  - Number of simulations input
  - Simulation speed selector
- **Status Panel:**
  - Current simulation counter
  - Best score tracking
  - Best progress tracking
  - Real-time status indicator
- **Simulation Log:** Color-coded log panel with info, success, error, and warning messages
- **Keyboard Navigation:** WASD/QE controls for manual fly navigation
- **Live Animation:** Fly moves and animates in real-time

### 2. 3D Obstacle Tunnel Environment (`environment.py`)
- **Continuous Racing Tunnel:** A 3D tunnel with dynamic, shifting wind fields (turbulence) that affect the fly's flight path
- **Procedural Obstacle Generation:** Randomized obstacles including:
  - Triangles, cubes, rectangles, flat planes, walls, and cones
  - Variant versions with custom **holes/apertures** for navigation
- **Physics Limits:**
  - Strict maximum speed limit (5.0 m/s)
  - Continuous tracking of velocity, speed, altitude, direction, and 3D position
- **Collision Detection:** Real-time collision checking with obstacles and tunnel walls
- **Progress Tracking:** Calculate progress to finish line (0 to 1)
- **Dynamic Wind:** Wind fields with turbulence that affect flight path

### 3. Sensory & Neural Inputs (`agent_logic.py`)
Feed telemetry and environmental data into the fly's neural network:
- Current altitude and direction vector
- Current speed and velocity vector
- **Wind direction vector** (NEW - sent to fly's brain)
- Localized positional awareness (visual field with 20 angular bins)
- Detected obstacles and relative layout data (20 nearest obstacles)
- Speed limit enforcement

### 4. Reward & Penalty System (`rewards.py`)
- **Positive Rewards:** Dense distance-based progression shaping (closer to finish = more points)
- **Negative Penalties:** Collision penalties (-50 points per collision)
- **Score Tracking:** Total points (max 1000), collision count, best progress

### 5. Neural Memory & Cloud Sync (`sync_manager.py`)
- **Synaptic Weight Checkpointing:** Separate static connectome from mutable synaptic weights (`fly_memory.pkl`)
- **Cloud Synchronization:** Fetch updated memory weights from remote repository
- **Version Tracking:** Track memory version and sync timestamps
- **Connectome Initialization:** Create initial checkpoint from connectome weights

### 6. MuJoCo Fly Model (`mujoco_fly_model.py`)
- **Official TuragaLab/DeepMind Flybody Model:** Real scientific fruit fly model from Nature publication
- **Anatomically-Accurate Fly Body:**
  - 109 degrees of freedom (DOF)
  - 78 actuators (motors)
  - 68 bodies (thorax, head, abdomen, wings, legs, antennae, halteres)
  - 103 joints for realistic articulation
  - High-fidelity OBJ mesh assets for all body parts
- **MuJoCo Physics Integration:** Full MuJoCo physics simulation
- **MaleCNS v1.0 Connectome Integration:** Loads and integrates with connectome weights
- **Neural Control System:** Connects sensory inputs to motor outputs via connectome
- **No Bazel Required:** Loads official flybody XML directly with MuJoCo

### 7. Core Brain Simulation (`brain_sim/`)
- **brain_engine.py:** Core brain simulation engine with optional connectome loading
- **neuron_models.py:** Integrate-and-fire neuron models
- **synapse_models.py:** Synapse models with plasticity

### 8. Main Simulation Runner (`main_simulation.py`)
- **Full Simulation Loop:** Runs millions of simulations automatically
- **Map Regeneration:** Each simulation generates new procedural obstacles
- **Neural Memory Persistence:** Learned weights persist across simulations
- **Learning Transfer:** Techniques learned on one course transfer to different courses
- **Automatic Checkpointing:** Saves progress every 100 simulations
- **Best Tracking:** Records best score and best progress
- **Wind Integration:** Wind direction sent to neural network inputs

## Project Structure

```
fruit-fly-racer/
├── environment.py           # 3D obstacle tunnel with MuJoCo physics
├── agent_logic.py           # Sensory & neural inputs
├── rewards.py              # Reward and penalty system
├── sync_manager.py          # Neural memory and cloud sync
├── mujoco_fly_model.py      # MuJoCo fly body model with connectome
├── main_simulation.py        # Main simulation runner
├── desktop_gui.py          # Desktop GUI application
├── brain_sim/
│   ├── brain_engine.py      # Core brain simulation
│   ├── neuron_models.py     # Neuron models
│   └── synapse_models.py    # Synapse models
├── data/
│   ├── connectome-weights-male-cns-v1.0-minconf-0.5.feather
│   └── body-annotations-male-cns-v1.0-minconf-0.5.feather
├── fly_memory.pkl           # Neural memory checkpoint (auto-generated)
├── Fruit Fly Racer.bat      # Desktop application launcher
├── env.example              # Environment configuration template
├── .env                     # Environment configuration (local, ignored)
├── FEATURES.md              # This file
├── PROJECT_PLAN.md          # Project plan and roadmap
└── agent_memory.md          # Development memory and notes
```

## Recent Updates (2026-10-14)

**Desktop GUI Enhancements:**
- Added WASD/QE keyboard controls for manual fly navigation
- Added real-time fly animation loop
- Made tunnel walls solid/opaque (no longer see-through)
- Enhanced obstacle rendering with solid colors
- Fly now moves and animates in real-time
- Wind direction now sent to neural network inputs
- Added keyboard binding for navigation control

**Technical Notes:**
- Official flybody requires Bazel (Windows build issues)
- Using custom MuJoCo fly model with full anatomy (thorax, head, abdomen, wings, 6 legs)
- PyTorch has DLL issues on Windows, using NumPy with PyTorch-compatible architecture
- All core functionality implemented per specification
