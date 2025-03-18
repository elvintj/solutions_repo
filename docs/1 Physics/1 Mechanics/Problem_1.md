1. Theoretical Foundation
To derive the governing equations, start with Newton’s second law:

The horizontal and vertical motion can be treated separately.
The acceleration due to gravity acts only in the vertical direction.
The motion equations:

𝑥(𝑡)=𝑣0cos(𝜃)𝑡

𝑦(𝑡)=𝑣0sin(𝜃)𝑡−1/2𝑔𝑡^2

 
Solving for the time of flight (𝑇) when 𝑦(𝑇)=0

𝑇=2v0sin(𝜃)/g


The range R is given by:

𝑅=𝑣0cos(𝜃)⋅𝑇 = 𝑣02sin(2𝜃)/g

​
This equation shows that the range depends on:

Initial velocity 𝑣0
Gravity 𝑔
Angle θ


2. Analysis of the Range
The range is maximized when 𝜃 = 45∘
Increasing 𝑣0 increases the range quadratically.
Lower gravity (e.g., on the Moon) increases the range.
You can explore how changing these parameters affects 𝑅


3. Practical Applications
Sports: Optimizing throwing angles.
Engineering: Projectile launches in varying gravity.
Realistic factors like air resistance can reduce the range, requiring numerical methods for a solution.
 
4. Implementation in Python

import numpy as np
import matplotlib.pyplot as plt

def compute_range(v0, g, angles):
    """Computes the range of a projectile for different angles."""
    ranges = (v0**2 * np.sin(2 * np.radians(angles))) / g
    return ranges

# Parameters
v0 = 20  # Initial velocity (m/s)
g = 9.81  # Acceleration due to gravity (m/s^2)
angles = np.linspace(0, 90, 100)  # Angle range from 0 to 90 degrees

# Compute ranges
ranges = compute_range(v0, g, angles)

# Plot results
plt.figure(figsize=(8, 5))
plt.plot(angles, ranges, label=f'v0 = {v0} m/s')
plt.xlabel("Launch Angle (degrees)")
plt.ylabel("Range (m)")
plt.title("Projectile Range vs Launch Angle")
plt.legend()
plt.grid()
plt.show()
