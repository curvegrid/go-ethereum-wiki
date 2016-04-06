# Binaries

## Download stable binaries

All versions of Geth are built and available for download at https://build.ethdev.com/builds/Windows%20Go%20master%20branch/. The latest version is always available as [Geth-Win64-latest.zip](https://build.ethdev.com/builds/Windows%20Go%20master%20branch/Geth-Win64-latest.zip)

1. Download zip file
1. Extract geth.exe from zip
1. Open a command terminal
1. chdir <path to geth.exe>
1. open geth.exe

## ~~Installing the binary package from Chocolatey~~

_**Warning: Chocolatey is stuck at 1.2.2 and deprecated. Please use another method.**_

For master branch:
```
choco install geth-stable
```
_For more information see https://chocolatey.org/packages/geth-stable_

# Source

## Compiling geth with tools from chocolatey

The Chocolatey package manager provides an easy way to get
the required build tools installed. If you don't have chocolatey yet,
follow the instructions on https://chocolatey.org to install it first.

Then open an Administrator command prompt and install the build tools
we need:

```text
C:\Windows\system32> choco install git
C:\Windows\system32> choco install golang
C:\Windows\system32> choco install mingw
``` 

Installing these packages will set up the `Path` environment variable.
Open a new command prompt to get the new `Path`. The following steps don't
need Administrator privileges.

First we'll create and set up a Go workspace directory layout,
then clone the source.

```text
C:\Users\xxx> set "GOPATH=%USERPROFILE%"
C:\Users\xxx> set "Path=%USERPROFILE%\bin;%Path%"
C:\Users\xxx> setx GOPATH "%GOPATH%"
C:\Users\xxx> setx Path "%Path%"
C:\Users\xxx> go get github.com/tools/godep
C:\Users\xxx> mkdir src\github.com\ethereum
C:\Users\xxx> git clone https://github.com/ethereum/go-ethereum src\github.com\ethereum\go-ethereum
C:\Users\xxx> cd src\github.com\ethereum\go-ethereum
```

Finally, the command to compile geth is:

```text
C:\Users\xxx\src\github.com\ethereum\go-ethereum> godep go install -v ./...
```

## Powershell script for building with Cygwin

This script installs Go, MSYS2 and the development version of geth.

https://gist.github.com/tgerring/79f018954aadfb3f406e

## Compiling geth with tools from winbuilds

1. Install Git from http://git-scm.com/downloads
1. Install Golang from https://storage.googleapis.com/golang/go1.4.2.windows-amd64.msi
1. Install winbuilds from http://win-builds.org/1.5.0/win-builds-1.5.0.exe to `c:\winbuilds`
1. Run win builds here. It's safe to remove big dependencies like QT and GTK which aren't needed. _An exact list of dependencies should be determined_
1. Setup environment paths
  1. Add `GOROOT` pointed to `c:\go` and `GOPATH` to `c:\godev` (you are free to pick these paths).
  1. Set `PATH` to `%PATH%;%GOROOT%\bin;%GOPATH%\bin;c:\winbuilds\bin`
1. Open a terminal and install godep first: `go get -u github.com/tools/godep`
1. Open a terminal and download go-ethereum `go get -d -u github.com/ethereum/go-ethereum`
1. Try building ethereum with go dep, navigate to `c:\godev\src\github.com\ethereum\go-ethereum\cmd\geth` and run `git checkout develop && godep go install`

If you want to build from an other branch bypass `godep go install` for `go install` and checkout the dependencies manually.
