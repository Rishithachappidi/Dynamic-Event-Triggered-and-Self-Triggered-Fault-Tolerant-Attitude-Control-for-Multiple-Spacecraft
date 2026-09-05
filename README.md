# SFF Control Lab

**Interactive visualization and MATLAB/Simulink reproduction of dynamic event-triggered and self-triggered fault-tolerant attitude control for a four-spacecraft formation.**

[![MATLAB](https://img.shields.io/badge/MATLAB-Simulink-orange)](#matlabsimulink-simulation)
[![Website](https://img.shields.io/badge/Website-GitHub%20Pages-blue)](#interactive-visualization)

> **Project status:** academic reproduction / visualization. The control strategies are based on the reference IEEE paper by Xie, Sheng, and Chen (2024); this repository does not claim the algorithms as original contributions.

---

## Project idea

The project visualizes how four spacecraft that begin with different attitudes and angular velocities can coordinate their attitude motion while reducing unnecessary communication and computation.

The control loop represented in the project is:

```text
Different spacecraft states σᵢ, ωᵢ
              ↓
Neighbor / reference comparison
              ↓
Formation errors e₁ᵢ and e₂ᵢ
              ↓
sᵢ = e₂ᵢ + r e₁ᵢ
              ↓
Trigger decision
              ↓
Fault-tolerant controller uᵢ
              ↓
Input saturation + actuator effectiveness
              ↓
Spacecraft rotational dynamics
              ↓
New σᵢ, ωᵢ
              ↺
```

The desired behavior is that the attitude and angular-velocity errors become small while communication is performed according to the selected triggering strategy.

---

## Three control cases

### Case 1 — Time-driven control
The controller is updated on a fixed time schedule (10 Hz in the simulation setup).

### Case 2 — Dynamic event-triggered control
The controller/state broadcast occurs only when the event-trigger condition is satisfied. This demonstrates how communication can be reduced compared with fixed periodic updates.

### Case 3 — Self-triggered control
The next update time is predicted from information available at the current trigger. This removes the need to continuously monitor the event condition.

---

## Interactive visualization

The public website is the root file [`index.html`](index.html). It is intentionally a **visual explanation of how the control system works**, rather than an attempt to reproduce every numerical sample from MATLAB inside the browser.

The visualization shows:

- four spacecraft at different schematic formation positions;
- different initial attitude states and angular velocities;
- spacecraft body-axis rotation from the attitude state;
- directed communication links;
- trigger broadcasts;
- live attitude/angular-velocity errors;
- the coordination variable `s`;
- controller torque and saturation;
- disturbance and actuator-effectiveness effects;
- Case 1, Case 2, and Case 3 behavior;
- live convergence and trigger plots.

### Important visualization note

The reference paper models **attitude and angular velocity**, not translational/orbital position. Therefore, the spacecraft locations in the web scene are schematic positions chosen to make the four-spacecraft communication network understandable. Their attitude/orientation behavior is what represents the control problem.

To preview locally, double-click `index.html` or open it in a modern browser.

---

## MATLAB/Simulink simulation

All relevant source files are in [`simulation/`](simulation/).

| File | Purpose |
|---|---|
| `sff_params.m` | Paper parameters, topology, gains, inertia, actuator effectiveness, initial conditions, simulation settings |
| `sff_x0.m` | Packs the four spacecraft initial states into the 24-state Simulink vector |
| `sff_ref.m` | Desired attitude trajectory and desired angular velocity |
| `sff_controller_core.m` | Time-driven, dynamic event-triggered, and self-triggered control logic |
| `sff_plant_core.m` | Nonlinear spacecraft attitude/rotational dynamics, disturbances and actuator effectiveness |
| `sff_build_model.m` | Programmatically constructs the Simulink model |
| `sff_ets_model.slx` | Simulink block-diagram model |
| `sff_run_all.m` | Runs all three cases, creates figures, and prints comparison tables |

### Run the simulation

1. Open MATLAB.
2. Set the current folder to `simulation/`.
3. Run:

```matlab
R = sff_run_all;
```

To force a rebuild of the Simulink model:

```matlab
R = sff_run_all(true);
```

Generated figures are saved automatically to the repository-level [`output_images/`](output_images/) folder.

### Software required

- MATLAB
- Simulink

The repository does not require Python or Node.js for the simulation or the static web visualization.

---

## Output images

All reproduced output figures are kept **only in one dedicated folder**:

```text
output_images/
├── Fig02.png
├── Fig03.png
├── Fig04.png
├── Fig05.png
├── Fig06.png
├── Fig07.png
├── Fig08.png
├── Fig09.png
├── Fig10.png
├── Fig11.png
├── Fig12.png
├── Fig13.png
├── Fig14.png
└── Fig15.png
```

These files came from the supplied `SFF_EventTriggered_Simulink` project. Running `sff_run_all.m` will write new generated figures back into this same folder.

---

## Repository structure

```text
SFF_Control_Lab_GitHub_Ready/
│
├── index.html                       # GitHub Pages visualization
├── README.md                        # Main project documentation
├── .gitignore                       # MATLAB/Simulink cache exclusions
├── .nojekyll                        # Simple GitHub Pages publishing
│
├── simulation/                      # MATLAB + Simulink implementation
│   ├── sff_build_model.m
│   ├── sff_controller_core.m
│   ├── sff_ets_model.slx
│   ├── sff_params.m
│   ├── sff_plant_core.m
│   ├── sff_ref.m
│   ├── sff_run_all.m
│   └── sff_x0.m
│
├── output_images/                   # ALL project output figures
│   ├── Fig02.png
│   ├── ...
│   └── Fig15.png
│
├── reference/
│   ├── IEEE_REFERENCE_PAPER.pdf     # Local reference copy; ignored by git by default
│   └── REFERENCE.md                 # Citation and publication note
│
└── docs/
    ├── MATHEMATICAL_WORKFLOW.md
    ├── GITHUB_PAGES_SETUP.md
    ├── PUBLIC_RELEASE_CHECKLIST.md
    └── website_preview.png
```

Generated Simulink cache folders (`slprj/`) and `.slxc` files are intentionally not included because they are build artifacts and should not be committed to GitHub.

---

## Reference paper

The project is based on:

**X. Xie, T. Sheng, and X. Chen**, “Dynamic Event-Triggered and Self-Triggered Fault-Tolerant Attitude Control for Multiple Spacecraft Systems With Uncertainties and Input Saturation,” *IEEE Transactions on Aerospace and Electronic Systems*, vol. 60, no. 3, 2024. DOI: **10.1109/TAES.2024.3355381**.

See [`reference/REFERENCE.md`](reference/REFERENCE.md) for the citation and a note about the local PDF.

---

## Publish the visualization with GitHub Pages

After pushing the repository to GitHub:

1. Open the repository on GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select the `main` branch and `/ (root)` folder.
5. Save.
6. GitHub will provide a public URL similar to:

```text
https://USERNAME.github.io/REPOSITORY-NAME/
```

Full steps are in [`docs/GITHUB_PAGES_SETUP.md`](docs/GITHUB_PAGES_SETUP.md).

---

## Academic-use note

This repository is organized as a reproducible academic project and visualization. When presenting it, describe it as a **reproduction / implementation / visualization of the reference work** rather than as the original invention of the control strategy.

The browser visualization focuses on explaining the mechanism. MATLAB/Simulink remains the project implementation used for the simulation figures.
