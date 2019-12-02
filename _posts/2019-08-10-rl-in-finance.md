---
layout: article
title: Reinforcement Learning in Trading --  NYU Reinforcement Learning in Finance Course
tags: RL 
mathjax: true
---

Finance is one of the most difficult application field of applying reinforcement learning, because we cannot model the financial market perfectly as a fully observable environment. And in trading, where the risk control is important but becomes more difficult because of the parial-observable environment. NYU's cource Reinfcement Learning in Traing <a href="https://www.coursera.org/specializations/machine-learning-reinforcement-finance#"> link </a> introduces a risk management reward shaping method that make it possible to use the standard risk neutral formulation of Q-learning. 



## Portfolio Model

First of all, to build a simple portfolio model.
Consider a universe of N assets with the vector $P_t$ of market prices at time t. And there's a back cash account with risk free interest rate $r_f$. $x_t \in R^{N}$ describes the position of an individual assets. Trades $u_t \in R^{N}$ are made at the begining of the intervals t, the portfolio vector $x_t^{+}$ after the trades are deterministic:

$$ x_{t}^{+} = x_t + u_t$$

The total portfolio value is:

$$v_t = 1^{T} x_{t} + b_t $$

where 1 is a vector of ones, the post-trade portfolio is: 


$$ v_t^{+} = q^{T}x_t + b_t^{+} = 1^{T} (x_t + u_t) + b_t^{+} = v_t + 1^{T}u_t + b_t^{+} - b_t  $$

The return of the asset i over time peroid t is defined as:

$$ (r_t)_i  = \frac{(p_{t+1})_i - (p_t)_i}{(p_t)_i}, i=1, ..., n$$

The asset positions at the next time period are:

$$ x_{t+1} = x_{t}^{+} + r_{t} \bigodot x_t^+ $$

Use a linear estimination of one-period excess asset returns:

$$r_t - r_{f}1 = W_{t} z_t - M_t^{T} u_t  + \epsilon_t$$
$z_t$ is a vector of predictions 
$W_t$ is a factor loading matrix
$M_t$ is a matrix of permanent market impacts, $ \epsilon_t$ is a vector of residuals. 

The next-period portfolio value is obtained as follows:
$$ v_{t+1} = 1^Tx_{t+1} = (1+r_t)^T x_t^+ = (1+r_t)^T(x_t + u_t)$$

The change of the portfolio value also is the risk-free growth $r_f$ is:

$$\delta v_t = v_{t+1} - (1+r_f)v_t = (1+r_t)^T (x_t +u_t) + (1+f_f)b_t^+ - (1+r_f)1^Tx_t - (1+r_f)b_t = (r_t -r_f 1)^T (x_t + u_t)$$

The equation can be applied at any step except for the last step. The last step, we need to impose terminal conditions.  For example, a boundary conditions for an index/benchmark tracking problem: $x_T = x_T^B$ where $x_T^B$ is a benchmark portifolio at time T. In this way,  action $u_T = x_T^B -x_{T-1}$ is deterministic and is not subject to optimization that should be applied to T remaining actions $y_{T-1, ..., U_0}$


## One Period Rewards

With the excess return equation and portfolio change equation we derive the instantaneous random reward received upon taking action $u_t$:
$$R_{t}^{(0)} (x_t, u_t) = (W_tz_t -M_t^T u_t +\epsilon_t)^T (x_t + u_t)$$

From the euqation we can see that the reward is risky because it depends on the noise term $\epsilon$. Therefore we need a risk penalty. 

Use the variance of the instantaneoud reward as a simple quadratic risk meature. 

$$ R_t^{Risk} = - \lambda Var_t[R_t^{0}(x_t, u_t) | x_t + u_t] = -\lambda (x_t + u_t)^T \sum_t(x_t + u_t)$$
Here $\lambda $ is a risk aversion parameter, and $\sum_t$ is the noise covariance matrix. 

Next, we also need to include the penalty of fees:

$$R_t^{(fee)} (x_t, u_t) = -{k_t^+}^T u_t^+ - k_t^{-T} u_t^{-}$$

ALso, the transaction will impace the market, so the impact penalty is:

$$R_t^{(impact)} (x_t, u_t) = -x_t^T ({\Theta_t^+}^T u_t^+ + \Theta_t^{-T} u_t^- + \Pi_t^T z_t )$$

The final one-step reward is:

$$R_t(x_t, u_t) = R_{t}^{(0)}  +  R_t^{(Risk)} + R_t^{(fee)} + R_t^{(impact)}$$

We can write the one-step expected reward as a quadratic functional of states and actions:
$$R_t(x_t, u_t) = a_t^TR_{aat} a_t + x_t^TR_{xxt}x_t + a_t^TR_{axt}x_t +a_t^TR_{at} + x_t^TR_{xt} $$
