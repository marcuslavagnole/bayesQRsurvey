# tauBayesW: Bayesian Weighted Quantile Regression

[![R-CMD-check](https://github.com/torodriguezt/tauBayesW/workflows/R-CMD-check/badge.svg)](https://github.com/torodriguezt/tauBayesW/actions)
[![CRAN status](https://www.r-pkg.org/badges/version/tauBayesW)](https://CRAN.R-project.org/package=tauBayesW)

**tauBayesW** is an R package for **Bayesian weighted quantile regression** for complex survey designs. The package provides both single and multiple quantile estimation using efficient MCMC and EM algorithms with fast C++ implementations.

<p align="center">
  <img src="web/public/logo_tau.png" width="180" alt="tauBayesW logo"/>
</p>

---

## ⚙️ Features

- **Survey weights**: Handles complex survey designs with observation weights
- **Multiple algorithms**: MCMC (ALD, Score, Approximate) and EM methods  
- **Fast computation**: C++ implementations using Rcpp, RcppEigen, and RcppArmadillo
- **Comprehensive output**: Detailed summaries with convergence diagnostics
- **Visualization**: Built-in plotting functions with ggplot2
- **CRAN ready**: Full documentation and examples

---

## 🚀 Performance Benchmark

The following benchmark compares the R and C++ versions of the main algorithms on a simulated dataset with 100,000 observations and 10 predictors.

| Algorithm            | R Time (seg) | C++ Time (seg) | R RAM  | C++ RAM | Speedup Factor | Memory Saving |
|----------------------|--------------|----------------|--------|---------|----------------|----------------|
| EM_BWQR_AL_MO        | 2.44         | 0.0032         | 2.3 GB | 190 MB  | ×769           | ~12×           |
| MCMC_BWQR_AL         | 12.3         | 0.01           | 2.0 GB | 50 MB   | ×1100          | ~40×           |
| MCMC_BWQR_AP         | 21.78        | 2.81           | ? GB   | ? GB    | ×7.8           | ?              |
| MCMC_BWQR_SL         | 9.7          | 1.4            | ?      | ?       | ×7.1           | ~7.2×          |

> Test environment: R 4.4.2, Windows 11, Intel i5 13600-K, 32 GB RAM

---

## 📦 Installation

### From GitHub (development version)

```r
# Install 'devtools' (or 'remotes') once
install.packages("devtools")

# Install the latest tauBayesW from GitHub
devtools::install_github("torodriguezt/tauBayesW")

# Load the package
library(tauBayesW)
```

---

## 🎯 Main Functions

### Single Quantile Estimation: `bqr.svy()`

Fits Bayesian quantile regression for a single quantile using MCMC methods:

- **ALD (Asymmetric Laplace Distribution)**
- **Score**
- **Approximate**

```r
library(tauBayesW)

# Simulate data
set.seed(123)
n <- 100
x1 <- rnorm(n)
x2 <- runif(n) 
y <- 1 + 2*x1 - 0.5*x2 + rnorm(n)
weights <- runif(n, 0.5, 2)
data <- data.frame(y, x1, x2)

# Fit single quantile regression
fit <- bqr.svy(y ~ x1 + x2, weights = weights, data = data, 
               quantile = 0.5, method = "ald", niter = 1000)
summary(fit)
```

### Multiple Quantile Estimation: `mo.bqr.svy()`

Fits Bayesian quantile regression for multiple quantiles using EM algorithm:

```r
# Fit multiple quantile regression
fit_multi <- mo.bqr.svy(y ~ x1 + x2, weights = weights, data = data, 
                        quantile = c(0.25, 0.5, 0.75), algorithm = 'em')
summary(fit_multi)
```

### Visualization: `plot()`

Create plots showing fitted quantile curves with credible intervals:

```r
# Plot results
plot(fit)
plot(fit_multi)
```

---

## 📚 Methods

The package implements methods based on:

- Marcus L Nascimento, Kelly C M Gonçalves, Bayesian Quantile Regression Models for Complex Survey Data Under Informative Sampling, Journal of Survey Statistics and Methodology, Volume 12, Issue 4, September 2024, Pages 1105–1130, [https://doi.org/10.1093/jssam/smae015](https://academic.oup.com/jssam/article-abstract/12/4/1105/7642687)
- [GitHub Repository: bqr_informative_sampling](https://github.com/marcuslavagnole/bqr_informative_sampling)

---

## 👥 Authors

- **Marcus L. Nascimento**  
  Postdoctoral Researcher, School of Applied Mathematics, Fundação Getulio Vargas (FGV EMAp)

- **Kelly Christina Mota Gonçalves**  
  Professor, Department of Statistics, Federal University of Rio de Janeiro (UFRJ)

- **Johntan Cardona Jiménez**  
  Professor, Department of Statistics, Universidad Nacional de Colombia (UNAL)

- **Tomás Rodríguez Taborda**  
  Student, Department of Statistics and Department of Computer and Decision Sciences, Universidad Nacional de Colombia (UNAL)

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.
