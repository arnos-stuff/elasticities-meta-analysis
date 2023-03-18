# Meta Analysis of Elasticities of Income Taxation

## Description

This is an attempt to open source the estimations of the paper by [Neisser et al. (2017)](https://www.econstor.eu/handle/10419/168333). The paper is a meta analysis of the literature on the elasticity of income taxation. The authors use a meta regression approach to estimate the elasticity of income taxation.

## Get data using CDN API

The data can be downloaded using the [CDN API](https://github.com/jsdelivr/jsdelivr) as follows:

```bash
curl -sSL https://cdn.jsdelivr.net/gh/arnos-stuff/elasticities-meta-analysis@latest/elasticities-parsed.csv > elasticities.csv
```

or if you're on Windows:

```powershell
iwr -uri https://cdn.jsdelivr.net/gh/arnos-stuff/elasticities-meta-analysis@latest/elasticities-parsed.csv -OutFile elasticities.csv
```

## Meta Analysis regression model

The model is derived as:

```math
\zeta_{i s}=\zeta_0+\beta X_i+\delta Z_s+\epsilon_{i s}$$
```

where $\zeta_{i s}$ represents the i-th estimate estimate collected from study s. $\zeta_0$ denotes the intercept, $X_i$ and $Z_s$ represent study and estimate-specific variables respectively, and $\epsilon_{i s}$ is the sampling error.

Since the variances of collected estimates are heteroscedastic, it is preferable to estimate the model using Weighted Least Squares (WLS) rather than through an OLS estimation.  

The authors use the inverse of the error term variance of an individual estimate

```math
\mathrm{V}\left(\hat{\zeta}_{i s}\right)=\sigma_{i s}^2$$
```

as analytic weights.  

Hence, The authors give observations with smaller variances a larger weight and greater influence on the estimates since precision can be seen as an indicator of quality.  

Standard errors are clustered at the study level to control for study dependence in the estimates

# Estimating Elasticities of income wrt tax rates

The most standard regression specification is derived as:

```math
\log \left(\frac{z_{i t}}{z_{i t-k}}\right)=\zeta \log \left(\frac{1-\tau_{i t}}{1-\tau_{i t-k}}\right)+\delta f\left(z_{i t-k}\right)+\theta X_{i t-k}+\mu_t+\epsilon_{i t}
```

where:

* $z_{i t}$ is the income of taxpayer $i$ in year $t$
* $i$ refers to the respective taxpayer
* $t$ is the underlying year
* $\zeta$ is the parameter of interest
* $k$ is the chosen difference length
* $t-k$ denotes the base-year
* $X_{i t-k}$ is a vector of control variables
* Time dummies $\mu_t$ control for any omitted variables in differences that are the same on average for all individuals.
* $f\left(z_{i t-k}\right)$ denotes the income control in order to capture non-tax related income trends.

In equation (2) $\zeta$ represents the elasticity of the income tax base that measures the responsiveness of income to changes in the net-of-tax rate $(1-\tau)$
