
# gv

`gv` is a simple Go version manager implemented as a standalone shell script.

`gv` is designed to be used as a Go wrapper. When used this way `gv` will automatically resolve, install, and delegate to the correct version of Go when invoked as `go` or `gofmt`.

This allows `gv` to seamlessly integrate with existing tools which call out to `go`.

Features,

* Project / directory specific versions via `.goversion` files.
* Version override via the `$GOVERSION` environment variable.
* Install and upgrade stable, unstable, and development (gotip) versions of Go.

## Installation

Download [`gv`](https://raw.githubusercontent.com/c2nes/gv/master/gv), mark it executable, and place it somewhere in `$PATH`,

``` shell
# Replace ~/bin with your bin directory of choice
curl -fsLO https://raw.githubusercontent.com/c2nes/gv/master/gv && chmod +x gv && mv gv ~/bin
```

If you plan to use `gv` as a wrapper, then create `go` and `gofmt` symlinks as well,

``` shell
# Replace ~/bin with your bin directory of choice
ln -sv ~/bin/gv ~/bin/go
ln -sv ~/bin/gv ~/bin/gofmt
```

:warning: **Warning:** Make there are no other copies of `go` / `gofmt` in your `$PATH`. If there are, remove them or ensure the copies pointing to `gv` come first.

## Usage

For basic usage simply use `go` and `gofmt` as usual. The latest stable Go release will be installed as the system default,

``` shellsession
$ go version
Downloading go1.15.8.linux-amd64.tar.gz ...
################################################################ 100.0%
Unpacking go1.15.8.linux-amd64.tar.gz ...
go version go1.15.8 linux/amd64

$ go version
go version go1.15.8 linux/amd64
```

## Upgrade Go

Use `gv -u` and `gv -U` to upgrade the "stable" and "unstable" releases respectively,

``` shellsession
$ gv -u
Downloading go1.15.8.linux-amd64.tar.gz ...
################################################################ 100.0%
Unpacking go1.15.8.linux-amd64.tar.gz ...
$ ~/sdk/stable/bin/go version
go version go1.15.8 linux/amd64
$ gv -U
Downloading go1.16rc1.linux-amd64.tar.gz ...
################################################################ 100.0%
Unpacking go1.16rc1.linux-amd64.tar.gz ...
$ ~/sdk/unstable/bin/go version
go version go1.16rc1 linux/amd64
```

## Set project/directory version

``` shellsession
$ go version
go version go1.15.8 linux/amd64
$ gv -s unstable
$ go version
go version go1.16rc1 linux/amd64
$ cat .goversion
unstable
$ gv -s go1.14
$ go version
go version go1.14 linux/amd64
```

## Set system version

``` shellsession
$ go version
go version go1.15.8 linux/amd64
$ gv -S unstable
$ go version
go version go1.16rc1 linux/amd64
$ readlink ~/sdk/system
unstable
```

## Use a version specific wrapper

``` shellsession
$ gv -i go1.15.3
Downloading go1.15.3.linux-amd64.tar.gz ...
################################################################ 100.0%
Unpacking go1.15.3.linux-amd64.tar.gz ...
$ go1.15.3 version
go version go1.15.3 linux/amd64
```

## Show `gv` help

```shellsession
$ gv -h
usage: gv [-uUisSh]
   or: go|gv <command> [arguments]
   or: gofmt [flags] [path ...]

gv is a Go version manager and wrapper

  -u
        Install or upgrade the stable release
  -U
        Install or upgrade the unstable release
  -i version
        Install the specified version (e.g. "go1.15.6", "gotip", etc)
  -s version
        Set (and install) the Go version for the current directory (i.e. update .goversion)
  -S version
        Set (and install) the system Go version (i.e. update ~/sdk/system symlink)
  -h
        Show this usage information
```
