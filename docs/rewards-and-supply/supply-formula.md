---
layout: default
title: Supply formula
parent: Rewards and Supply
nav_order: 3
---

# Supply formula

---

What we want is a formula that gives us the supply of NIM at any given time _t_. To do this first, we need the formula for the velocity. We know that it begins at <img src="https://render.githubusercontent.com/render/math?math=V_0"> and then decreases at a fixed percentage per year. This means it can only be a negative exponential:

<br/>

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=V(t)=V_0e^{-\beta t}">
</p>

<br/>

We know that the supply begins at <img src="https://render.githubusercontent.com/render/math?math=S_0"> and then increases by <img src="https://render.githubusercontent.com/render/math?math=V(t)"> at each instant _t_. We only need to calculate the integral over <img src="https://render.githubusercontent.com/render/math?math=V(t)">, and we get the formula:

<br/>

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=S(t)=S_0+\int_0^tV_0e^{-\beta x}dx=S_0+V_0 \left[-\frac {e^{-\beta x}}{\beta}\right]_0^t=">
</p>

<br/>

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=S_0+V_0 \left(-\frac {e^{-\beta t}}{\beta}+\frac{1}{\beta} \right)=S_0+\frac{V_0}{\beta}\left(1-e^{-\beta t}\right)">
</p>

<br/>

We still need to decide what units of time we want for _t_ and calculate the corresponding constants <img src="https://render.githubusercontent.com/render/math?math=V_0"> and <img src="https://render.githubusercontent.com/render/math?math=\beta">. We want _t_ to be in milliseconds for the code since the timestamps are in milliseconds, and we want the maximum precision possible. We also want the supply to be given in Lunas (1 NIM = 100,000 Lunas).

The initial velocity is simple to calculate. 525 NIM/minute corresponds to 875 Lunas/millisecond. For the decay, we know that the velocity should be 1.47% smaller after one year. First, we calculate the number of milliseconds in a year. By definition, there are 24∗60∗60∗1000 = 86400000 milliseconds in a day. We also know that, on average, a year has 365.2425 days. That gives us a total of 31556952000 milliseconds in a year. To get the decay then we solve the following equation:

<br/>

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=e^{-\beta \cdot 31556952000}=1-0.0147\Leftrightarrow e^{-\beta  \cdot  31556952000}=0.9853\Leftrightarrow">
</p>

<br/>

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=\Leftrightarrow \beta=-\frac{\ln(0.9853)}{31556952000}\Leftrightarrow \beta \approx4.69282 \times10^{-13}">
</p>

<br/>

From these constants and the general formula for the supply, we get the reference formula for the supply of Nimiq 2.0. This is the formula that will be in the code, and it is, by definition, ”the correct one.” (Note that we still need to know the initial supply!)

<br/>

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=S(t)=S_0-\frac{875\times31556952000}{\ln(0.9853)}(1-e^\frac{\ln (0.9853)}{31556952000} t)">
</p>

<br/>

However, it might be more useful for other applications to have a supply formula that uses days instead of seconds and returns NIM instead of Lunas, so that we don’t have to handle very large numbers. In this case, we can recalculate the constants to yield:

<br/>

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=V_0=\text{75000 NIM/day}">
</p>

<br/>

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=\beta=-\frac{\ln(0.9853)}{365.2425}\approx0.000040546">
</p>

<br />

[Back to top](#)