# Mathematical Workflow Used in the Visualization

The website is designed to show the *mechanism* of the reference control system.

## 1. Spacecraft state

Each spacecraft has an attitude state `σᵢ` (Modified Rodrigues Parameters) and angular velocity `ωᵢ`.

The attitude kinematics are represented by:

```text
σ̇ᵢ = G(σᵢ) ωᵢ
```

The rotational dynamics are represented by:

```text
Jᵢ ω̇ᵢ = -ωᵢ× Jᵢ ωᵢ + Γᵢ sat(uᵢ) + ūᵢ + ρᵢ
```

## 2. Formation errors

Each spacecraft compares its state with the relevant neighbors and, where applicable, the desired reference state.

```text
e₁ᵢ = attitude coordination error
e₂ᵢ = angular-velocity coordination error
sᵢ  = e₂ᵢ + r e₁ᵢ
```

## 3. Triggering

### Time-driven
Updates occur periodically.

### Dynamic event-triggered
An update/broadcast occurs when the event condition becomes true.

```text
||s̃ᵢ||² - δᵢ ||sᵢ(tₖ)||² >= χᵢ + εᵢ
```

### Self-triggered
The next trigger time is predicted from information at the current trigger, so the event condition does not need to be checked continuously.

## 4. Fault-tolerant actuation

The commanded torque passes through input saturation and actuator-effectiveness terms before entering the spacecraft dynamics.

## 5. Desired result

```text
attitude error        → small
angular velocity error → small
coordination variable s → small
```

At the same time, event/self-triggering reduces unnecessary update/communication activity compared with fixed periodic operation.

## Visualization position note

The paper does not define translational/orbital position states for the four spacecraft. The website therefore places the four spacecraft at different **schematic** 3-D coordinates so that the formation graph and communication links can be seen. The controlled quantities being explained are attitude and angular velocity.
