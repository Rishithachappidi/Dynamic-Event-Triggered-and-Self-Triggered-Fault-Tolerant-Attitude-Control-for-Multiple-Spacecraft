# Simulation Source

This folder contains the MATLAB/Simulink implementation used for the SFF project.

## Main execution

```matlab
R = sff_run_all;
```

`R` contains the time histories and derived quantities for the three controller cases.

## Control/data flow

```text
Clock + Case selector
        ↓
ControlAndTrigger
        ↓
Actuator saturation
        ↓
Spacecraft dynamics
        ↓
24-state integrator
        ↺ state feedback
```

The 24-state vector is arranged as:

```text
[σ₁; ω₁; σ₂; ω₂; σ₃; ω₃; σ₄; ω₄]
```

The figure export path in `sff_run_all.m` has been changed only for repository organization: all generated images are written to `../output_images/`.

Generated `slprj/` and `.slxc` cache files are intentionally excluded from this repository package.
