
# gv

`gv` is a Go version manager and wrapper implemented as a standalone shell script.

When used as a `go` wrapper `gv` looks for a `.goversion` file to choose the appropriate Go release. This file can contain a specific version (e.g. "go1.15.8") or one of the following symbolic release names: "stable" (latest [stable release](https://golang.org/dl/#stable)), "unstable" (latest [unstable release](https://golang.org/dl/#unstable)) or "gotip" (development tip).

When `gv` is invoked as `go` or `gofmt` it will delegate to the appropriate version of Go, first installing it if necessary.

## Installation

Download [`gv`](https://raw.githubusercontent.com/c2nes/gv/master/gv), mark it executable, and place it somewhere in `$PATH`,

``` shell
# Replace ~/bin with your bin directory of choice
curl -fsLO https://raw.githubusercontent.com/c2nes/gv/master/gv && chmod +x gv && mv gv ~/bin
```

If you are using `gv` as a wrapper, then create `go` and `gofmt` symlinks as well,

``` shell
# Replace ~/bin with your bin directory of choice
ln -sv ~/bin/gv ~/bin/go
ln -sv ~/bin/gv ~/bin/gofmt
```

:warning: **Warning:** Make there are no other copies of `go` / `gofmt` in your `$PATH`. If there are, remove them or ensure the copies pointing to `gv` come first.
