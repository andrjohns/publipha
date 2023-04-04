
<!-- README.md is generated from README.Rmd. Please edit that file -->

# publipha <img src="man/figures/logo.png" align="right" width="177" height="65" />

[![CRAN_Status_Badge](https://www.r-pkg.org/badges/version/publipha)](https://cran.r-project.org/package=publipha)

An `R` package for Bayesian meta-analysis that accounts for publication
bias or *p*-hacking.

## Overview

publipha is an package for doing Bayesian meta-analysis that accounts
for publication bias or *p*-hacking. Its main functions are:

- `psma` does random effects meta-analysis under publication bias with a
  one-sided *p*-value based *selection probability*. The model is
  roughly the same as that of [(Hedges,
  1992)](https://www.jstor.org/stable/pdf/2246311.pdf)
- `phma` does random effects meta-analysis under a certain model of
  *p*-hacking with a one-sided *p*-value based propensity to *p*-hack.
  This is based on the forthcoming paper of [by Moss and De Bin
  (2019)](https://arxiv.org/abs/1911.12445).
- `cma` does classical random effects meta-analysis with the same priors
  as `psma` and `cma`.

## Installation

Use the following command from inside `R`:

``` r
# install.packages("devtools")
devtools::install_github("JonasMoss/publipha")
```

## Usage

Call the `library` function and use it like a barebones `metafor::rma`.
The `alpha` tells `psma` or `phma` where they should place the cutoffs
for significance.

``` r
library("publipha")
# Publication bias model
set.seed(313) # For reproducibility
model_psma = publipha::psma(yi = yi,
                            vi = vi,
                            alpha = c(0, 0.025, 0.05, 1),
                            data = metadat::dat.bangertdrowns2004)

# p-hacking model
set.seed(313)
model_phma = publipha::phma(yi = yi,
                          vi = vi,
                          alpha = c(0, 0.025, 0.05, 1),
                          data = metadat::dat.bangertdrowns2004)

# Classical model
set.seed(313)
model_cma = publipha::cma(yi = yi,
                          vi = vi,
                          alpha = c(0, 0.025, 0.05, 1),
                          data = metadat::dat.bangertdrowns2004)
```

You can calculate the posterior means of the meta-analytic mean with
`extract_theta0`:

``` r
extract_theta0(model_psma)
#> [1] 0.1277197
```

``` r
extract_theta0(model_cma)
#> [1] 0.2212093
```

If you wish to plot a histogram of the posterior distribution of `tau`,
the standard deviation of the effect size distribution, you can do it
like this:

``` r
extract_tau(model_psma, hist)
```

<img src="man/figures/README-unnamed-chunk-4-1.png" width="750px" />

## References

- [Hedges, Larry V. “Modeling publication selection effects in
  meta-analysis.” Statistical Science (1992):
  246-255.](https://www.jstor.org/stable/pdf/2246311.pdf)
- [Moss, Jonas and De Bin, Riccardo. “Modelling publication bias and
  p-hacking” (2019)](https://arxiv.org/abs/1911.12445)

## How to Contribute or Get Help

If you encounter a bug, have a feature request or need some help, open a
[Github issue](https://github.com/JonasMoss/publipha/issues). Create a
pull requests to contribute. This project follows a [Contributor Code of
Conduct](https://www.contributor-covenant.org/version/1/4/code-of-conduct.md).
