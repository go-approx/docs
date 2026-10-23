---
title: "go-approx documentation"
linkTitle: "Home"
type: docs
cascade:
  type: docs
description: "Interpolation, splines and polynomials in pure Go (CGO=0), validated against SciPy and NumPy."
---

**go-approx is approximation in pure Go.** Two libraries reproduce the
behaviour of the reference tools and test it against them: `interp` against
SciPy 1.18.1 and NumPy 2.5.4, `poly` against NumPy 2.5.4. Both run the
reference as an oracle from Go, write the answers to committed fixtures, and
state where the Go code differs or is slower. Chebyshev and AAA rational
approximation are planned and not written yet.

## The libraries

| Library | Status | What it does |
| --- | --- | --- |
| [interp]({{< relref "interp" >}}) | released v0.2.1 | interpolation, splines and smoothing: `interp1d`, cubic splines, PCHIP, Akima and makima, B-splines, FITPACK smoothing splines and `splrep`, regular-grid and tensor-product interpolation |
| [poly]({{< relref "poly" >}}) | released v0.1.0 | polynomials: evaluation, arithmetic, least-squares fitting, roots, and the Chebyshev, Legendre, Hermite, HermiteE and Laguerre bases |

## Install

```sh
go get github.com/go-approx/interp@v0.2.1
go get github.com/go-approx/poly@v0.1.0
```

Each module is pure Go (`CGO_ENABLED=0`).

## Where each library's own documents are

| Library | Source | Performance |
| --- | --- | --- |
| interp | [README](https://github.com/go-approx/interp/blob/main/README.md) | [docs/perf.md](https://github.com/go-approx/interp/blob/main/docs/perf.md) |
| poly | [README](https://github.com/go-approx/poly/blob/main/README.md) | [docs/perf.md](https://github.com/go-approx/poly/blob/main/docs/perf.md) |

## Part of a family

go-approx belongs to the family of [go-numerics](https://github.com/go-numerics)
(optimisation, quadrature, special functions) and
[go-statistics](https://github.com/go-statistics).
