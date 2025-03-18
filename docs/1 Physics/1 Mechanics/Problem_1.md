# Problem 1

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


