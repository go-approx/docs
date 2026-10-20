---
title: "interp"
weight: 10
description: "Interpolation, splines and smoothing in pure Go, validated against SciPy and NumPy. Released v0.1.0."
---

<!-- From go-approx/interp v0.1.0, README.md: the text is the library's. Change it there first. -->

**Status: released v0.1.0.**

Interpolation, splines and smoothing in pure Go (`CGO_ENABLED=0`), validated
against SciPy and NumPy.

```sh
go get github.com/go-approx/interp@v0.1.0
```

```go
import "github.com/go-approx/interp"

s, _ := interp.NewCubicSpline(x, y, "not-a-knot")   // scipy CubicSpline, MATLAB spline
v := s.At(2.5); d := s.Derivative(1).At(2.5); area := s.Integrate(0, 4)

b, _ := interp.MakeInterpSpline(x, y, 5, nil)       // make_interp_spline
sm, _ := interp.MakeSplrep(x, y, 3, 0.05, nil)      // FITPACK smoothing spline
```

## What is provided

| here | SciPy / NumPy |
|---|---|
| `NewInterp1D` (`Linear`, `Nearest`, `NearestUp`, `Previous`, `Next`) | `interpolate.interp1d` |
| `Interp` | `numpy.interp` |
| `Interp1` (`linear nearest next previous pchip cubic spline makima`) | the matching classes; MATLAB `interp1` |
| `NewCubicSpline`, `NewCubicSplineBC` (not-a-knot, natural, clamped, periodic, prescribed derivatives), `PPoly` | `CubicSpline`, `PPoly` |
| `NewCubicHermite`, `NewPchip`, `NewAkima` | `CubicHermiteSpline`, `PchipInterpolator`, `Akima1DInterpolator` / `makima` |
| `MakeInterpSpline`, `BSpline`, `NewDesignMatrix` | `make_interp_spline`, `BSpline`, `BSpline.design_matrix` |
| `MakeLsqSpline` | `make_lsq_spline` |
| `MakeSplrep` (FITPACK smoothing: automatic knots, smoothing factor, weights, user knots) | `make_splrep`, `splrep`, `UnivariateSpline` |
| `NewRGI`, `Interpn` (`linear`, `nearest`, `slinear`, `cubic`, `quintic`) | `RegularGridInterpolator`, `interpn` |
| `MakeNdInterp`, `NdBSpline` (tensor-product splines) | `make_ndbspl`, `NdBSpline` |
| `NewBicubic` | `RectBivariateSpline(..., s=0)` |

The README has the full table with the MATLAB equivalents.

## Verification

The reference tools are SciPy 1.18.1 and NumPy 2.5.4, run as oracles from Go
by the tag-gated harness `oracle_test.go`. The harness generates the inputs
and writes the answers to `testdata/*.json`; the ordinary tests read them and
need no Python.

Inputs include the edge cases: 1, 2 and 3 points, duplicate and unsorted x,
NaN and Inf in x and y, queries outside the range and exactly on the nodes,
nodes clustered over 14 orders of magnitude, offsets of 1e8, spans of 1e-9,
values of 1e150 and 1e-290, mismatched lengths, invalid boundary conditions
and knots, and n = 1e5 and 1e6. Where SciPy raises, the Go constructor returns
an error and the fixture says so.

Octave is not installed on the build host, so MATLAB/Octave semantics are
implemented from the MathWorks documentation and checked through the SciPy
objects that implement the same maths, not against Octave.

## Differences from SciPy

Errors are returned, not raised, and no function panics on input.
`NewInterp1D` rejects NaN in x. `numpy.interp` is reproduced with `math.FMA`
because NumPy's C loop is compiled with fused multiply-add on arm64; every
other kernel rounds each product before summing. Tensor-product splines are
solved one axis at a time. `RectBivariateSpline` smoothing (`s > 0`) and the
periodic smoothing spline of `make_splrep` are not provided.

## Performance

[docs/perf.md](https://github.com/go-approx/interp/blob/main/docs/perf.md) has
the table: construction and evaluation at 10^3 to 10^6 points, Go against
SciPy on one core, interleaved rounds, with the load average of the host.

The README, measured 2026-10-19 (load 3–5), reports interp faster than SciPy
on PCHIP, Akima/makima, cubic evaluation, periodic cubic construction,
nearest-neighbour `interp1d`, `make_splrep`, least-squares splines and every
`RegularGridInterpolator` construction.

**Open defects, still slower than the reference** (from the README): `splrep`
against SciPy's legacy FITPACK `splrep` (0.33–0.38x), `RectBivariateSpline`
construction (0.84–0.88x), `np.interp` and linear `interp1d` evaluation
(0.86–0.89x), quintic B-spline evaluation (0.92–0.97x) and not-a-knot cubic
construction from 10^5 points (0.92x).

## Gaps

Scattered-data interpolation (Delaunay, natural neighbour, RBF) is a later
work item. There is no periodic smoothing spline, no parametric splines
(`make_splprep`), no `make_smoothing_spline`, no MATLAB `interp2` `cubic`
kernel, no complex-valued data, no N-dimensional `y`, and `PPoly.roots` and
`BSpline.roots` are not provided.

Source: [go-approx/interp](https://github.com/go-approx/interp), BSD-3-Clause.
