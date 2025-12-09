# Instrumental Variables (IV) Analysis: Education & Income

![Python](https://img.shields.io/badge/Python-3.12%2B-blue)
![Analysis](https://img.shields.io/badge/Analysis-Causal%20Inference-green)
![Status](https://img.shields.io/badge/Status-Complete-success)

This repository contains a Jupyter Notebook demonstrating **Instrumental Variables (IV)** analysis using **Two-Stage Least Squares (2SLS)**. 

The project simulates a classic econometric scenario: estimating the causal return of **Education** on **Income** while controlling for unobserved **Ability** (Confounding Variable). It demonstrates how standard OLS regression fails in the presence of endogeneity and how IV fixes this bias.

## 📉 The Problem: Endogeneity

In causal inference, we often want to measure the effect of a treatment $X$ (Education) on an outcome $Y$ (Income). A standard OLS regression assumes:

$$Y = \beta_0 + \beta_1 X + \epsilon$$

However, in the real world, **Ability** (which is hard to measure) affects *both* Education and Income. 
* High ability $\rightarrow$ More Education.
* High ability $\rightarrow$ Higher Income (regardless of education).

This creates **Endogeneity** ($Cov(X, \epsilon) \neq 0$). Because we cannot control for Ability, OLS attributes the income boost from Ability to Education, resulting in an **Overestimation Bias**.

## 🛠 The Solution: Instrumental Variables

To solve this, we use an **Instrument ($Z$)**. In this analysis, we use **Distance to the nearest college**.

For an instrument to be valid, it must satisfy two conditions:
1.  **Relevance:** $Cov(Z, X) \neq 0$ (Living closer to college makes you more likely to attend).
2.  **Exclusion Restriction:** $Cov(Z, \epsilon) = 0$ (Distance doesn't affect your income directly, only through education).

### The 2SLS Method (Two-Stage Least Squares)

**Stage 1:** Regress the endogenous variable ($X$) on the instrument ($Z$).
$$\widehat{Education} = \alpha_0 + \alpha_1 Distance + \nu$$

**Stage 2:** Regress the outcome ($Y$) on the *predicted* endogenous variable from Stage 1.
$$Income = \beta_0 + \beta_{IV} \widehat{Education} + \epsilon$$

---

##  Implementation Details

### 1. Variables Simulated
| Variable | Type | Description |
| :--- | :--- | :--- |
| `Income` | Outcome ($Y$) | Hourly wage (Dependent Variable). |
| `Education` | Treatment ($X$) | Years of schooling (Endogenous). |
| `Ability` | Confounder | Unobserved intelligence/talent. |
| `Distance` | Instrument ($Z$) | Distance to nearest college (Exogenous). |

### 2. True Causal Model
The data is generated with a known ground truth to verify our results:
$$Income = 10 + \mathbf{2.5} \times Education + 1.5 \times Ability + Noise$$

*The target coefficient we want to recover is **2.5**.*

### 3. Requirements
This analysis relies on `linearmodels` for professional IV estimation.

```bash
pip install pandas numpy matplotlib seaborn statsmodels linearmodels
```
