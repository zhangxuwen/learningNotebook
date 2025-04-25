## Definition

$f'(x_0)$ derivative of $f$ at $X_0$ , which is the slope of the tangent line to $y$ 

$f'(x_0) = \displaystyle \lim_{\triangle x \to 0}\frac{ f(x_0 + \triangle x) - f(x_0) }{ \triangle x }$



## Four Discontinuity

1. Removable Discontinuity
2. Jump Discontinuity
3. Infinite Discontinuity
4. Oscillating Discontinuity



## Genral Formulas

1. $(u+v)' = u' + v'$
2. $(Cu)' = Cu'$
3. $(uv)' = u'v + uv'$
4. $(\frac{u}{v})' = \frac{u'v - uv'}{v^2}$ 



## Chain Rule

$y=f(u)$ and $u=g(x)$ differentiable functions , then composite function $y=f(g(x))$ 

$\frac{dy}{dx} = \frac{dy}{du} \cdot \frac{du}{dx}$



## Ingeneral

$y = f(x), g(y) = x$

$=> g(f(x)) = x, g = f^{-1}$

then we call $f = g^{-1}$



## Hot to get e

find answer in $\displaystyle \lim_{n \rightarrow \infty}(1 + \frac{1}{n})^n$

$\displaystyle \ln ((1 + \frac{1}{n})^n) = n \ln (1+\frac{1}{n})$

let $\triangle x = \frac{1}{n}$

then get $\frac{1}{\triangle x} \ln (1 + \triangle x) = \frac{1}{\triangle x} (\ln (1 + \triangle x) - \ln1)$

beacuse $\ln1 = 0$, and we can know it is $e$ from  $\frac{d(lnx)}{dx}|{_x=1} => \frac{1}{x}|_{x=1}=1$ and $\frac{df(1)}{d\triangle x} = \frac{f(1 + \triangle x)}{\triangle x}$ 

so

$\displaystyle \lim_{n \to \infty} (1 + \frac{1}{n})^n = e ^ {\displaystyle \lim_{n \to \infty} \ln [(1 + \frac{1}{n})^n]} = e$



## Linear approximations

$f(x) \approx f(x_0) + f'(x_0)(x - x_0)$

### Example

when $x \approx 0$

$\ln (1 + x) \approx x$

$(1 + x)^r \approx 1 + rx$

$sinx \approx x$

$cosx \approx 1$

$e^x \approx 1 + x$



## Guadratic  approximation

$f(x) \approx f(x_0) + f'(x_0)(x - x_0) + \frac{f''(x_0)}{2}(x - x_0)^2$

### Example

when x near 0

$sinx \approx x$

$cosx \approx 1 - \frac{1}{2}x^2$

$e^x \approx 1 + x + \frac{1}{2}x^2$

$\ln (1 + x) \approx x - \frac{1}{2}x^2$

$(1 + x)^r \approx 1 + r + \frac{r (r - 1)}{2}x^2$



## Definition

which is that is if $f'(x_0)=0$ call $x_0$ a critical point



## Newton's method

tangent line

$y - y_0 = m(x - x_0)$

$x_1$ is the $x - intercept$

$0 - y = m(x_1 - x_0)$

$x_1 = x_0 - \frac{f(x_0)}{f'(x_0)}$

then we can get

$x_{n + 1} = x_n - \frac{f(x_n)}{f'(x_n)}$



## MEAN VALUE THEOREM

**provided f is differentiable in a<x<b and continuous in a≤x≤b**

$\frac{f(b) - f(a)}{b - a} = f'(c)$

from some c, $a < c < b$



## Application to graphing

1. if $f'$ is positive, then $f$ is increasing
2. if $f'$ is negative, then $f$ is decreasing
3. if $f'$ , then f is constant



## Differentials

$y = f(x)$

Differentials of $y$ (or $f$)：$dy = f'(x)dx \Leftrightarrow \frac{dy}{dx} = f'(x)$

$\frac{dy}{dx} = f'(x)$​ named Leibniz interpretation of derivative a ratio of these of "infinitesimals"



## Antideriuative

$G(x) = \int g(x) dx$

antiderivative of $g =$ indefinite integral of g

uniqueness of atiderivatives up to a constant

**Theorem**：if $F' = G'$ then $F = G$

​	so $F(x) = G(x) + C$



## Differential equation

example

1. Substitution

   $\frac{dy}{dx} = f(x)$

   $y = \int f(x) dx$

2. Sepanation of variables

   $\frac{dy}{dx} = f(x)g(y)$

   $\frac{dy}{g(y)} = f(x)dx$

   $H(y) = \int \frac{dy}{g(y)}, F(x) = \int f(x)dx$

   $\Rightarrow H(y) = F(x) + C$, C constant

   $y = H^{-1}(F(x) + c)$

   

   # Fundamental theorem of calculus

   If $F'(x) = f(x)$ then $\int^{b}_{a} f(x)dx = F(b) - F(a)$
   
   Notation
   
   $F(b) - F(a) = F(x)|^{b}_{a}$
   
   
   
   # properties of integrals
   
   1. $\int^{b}_{a} (f(x) + g(x))dx = \int^{b}_{a} f(x)dx + \int^{b}_{a} g(x)dx$
   
   2. $\int^{b}_{a} cf(x)dx = c \int^{b}_{a} f(x)dx$(c constant) (c constant)
   
   3. $a<b<c, \int^{b}_{a} f(x)dx + \int^{c}_{b}f(x)dx = \int^{c}_{a} f(x)dx$
   
   4. $\int^{a}_{a} f(x)dx = 0$ (=F(a) - F(a))
   
   5. $\int^{b}_{a} f(x)dx = -\int^{a}_{b} f(x)dx$
   
   6. (Estimation)
   
      If $f(x) \leq g(x)$, then
   
      $\int^{b}_{a} f(x) \leq \int^{b}_{a} g(x)dx$
   
   
   
   # Fundamental Theorem of Calculus 1
   
   If $F' = f$, then
   
   $\int^{b}_{a} f(x)dx = F(b) - F(a)$
   
   INFO ABOUT $F' \Rightarrow$ INFO ABOUT F 
   
   ## COMPARE FTC1 WITH MVT
   
   * **FTC1**
   
   $\triangle F = F(b) - F(a), \triangle x = b - a$
   
   $\triangle F = \int^{b}_{a} f(x)dx$ (FTC1)
   
   $\frac{\triangle F}{\triangle x} = \frac{\int^{b}_{a} f(x)dx}{b - a}$
   
   then,
   
   $\triangle F = [average(F')] \triangle x$
   
   * **MVT**
   
   $\triangle F = F'(c) \triangle x$, some $c \in (a, b)$
   
   then,
   
   $\triangle F \in ((\min F')\triangle x, (max F')\triangle x)$, $x \in (a, b)$
   
   
   
   # Fundamental Theorem of Calculus 2
   
   If f is continuous and $G(x) = \int^{x}_{a} f(t)dt, t \in [a, x]$
   
   then, $G'(x) = f(x)$
   
   $G(x)$ solves the differential equation 
   $$
   \begin{cases}
   y' = f \\
   y(a) = 0
   \end{cases}
   $$
   
   
   
   
   
   
   
