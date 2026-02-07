# Chapter 2
## Equilibrium and Stability

**Autonomous equations:** DEs of the form $dx/dt = f(x)$, where $t$ does not appear explicitly.

**Critical points:** Solutions of $f(x) = 0$. Each critical point $x = c$ gives a constant (equilibrium) solution $x(t) \equiv c$.

**Stability of critical points:**
- A critical point $c$ is **stable** if solutions starting near $c$ stay near $c$ for all $t > 0$. Formally: for each $\epsilon > 0$, there exists $\delta > 0$ such that $|x_0 - c| < \delta \implies |x(t) - c| < \epsilon$ for all $t > 0$.
- A critical point is **unstable** if it is not stable.

*note:* The definition says that if a solution starts close enough a critical point will approach the critical point if it is stable. However, we rarely use the $\epsilon$ and $\delta$ to determine the stability. Rather we use a geometric argument called phase diagrams. 



**Phase diagrams:** A number line showing critical points and arrows indicating the sign of $f(x)$ between them. Arrows point right where $f(x) > 0$ (increasing) and left where $f(x) < 0$ (decreasing).

Since we deal with 1st order ODE here, so the phase digram is just a line. Often it's useful to plot the rhs function f(x) so you see where the critical points are and where it is positive and negative and so you can draw arrow accordingly. That is, in the positive region, draw an arrow pointing to right; In a negative region, draw an arrow point to the left.  

**Key examples:**
- *Logistic equation* $dx/dt = kx(M - x)$: critical points $x = 0$ (unstable) and $x = M$ (stable). The stable equilibrium $x = M$ acts as a **funnel**; the unstable $x = 0$ acts as a **spout**.

  Phase diagram for $dx/dt = f(x) = kx(M - x)$:
  ```
  f(x) < 0       f(x) > 0       f(x) < 0
  ◄──────── ● ────────► ● ◄────────
            x=0          x=M
          (unstable)    (stable)
  ```
  - $x < 0$: both arrows point away from $x = 0$ → **unstable** (spout)
  - $0 < x < M$: arrows point toward $x = M$ → **stable** (funnel)
  - $x > M$: arrows also point toward $x = M$ → confirms **stable**

**Harvesting a logistic population:** $dx/dt = kx(M - x) - h$. Rewrite as $dx/dt = k(N - x)(x - H)$ where $H, N$ are roots of $-kx^2 + kMx - h = 0$:
$$H, N = \frac{1}{2}\left(M \pm \sqrt{M^2 - 4h/k}\right)$$
- If $4h < kM^2$: two critical points, $N$ stable (limiting population), $H$ unstable (threshold).
- If $4h = kM^2$: one semistable critical point (critical harvesting).
- If $4h > kM^2$: no critical points, population always declines to extinction.

**Bifurcation:** The value of a parameter at which the number or nature of critical points changes (e.g., $h = \frac{1}{4}kM^2$ above). A **bifurcation diagram** plots critical points vs. the parameter.

## Euler's method

**Motivation:** Most DEs $dy/dx = f(x, y)$ cannot be solved in closed form. Euler's method gives a numerical approximation by following the slope field in small steps.

**Algorithm:** Given the IVP $dy/dx = f(x, y)$, $y(x_0) = y_0$, and step size $h$:
$$y_{n+1} = y_n + h \cdot f(x_n, y_n), \qquad x_{n+1} = x_n + h$$

This generates successive approximations $y_1, y_2, \ldots$ to the true values $y(x_1), y(x_2), \ldots$

**Geometric idea:** At each point $(x_n, y_n)$, follow the tangent line (slope = $f(x_n, y_n)$) for a horizontal distance $h$ to reach the next point.

**Error:**
- **Local error:** error introduced in a single step (tangent line departing from true curve).
- **Cumulative error:** total accumulated drift from the true solution after many steps.
- **Theorem (Error bound):**  Let $y_n$ be the numerical solution at n-th step by Euler's method and $y(x_n)$ be the true solution. The error in Euler's method is of order $h$: $|y_n - y(x_n)| \leq Ch$ for some constant $C$. This is called 1st order accurate. Halving $h$ roughly halves the error. 
- **Roundoff error:** too-small $h$ means more steps, accumulating floating-point errors. There is a practical trade-off.

**Improved Euler method (Heun's method):** A predictor-corrector scheme with error of order $h^2$:
$$k_1 = f(x_n, y_n)$$
$$u_{n+1} = y_n + h \cdot k_1 \quad \text{(predictor)}$$
$$k_2 = f(x_{n+1}, u_{n+1})$$
$$y_{n+1} = y_n + \frac{h}{2}(k_1 + k_2) \quad \text{(corrector)}$$

Uses the average of slopes at both endpoints of each subinterval. Error satisfies $|y_n - y(x_n)| \leq Ch^2$, so halving $h$ cuts error by a factor of 4. This is called 2nd order accurate. 

**Caution:** Always validate by running with successively smaller step sizes. If results don't stabilize, the solution may have a singularity or the problem may be ill-conditioned (e.g., $dy/dx =  y^2$, $y(0) = 1$ blows up near $x \approx 1$).

**Demo:** Vibe code on [Matlab Online](https://matlab.mathworks.com/)
using a prompt like this "Write a script to compare Euler's method and Improved Euler for y' = y, y(0)=1. Reduce step size by half 5 times, store errors and compute error ratios." 