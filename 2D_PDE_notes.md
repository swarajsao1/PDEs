# 2D Mesh representation

For a two-dimensional problem, the domain can be divided into a two-dimensional grid:

$$
(x_i,y_j).
$$

The solution is then represented as

$$
T_{i,j}=T(x_i,y_j).
$$

In 2D the mesh look like:

```text
                 y(t)
                 ↑
                 │
        y₃(t)    ●──────●──────●──────●──────●
                 │      │      │      │      │  $\Delta y_{t}$
        y₂(t)    ●──────●──────●──────●──────●
                 │      │      │      │      │
        y₁(t)    ●──────●──────●──────●──────●
                 │      │      │      │      │
        y₀(t)    ●──────●──────●──────●──────●──→ x(t)
                 x₀(t)  x₁(t)  x₂(t)  x₃(t)  x₄(t)
                          $\Delta x_{t}$
```


# 2D Laplace equation

$$
\frac{\partial^2u}{\partial x^2}
+
\frac{\partial^2u}{\partial y^2}
=0
$$

can be approximated using

$$
\frac{\partial^2u}{\partial x^2}
\approx
\frac{
    u_{i+1,j} - 2u_{i,j} + u_{i-1,j}
}
{\Delta x^2}
$$

and

$$
\frac{\partial^2u}{\partial y^2}
\approx
\frac{
    u_{i,j+1} - 2u_{i,j} + u_{i,j-1}
}
{\Delta y^2}.
$$

Therefore,

$$
\frac{
    u_{i+1,j} - 2u_{i,j} + u_{i-1,j}
}
{\Delta x^2}
+
\frac{
    u_{i,j+1} - 2u_{i,j} + u_{i,j-1}
}
{\Delta y^2}
=0.
$$

For $\Delta x=\Delta y=h$,

$$
\boxed{
u_{i,j} =
\frac{1}{4}
\left(
    u_{i+1,j} + u_{i-1,j} + u_{i,j+1} + u_{i,j-1}
\right)
}
$$

This shows an important idea of finite difference methods:

> The differential equation is transformed into algebraic relations between neighboring grid points.

---