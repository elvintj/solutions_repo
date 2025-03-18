# Problem 2

## Investigating the Dynamics of a Forced Damped Pendulum

## Introduction
The forced damped pendulum is a fascinating physical system where the interplay of damping, restoring forces, and external periodic driving results in a wide range of complex behaviors. These include resonance, quasiperiodicity, and even chaos. Understanding these behaviors is crucial for applications in engineering, mechanical resonance systems, and nonlinear dynamics.

## Theory
The motion of a forced damped pendulum is governed by the nonlinear differential equation:

$$ \frac{d^2\theta}{dt^2} + \gamma \frac{d\theta}{dt} + \omega_0^2 \sin(\theta) = A \cos(\omega t) $$

where:
- \( \theta \) is the angular displacement,
- \( \gamma \) is the damping coefficient,
- \( \omega_0 \) is the natural frequency of the pendulum,
- \( A \) is the amplitude of the external forcing,
- \( \omega \) is the frequency of the external forcing,
- \( t \) represents time.

### Small-Angle Approximation
For small oscillations (i.e., \( \theta \ll 1 \)), we approximate \( \sin(\theta) \approx \theta \), leading to the linearized equation:

$$ \frac{d^2\theta}{dt^2} + \gamma \frac{d\theta}{dt} + \omega_0^2 \theta = A \cos(\omega t) $$

The general solution consists of a transient part and a steady-state solution:

$$ \theta(t) = \theta_0 e^{-\gamma t} + C \cos(\omega t - \phi) $$

where \( \phi \) is the phase shift.

### Resonance Conditions
Resonance occurs when the external forcing frequency \( \omega \) is close to the natural frequency \( \omega_0 \), amplifying oscillations significantly. The amplitude of the steady-state oscillation is given by:

$$ \theta_{max} = \frac{A}{\sqrt{(\omega_0^2 - \omega^2)^2 + (\gamma \omega)^2}} $$

## Implementation
A computational approach helps visualize the impact of damping, external forcing, and initial conditions.

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# Define parameters
gamma = 0.1  # Damping coefficient
omega_0 = 1.0  # Natural frequency
A = 0.5  # Forcing amplitude
omega = 1.2  # Driving frequency

def pendulum_eq(t, y):
    theta, omega_t = y
    dtheta_dt = omega_t
    domega_dt = -gamma * omega_t - omega_0**2 * np.sin(theta) + A * np.cos(omega * t)
    return [dtheta_dt, domega_dt]

# Initial conditions
y0 = [0.1, 0]  # Small initial angle, zero initial velocity

# Time range
tspan = (0, 50)
t_eval = np.linspace(*tspan, 1000)

# Solving the system
sol = solve_ivp(pendulum_eq, tspan, y0, t_eval=t_eval)

# Plotting results
plt.figure(figsize=(8,5))
plt.plot(sol.t, sol.y[0], label='Angle θ(t)')
plt.xlabel('Time (s)')
plt.ylabel('Angular Displacement (rad)')
plt.title('Forced Damped Pendulum Motion')
plt.legend()
plt.grid()
plt.show()
```
---

**Simulated Angular Displacement Over Time**

![Graphical Representation: Pendulum Motion](./_pics/Graph2.png)

