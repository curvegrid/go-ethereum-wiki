## Installing from Chocolatey

_**NOTE:** Not yet published. See https://github.com/tgerring/ethereum-chocolatey_

For master branch:
```
choco install geth-stable -pre
```

For develop branch:
```
choco install geth-stable -pre
```

## Building from source

1. Install Git from http://git-scm.com/downloads and Mercurial from http://mercurial.selenic.com/
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