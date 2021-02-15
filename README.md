
# gv

`gv` is a simple Go version manager implemented as a standalone shell script.

`gv` is designed to be used as a Go wrapper. When used this way `gv` will automatically resolve, install, and delegate to the correct version of Go when invoked as `go` or `gofmt`. This allows `gv` to seamlessly integrate with existing tools which call out to `go`.

`gv` supports three symbolic versions: "system", "stable", and "unstable". The system version acts as the default version for all commands. It itself defaults to "stable", but can be configured otherwise via `gv -S <version>`. The stable and unstable releases correspond to the releases found on https://golang.org/dl/ and can be updated via `gv -u` and `gv -U` respectively.

Other features,

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
$ gv -S unstable
$ go version
go version go1.16rc1 linux/amd64
$ readlink ~/sdk/system
unstable
$ gv -s go1.14
$ readlink ~/sdk/system
go1.14
```

## Install a specific version

``` shellsession
$ gv -i go1.15.3
Downloading go1.15.3.linux-amd64.tar.gz ...
################################################################ 100.0%
Unpacking go1.15.3.linux-amd64.tar.gz ...
$ ~/sdk/go1.15.3/bin/go version
go version go1.15.3 linux/amd64
```

## Use a version-specific wrapper

``` shellsession
$ go1.15.3 version
go version go1.15.3 linux/amd64
```

## Check the configured version

The current version,

``` shellsession
$ go version
go version go1.15.8 linux/amd64
```

The unstable version,

``` shellsession
$ GOVERSION=unstable go version
go version go1.16rc1 linux/amd64
```

The system (i.e. default) version,

``` shellsession
$ GOVERSION=system go version
go version go1.15.8 linux/amd64
```

## Install the development version of Go

``` shellsession
$ gv -i gotip
Cloning into '/home/chris/sdk/gotip'...
remote: Counting objects: 10336, done
remote: Finding sources: 100% (10336/10336)
remote: Total 10336 (delta 1335), reused 6667 (delta 1335)
Receiving objects: 100% (10336/10336), 24.24 MiB | 1.21 MiB/s, done.
Resolving deltas: 100% (1335/1335), done.
Updating files: 100% (9480/9480), done.
Updating the go development tree...
From https://go.googlesource.com/go
 * branch            master     -> FETCH_HEAD
HEAD is now at 33d72fd doc/faq: update generics entry to reflect accepted proposal
Building Go cmd/dist using /home/chris/sdk/go1.15.8. (go1.15.8 /amd64)
Building Go toolchain1 using /home/chris/sdk/go1.15.8.
Building Go bootstrap cmd/go (go_bootstrap) using Go toolchain1.
Building Go toolchain2 using go_bootstrap and Go toolchain1.
Building Go toolchain3 using go_bootstrap and Go toolchain2.
Building packages and commands for linux/amd64.
---
Installed Go for linux/amd64 in /home/chris/sdk/gotip
Installed commands in /home/chris/sdk/gotip/bin
Success. You may now run 'gotip'!
$ GOVERSION=gotip go version  # or "gotip version"
go version devel +33d72fd Mon Feb 15 01:54:22 2021 +0000 linux/amd64
```

## Show `gv` help

```shellsession
$ gv -h
usage: gv [-uUisSh]
   or: gv -c cmd [arguments]
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
  -c cmd
        Run the given command instead of delegating based on $0
  -h
        Show this usage information
```
