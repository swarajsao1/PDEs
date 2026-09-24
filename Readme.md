# Partial Differential Equations and Numerical Methods

## 1. ODEs vs PDEs

### Ordinary Differential Equations (ODEs)

An **Ordinary Differential Equation (ODE)** involves derivatives with respect to a **single independent variable**.

For example,

$$
\frac{dy}{dx} = f(x,y)
$$

Here:

- $x$ is the independent variable.
- $y(x)$ is the dependent variable.
- The solution is generally a **curve** $y(x)$.

Another example is the equation of a simple harmonic oscillator:

$$
\frac{d^2x}{dt^2} + \omega^2 x = 0
$$

The solution is a function of one variable:

$$
x = x(t)
$$


### Partial Differential Equations (PDEs)

A **Partial Differential Equation (PDE)** involves derivatives with respect to **two or more independent variables**.

For example,

$$
\frac{\partial u}{\partial t} =
\alpha
\frac{\partial^2 u}{\partial x^2}
$$

Here:

- $x$ and $t$ are independent variables.
- $u(x,t)$ is the dependent variable.
- The solution is a **field** defined over space and time.

For example,

$$
u = u(x,t)
$$

can represent temperature at position $x$ and time $t$.


### Main Difference

| ODE | PDE |
|---|---|
| One independent variable | Two or more independent variables |
| Uses ordinary derivatives | Uses partial derivatives |
| Solution is usually a curve | Solution is usually a field |
| Example: $y(x)$ | Example: $u(x,t)$ |
| $\frac{dy}{dx}$ | $\frac{\partial u}{\partial x}$ |


---

# 2. Solving PDEs Numerically

Analytical solutions of PDEs are available only for certain simple problems.

For more complicated problems, we often solve PDEs **numerically**.

Some common numerical methods are:

1. **Finite Difference Method (FDM)**
2. **Finite Element Method (FEM)**
3. **Finite Volume Method (FVM)**
4. **Spectral Methods**
5. **Particle-based methods**

Here we will start with the **Finite Difference Method**.


# 3. Finite Difference Method

The basic idea of the Finite Difference Method is to replace derivatives by **finite differences**.

For example,

$$
\frac{du}{dx}
\approx
\frac{u_{i+1}-u_i}{\Delta x}
$$

Instead of solving for a continuous function $u(x)$ directly, we calculate its value at a finite number of grid points.

Consider the grid

$$
x_0,\ x_1,\ x_2,\ldots,x_N
$$

with uniform spacing

$$
\Delta x = x_{i+1}-x_i.
$$

We write

$$
u_i = u(x_i).
$$


## 3.1 First Derivative

Using Taylor expansion,

$$
u(x+\Delta x) =
u(x)
+
\Delta x\frac{du}{dx}
+
\frac{\Delta x^2}{2}\frac{d^2u}{dx^2}
+\cdots
$$

Therefore,

$$
\boxed{
\frac{du}{dx}
\approx
\frac{u_{i+1}-u_i}{\Delta x}
}
$$

This is called the **forward difference**.


Similarly,

$$
\boxed{
\frac{du}{dx}
\approx
\frac{u_i-u_{i-1}}{\Delta x}
}
$$

is the **backward difference**.

A more accurate approximation is the central difference:

$$
\boxed{
\frac{du}{dx}
\approx
\frac{u_{i+1}-u_{i-1}}{2\Delta x}
}
$$


## 3.2 Second Derivative

The second derivative can be approximated using

$$
\boxed{
\frac{d^2u}{dx^2}
\approx
\frac{u_{i+1}-2u_i+u_{i-1}}
{\Delta x^2}
}
$$

This approximation will be particularly important for the heat equation.


---

# 4. Heat Equation

The **1D heat equation** is

$$
\boxed{
\frac{\partial T}{\partial t} =
\alpha
\frac{\partial^2 T}{\partial x^2}
}
$$

where:

- $T(x,t)$ = temperature
- $x$ = spatial coordinate
- $t$ = time
- $\alpha$ = thermal diffusivity

The equation describes how temperature evolves due to thermal diffusion.


## 4.1 Physical Interpretation

The equation

$$
\frac{\partial T}{\partial t} =
\alpha
\frac{\partial^2 T}{\partial x^2}
$$

contains two important quantities.

### Rate of change of temperature

$$
\frac{\partial T}{\partial t}
$$

describes how temperature changes with time.

### Spatial curvature of temperature

$$
\frac{\partial^2 T}{\partial x^2}
$$

describes the spatial curvature of the temperature profile.

Therefore,

$$
\text{Rate of temperature change}
\propto
\text{spatial curvature}.
$$


---

# 5. Discretizing the Heat Equation

We discretize both **space** and **time**.

For space,

$$
x_i = i\Delta x
$$

and for time,

$$
t^n = n\Delta t.
$$

We denote the numerical solution by

$$
T_i^n = T(x_i,t^n).
$$

Here:

- $i$ = spatial grid index
- $n$ = time-step index


## 5.1 Time Derivative

Using the forward difference,

$$
\frac{\partial T}{\partial t}
\approx
\frac{T_i^{n+1}-T_i^n}{\Delta t}.
$$


## 5.2 Spatial Second Derivative

Using the central difference,

$$
\frac{\partial^2 T}{\partial x^2}
\approx
\frac{ T_{i+1}^n - 2T_i^n + T_{i-1}^n}
{\Delta x^2}
$$


Substituting these into the heat equation,

$$
\frac{T_i^{n+1}-T_i^n}{\Delta t} =
\alpha
\frac{T_{i+1}^n - 2T_i^n + T_{i-1}^n}
{\Delta x^2}
$$


Rearranging,

$$
T_i^{n+1} =
T_i^n
+
\frac{\alpha\Delta t}{\Delta x^2}
\left(
   T_{i+1}^n - 2T_i^n + T_{i-1}^n
\right)
$$


Define

$$
r =
\frac{\alpha\Delta t}{\Delta x^2}.
$$

Then,

$$
\boxed{
T_i^{n+1} =
T_i^n
+
r
\left(
   T_{i+1}^n - 2T_i^n + T_{i-1}^n
\right)
}
$$

This is the **Forward-Time Central-Space (FTCS)** scheme for the 1D heat equation.


---

# 6. Physical Meaning of the Finite Difference Equation

The numerical equation

$$
T_i^{n+1} =
T_i^n
+
r
\left(
   T_{i+1}^n - 2T_i^n + T_{i-1}^n
\right)
$$

says that the temperature at a point at the next time step depends on:

- its current temperature $T_i^n$
- the temperature of its left neighbor $T_{i-1}^n$
- the temperature of its right neighbor $T_{i+1}^n$


The term

$$
T_{i+1}^n-2T_i^n + T_{i-1}^n
$$

measures the local curvature of the temperature profile.


---

# 7. Grid Representation

Consider a 1D domain

$$
0\leq x\leq L.
$$

Divide it into $N$ spatial intervals.

Then,

$$
\Delta x = \frac{L}{N}.
$$

The grid looks like:

```text
│
●────●────●────●────●────●──→ x
0    1    2    3    4    5
        Δx
```


```text
                 t
                 ↑
                 │
        t₃       ●──────●──────●──────●──────●
              Δt │      │      │      │      │
        t₂       ●──────●──────●──────●──────●
                 │      │      │      │      │
        t₁       ●──────●──────●──────●──────●
                 │      │      │      │      │
        t₀       ●──────●──────●──────●──────●──→ x
                 x₀     x₁     x₂     x₃     x₄
                           Δx
```