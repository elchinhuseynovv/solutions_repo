# Problem 2

## Investigating the Dynamics of a Forced Damped Pendulum  

---

## 1. Theoretical Foundation  

### Governing Equation of the System:  

The motion of a forced damped pendulum is described by the second-order nonlinear ordinary differential equation:

$$
 \frac{d^2\theta}{dt^2} + b\frac{d\theta}{dt} + \frac{g}{L}\sin\theta = A\cos(\omega t)
$$

Where:

- $\theta(t)$ — Angular displacement of the pendulum  
- $b$ — Damping coefficient representing energy dissipation (friction, air resistance)  
- $g$ — Acceleration due to gravity  
- $L$ — Length of the pendulum  
- $A$ — Amplitude of the external driving force  
- $\omega$ — Angular frequency of the external periodic driving force  

---

### Small Angle Approximation:  

For small oscillations ($\theta \approx 0$), we can linearize the system using $\sin\theta \approx \theta$. This gives a linear differential equation:

$$
 \frac{d^2\theta}{dt^2} + b\frac{d\theta}{dt} + \frac{g}{L}\theta = A\cos(\omega t)
$$

This linearized form allows us to analytically discuss resonance and energy behavior in the system.

---

### Resonance Condition:  

Resonance occurs when the driving frequency matches the natural frequency of the pendulum. The natural frequency is given by:

$$
 \omega_0 = \sqrt{\frac{g}{L}}
$$

At resonance ($\omega \approx \omega_0$), the system absorbs maximum energy from the driving force, leading to large amplitude oscillations — this is a critical phenomenon in both physics and engineering.

---

## 2. Analysis of System Dynamics  

The dynamics of the forced damped pendulum strongly depend on the following key parameters:

| Parameter | Influence on the System Behavior |  
|-----------|-----------------------------------|  
| Damping Coefficient ($b$) | Controls energy dissipation. Large $b$ leads to overdamping (motion dies out quickly), small $b$ allows sustained oscillations or even chaos.  
| Driving Amplitude ($A$) | Higher values of $A$ can lead to stronger oscillations, possible chaotic behavior, and sensitivity to initial conditions.  
| Driving Frequency ($\omega$) | Near resonance, oscillations are amplified. Far from resonance, oscillations remain limited.  

---

### Types of Motion Observed:

- Periodic Motion — Regular oscillations repeating in time  
- Quasiperiodic Motion — Oscillations with two or more frequencies  
- Chaotic Motion — Highly sensitive to initial conditions; non-repeating, complex trajectories  

---

## 3. Practical Applications  

The forced damped pendulum model applies to many real-world systems:

| Field | Real-World Example |  
|-------|---------------------|  
| Engineering | Suspension bridges, where resonance from wind or traffic must be avoided (Tacoma Narrows Bridge collapse).  
| Energy Harvesting | Devices that capture oscillatory energy from mechanical vibrations (self-powered sensors).  
| Clocks | Analysis of pendulum motion in clock mechanisms.  
| Electrical Circuits | Driven RLC circuits are mathematically equivalent to a forced damped pendulum.  
| Biomechanics | Studying human gait patterns and walking dynamics.  

---

## 4. Python Implementation for Simulation  

### Description:  

We simulate the system using the `solve_ivp` function from SciPy, varying the damping coefficient $b$, driving amplitude $A$, and driving frequency $\omega$ to observe different behaviors of the pendulum.

import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp
# Constants
g = 9.81  # gravitational acceleration (m/s^2)
# Pendulum differential equation
def forced_damped_pendulum(t, y, b, A, omega, L):
    theta, omega_ = y
    dydt = [omega_, -b * omega_ - (g / L) * np.sin(theta) + A * np.cos(omega * t)]
    return dydt
# Define different scenarios to study dynamics
simulations = [
    {"b": 0.2, "A": 1.2, "omega": 2.0, "L": 1.0, "label": "Underdamped"},
    {"b": 1.5, "A": 1.2, "omega": 2.0, "L": 1.0, "label": "Overdamped"},
    {"b": 0.5, "A": 2.0, "omega": 2.0, "L": 1.0, "label": "High Driving Force"},
    {"b": 0.5, "A": 1.2, "omega": 3.0, "L": 1.0, "label": "Higher Frequency"},
]

t_span = (0, 50)
t_eval = np.linspace(*t_span, 5000)
y0 = [0.1, 0.0]

results = []

for sim in simulations:
    sol = solve_ivp(forced_damped_pendulum, t_span, y0, t_eval=t_eval,
                    args=(sim["b"], sim["A"], sim["omega"], sim["L"]))
    results.append((sim["label"], sol.t, sol.y[0], sol.y[1]))
# Plot Results
fig, axs = plt.subplots(len(results), 2, figsize=(12, 10))

for i, (label, t, theta, omega_) in enumerate(results):
    axs[i, 0].plot(t, theta)
    axs[i, 0].set_title(f"{label}: θ(t)")
    axs[i, 0].set_xlabel("Time (s)")
    axs[i, 0].set_ylabel("Angular Displacement (rad)")
    axs[i, 0].grid(True)

    axs[i, 1].plot(theta, omega_, alpha=0.8)
    axs[i, 1].set_title(f"{label}: Phase Portrait")
    axs[i, 1].set_xlabel("θ (rad)")
    axs[i, 1].set_ylabel("ω (rad/s)")
    axs[i, 1].grid(True)
plt.tight_layout()
plt.show()





---

## 5. Discussion & Limitations  

### Limitations of the Current Model:

- Small-angle approximation is only valid for small displacements.
- Real-world frictions (nonlinear damping) are not considered.
- External forces are assumed to be purely sinusoidal and continuous.
- Numerical simulations rely on specific methods — accuracy depends on step size and method used.

---

### Possible Extensions and Future Work:

- Introduce nonlinear damping (air resistance quadratic in velocity).  
- Explore non-periodic driving forces (random noise, pulses).  
- Generate Poincaré sections for a more detailed analysis of chaotic motion.  
- Produce bifurcation diagrams to analyze transitions from regular to chaotic behavior.  
- Investigate energy transfer in the system over time.

---

## Conclusion  

This study of the forced damped pendulum reveals a fascinating range of physical behaviors depending on system parameters. We have demonstrated:

- Stable periodic motion under light forcing and damping.  
- Resonance effects when driving frequency matches the natural frequency.  
- The onset of chaotic behavior under certain conditions — a hallmark of nonlinear dynamics.  

The forced damped pendulum is not just a theoretical model but a cornerstone for understanding oscillatory phenomena in nature and technology. The insights gained from this model are widely applicable in physics, engineering, and beyond.