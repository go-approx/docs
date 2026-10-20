# docs

[![docs](https://github.com/go-approx/docs/actions/workflows/docs.yml/badge.svg)](https://github.com/go-approx/docs/actions/workflows/docs.yml)

**The go-approx documentation site**, published at
<https://go-approx.github.io/docs/>.

Hugo with the [Hextra](https://github.com/imfing/hextra) theme (the
`tannevaled/hextra` fork, submodule `themes/hextra`, pinned at v0.16.0), built
like the [go-numerics documentation](https://github.com/go-numerics/docs) with
go-approx's branding. It documents
[`go-approx/interp`](https://github.com/go-approx/interp) and
[`go-approx/poly`](https://github.com/go-approx/poly), one section each,
summarising the library's README with its verification, its performance and
its release status (both released v0.1.0).

## Versions

| source | published at |
| --- | --- |
| branch `main`, with `params.libraries` in `hugo.yaml` | `https://go-approx.github.io/docs/<MAJOR.MINOR>/`, or `/docs/main/` |
| the newest version (else `main`) | also `https://go-approx.github.io/docs/latest/` |

`https://go-approx.github.io/docs/` redirects to `latest/`. Each push to
`main` replaces its version's directory on the `gh-pages` branch
(`scripts/publish-version.sh`) and keeps every other version. To document a new
release, set its tag in `params.libraries` and update the pages.

## Layout

| Path | What |
| --- | --- |
| `content/en/` | the pages: home, then one section per library |
| `hugo.yaml` | site configuration, `params.libraries` |
| `assets/css/custom.css` | the brand orange as Hextra's primary colour, the table headers |
| `layouts/_partials/` | favicons from the brand kit, the version selector, the outdated-version banner |
| `scripts/publish-version.sh` | publishes one build into the `gh-pages` checkout |
| `themes/hextra` | submodule: the Hextra fork |
| `branding` | submodule: [go-approx/brand](https://github.com/go-approx/brand) (the mark, its PNGs and ICO) |

## Build locally

```sh
git clone --recurse-submodules https://github.com/go-approx/docs.git
cd docs
hugo server
```

Hugo extended is not required; no Node.

## Workflows

| Workflow | What |
| --- | --- |
| `docs.yml` `build` | builds the site with `--panicOnWarning` and checks internal links and anchors offline |
| `docs.yml` `links` | every external link in the pages resolves |
| `docs.yml` `deploy` | on `main`, publishes the build to `gh-pages` |

BSD-3-Clause.
