# Problem 1

## Investigating the Range as a Function of the Angle of Projection

## Introduction
Projectile motion describes the movement of an object under the influence of gravity after an initial launch. The horizontal distance traveled by a projectile, known as its range, depends on several factors, including the initial velocity and angle of projection. This investigation explores the relationship between the launch angle and the range of a projectile.

## Theory
Projectile motion can be analyzed using kinematic equations, considering separate horizontal and vertical components:

- **Horizontal motion:**
  $$ x = v_0 \cos(\theta) \cdot t $$
  
- **Vertical motion:**
  $$ y = v_0 \sin(\theta) \cdot t - \frac{1}{2} g t^2 $$
  
where:
- \( x \) and \( y \) are the horizontal and vertical displacements,
- \( v_0 \) is the initial velocity,
- \( \theta \) is the launch angle,
- \( g \) is the gravitational acceleration (9.81 m/s²),
- \( t \) is the time of flight.

### Time of Flight
The total time a projectile remains in motion before hitting the ground can be found by setting \( y = 0 \):
  $$ T = \frac{2 v_0 \sin(\theta)}{g} $$

### Range of the Projectile
The range \( R \) is the total horizontal distance covered:
  $$ R = v_0 \cos(\theta) \cdot T $$
  
Substituting \( T \):
  $$ R = \frac{v_0^2 \sin(2\theta)}{g} $$

From this equation, the maximum range is achieved when \( \sin(2\theta) \) is maximized, which occurs at \( \theta = 45^\circ \).

## Implementation
A Python script can be used to simulate projectile motion and visualize how the range varies with the launch angle.

```python
import numpy as np
import matplotlib.pyplot as plt

def compute_range(v_0, g, angles):
    """Computes the range of a projectile for different launch angles."""
    ranges = (v_0**2 * np.sin(2 * np.radians(angles))) / g
    return ranges

# Parameters
v_0 = 25  # Initial velocity (m/s)
g = 9.81  # Acceleration due to gravity (m/s²)
angles = np.linspace(0, 90, 100)  # Angles from 0 to 90 degrees

# Compute ranges
ranges = compute_range(v_0, g, angles)

# Plot results
plt.figure(figsize=(8, 5))
plt.plot(angles, ranges, label=f'v_0 = {v_0} m/s')
plt.xlabel("Launch Angle (degrees)")
plt.ylabel("Range (m)")
plt.title("Projectile Range vs Launch Angle")
plt.legend()
plt.grid()
plt.show()
```


**Sample Trajectories of Projectile Motion**

![Graphical Representation: Projectile Motion](/Users/elvintahmaz/Downloads/Graph01.png)
