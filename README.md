# analise_redes
Trabalho sobre Tópicos em IA 3 - 2025.2

# documentation 

Documentation of AI Project – Machine Learning with PINN

       Topics:
       
    1. PINN ( a general explanation)
    2. creation of Data
    3. The Model
    4. The other Model
    5. Comperison and Conclusion 


2. creation of data

why RK4 and what is it for?

RK4 (Runge–Kutta 4th order) is one method for numerically solving differential equations (ODEs).
It approximates the solution step-by-step by evaluating the derivative (rate of change) several times per step and taking a weighted average.
So if you have an ODE:	\dot{y} = f(t, y)
RK4 gives a very accurate approximation of y(t) without needing the exact analytical solution.
Reason
Explanation
1. Accuracy
RK4 is 4th order accurate, meaning the local error per step scales as O(Δt5)O(\Delta t^5)O(Δt5) and the global error as O(Δt4)O(\Delta t^4)O(Δt4). That’s much more accurate than Euler’s method O(Δt2)O(\Delta t^2)O(Δt2).
2. Stability
It’s stable for many typical physics problems (like pendulum, projectile motion, oscillators). You can use relatively large time steps without the solution “blowing up.”
3. Simplicity
Easy to code from scratch — no complex adaptive schemes needed. That’s why it’s often used in teaching, research, and PINN data generation.
4. Deterministic / noise-free
You control the step size and precision — perfect for generating clean reference data to test learning methods.
5. Reproducibility
Always gives the same output for given parameters — ideal for machine learning experiments.
RK4 is the best choice:
    • It’s accurate, stable, and easy to implement.
    • Perfect for small to medium systems (pendulum, projectile, oscillator).
    • If your system is very stiff (e.g., heavy damping or strong nonlinearity), you could switch to: scipy.integrate.solve_ivp(method='Radau') or 'BDF' (implicit).
    • If you simulate energy-conserving systems (e.g., undamped pendulum, orbits): Try symplectic integrators (like Velocity Verlet) → they preserve total energy better over long time horizons.

A sketch of the ceation of the data:
    • simulate_...() function:
        ◦ set physical parameters and init. Conditions
        ◦ define derivatives() for RK4 at all steps
        ◦ calc derivations k_i to approximate best step:
          k2​ uses k1: “half-step based on the start slope”
          k3​ uses k2​ : “half-step based on a better slope”
          k4​ uses k3​ : “full-step based on our best midpoint slope”
        ◦ Mathematically, it can be shown that this formula reproduces the Taylor expansion up to terms of order (Δt)^4 exactly 
        ◦ save all steps (time, xs,ys) in a list
    • generate data with a loop of different parameter values, init conditions,  physical conditions and gaussian error.

Here is a paper which describes the method: https://pmc.ncbi.nlm.nih.gov/articles/PMC9215218/?utm_source
