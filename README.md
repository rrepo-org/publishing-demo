# hello.world release demo

This repository demonstrates publishing an R package release. The package is named `hello.world`; its current version is `0.1.1`. Releases are published to rrepo and can be installed using the commands below.

## Install the published package

The native rrepo repository is `https://rrepo.dev/rrepo/hello-world`. R tools that expect a CRAN-style repository use its mirror at `https://cran.rrepo.dev/rrepo/hello-world`.

### rpx

In a new project, add the native rrepo repository alongside rpx's default CRAN source:

```sh
rpx init --type project hello-world-demo
cd hello-world-demo
rpx repo add https://rrepo.dev/rrepo/hello-world
rpx add hello.world
```

### Base R

```r
install.packages(
  "hello.world",
  repos = c(
    rrepo = "https://cran.rrepo.dev/rrepo/hello-world",
    CRAN = "https://cloud.r-project.org"
  )
)
```

### rv (R package manager)

Start a new [rv](https://a2-ai.github.io/rv-docs/) project, register the CRAN-style mirror, and install the package from that repository:

```sh
rv init hello-world-demo
cd hello-world-demo
rv configure repository add rrepo --url https://cran.rrepo.dev/rrepo/hello-world
rv add hello.world --repository rrepo
```

### renv

In an R project with [renv](https://rstudio.github.io/renv/) installed:

```r
renv::init(bare = TRUE)
renv::install(
  "hello.world",
  repos = c(
    rrepo = "https://cran.rrepo.dev/rrepo/hello-world",
    CRAN = "https://cloud.r-project.org"
  )
)
```

After installation, check the exported function in R:

```r
hello.world::hello_world()
#> [1] "Hello, world!"
```

## How releases work

The package version lives in `DESCRIPTION`. Pushing a matching `v*` tag (for example, `v0.1.1`) triggers [the release workflow](.github/workflows/release.yml), which verifies the tag against `DESCRIPTION`, builds a source package, uploads it to rrepo, and creates a GitHub release with the source archive.
