# Problem 3: Trajectories of a Freely Released Payload Near Earth

## Motivation:
When an object is released from a moving rocket near Earth, its trajectory is determined by its initial conditions (position, velocity, and altitude) and the gravitational forces acting on it. The study of these trajectories is crucial for various space mission scenarios such as deploying payloads, orbital insertion, reentry, and escape.

## Task:
1. **Analyze Possible Trajectories:**
   - Investigate the potential types of trajectories, including parabolic, hyperbolic, and elliptical orbits, based on the initial conditions of the payload (position, velocity, altitude).
   - Understand how these trajectories relate to real-world space scenarios such as orbital insertion, reentry, or escape from Earth's gravity.

2. **Numerical Analysis:**
   - Use numerical methods to compute and simulate the path of the payload.
   - Account for Earth's gravitational force on the payload, and determine how the velocity and position evolve over time.

3. **Simulation and Visualization:**
   - Develop a Python-based computational tool to simulate and visualize the motion of the payload under the influence of Earth's gravity.
   - The tool should take initial velocity, position, and direction as input and simulate the payload's trajectory.
   
4. **Discuss Real-World Applications:**
   - Explore how these trajectories are relevant to space mission planning, satellite deployment, and planetary exploration.
   - Provide insights on how to predict payload trajectories for various mission scenarios.

## Theory:

### Gravitational Principles:
- **Newton’s Law of Gravitation:**  
  The gravitational force acting on an object with mass `m` near Earth is given by:

  \[
  F = \frac{G M_e m}{r^2}
  \]

  Where:
  - \( F \) is the gravitational force,
  - \( G \) is the gravitational constant (\( 6.674 \times 10^{-11} \, \text{N} \cdot \text{m}^2 / \text{kg}^2 \)),
  - \( M_e \) is Earth's mass (\( 5.972 \times 10^{24} \, \text{kg} \)),
  - \( r \) is the distance from the center of the Earth.

- **Kepler's Laws:**  
  These laws describe the motion of planets (and payloads in orbit) under the influence of gravity:
  1. **Elliptical Orbits:** Planets (or objects in space) move in elliptical orbits with the Sun (or Earth, in our case) at one focus.
  2. **Equal Areas in Equal Times:** A line connecting a planet to the Sun sweeps out equal areas in equal times.
  3. **Harmonic Law:** The square of the orbital period of a planet is proportional to the cube of the semi-major axis of its orbit.

### Trajectory Types:
- **Parabolic Trajectory:** An object that has exactly the escape velocity will follow a parabolic trajectory.
- **Elliptical Trajectory:** An object in orbit will follow an elliptical path, which can be bound or unbound depending on the velocity and altitude.
- **Hyperbolic Trajectory:** If the object has more than the escape velocity, it will follow a hyperbolic trajectory and escape Earth’s gravity.

### Escape Velocity:
The escape velocity \( v_e \) is the minimum velocity needed for an object to escape Earth's gravitational influence, calculated as:

\[
v_e = \sqrt{\frac{2 G M_e}{r}}
\]

Where:
- \( v_e \) is the escape velocity,
- \( r \) is the distance from the center of the Earth.

## Python Code: Numerical Simulation

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
G = 6.67430e-11  # Gravitational constant (m^3 kg^-1 s^-2)
M_e = 5.972e24    # Earth's mass (kg)
R_e = 6371e3      # Earth's radius (m)

# Initial conditions
initial_position = np.array([7000e3, 0])  # Position (m) [x, y]
initial_velocity = np.array([0, 10e3])    # Velocity (m/s) [vx, vy]

# Time settings
dt = 10  # Time step (seconds)
T = 10000  # Total time for simulation (seconds)
times = np.arange(0, T, dt)

# Initialize arrays for position and velocity
positions = []
velocities = []
position = initial_position
velocity = initial_velocity

# Function to compute the gravitational force
def gravitational_force(position):
    r = np.linalg.norm(position)
    force_magnitude = G * M_e / r**2
    force_direction = -position / r  # Unit vector pointing towards Earth's center
    return force_magnitude * force_direction

# Numerical integration (Euler's method)
for t in times:
    positions.append(position)
    velocities.append(velocity)
    
    # Compute acceleration
    force = gravitational_force(position)
    acceleration = force / 1  # Assuming mass = 1 kg for simplicity
    
    # Update velocity and position using Euler's method
    velocity += acceleration * dt
    position += velocity * dt

# Convert positions and velocities to arrays
positions = np.array(positions)

# Plot the trajectory
plt.figure(figsize=(6, 6))
plt.plot(positions[:, 0], positions[:, 1], label='Trajectory')
plt.scatter(0, 0, color='red', label='Earth')
plt.title('Trajectory of a Freely Released Payload Near Earth')
plt.xlabel('X Position (m)')
plt.ylabel('Y Position (m)')
plt.legend()
plt.grid(True)
plt.axis('equal')
plt.show()
```

**Graphical representation of orbital and escape velocities for Earth, Mars, and Jupiter**

![orbital trajectories, escape velocities, and payload trajectories near Earth.](/Users/elvintahmaz/Downloads/Graph5, /Users/elvintahmaz/Downloads/Graph6)