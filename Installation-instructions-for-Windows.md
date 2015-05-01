## Building from source

1. Install Git and Mercurial
1. Install Golang from https://storage.googleapis.com/golang/go1.4.2.windows-amd64.msi
1. Install winbuilds from http://win-builds.org/1.5.0/win-builds-1.5.0.exe to c:\winbuilds
1. Run win builds here I just remove a few big dependencies like QT and GTK which I know aren't needed an exactly list would be preferable but you can't remove deps after so it's hard to track down.
1. Setup environment paths add GOROOT to c:\go and GOPATH to c:\godev (you are free to pick this one) setup PATH to %PATH%;%GOROOT%/bin;%GOPATH%/bin;c:\winbuilds\bin
1. Open a terminal and install godep first: go get -u github.com/tools/godep
1. Open a terminal and install ethereum go get -d -u github.com/ethereum/go-ethereum/geth
1. Try building ethereum with go dep, navigate to c:\godev\src\github.com\ethereum\go-ethereum\gethand issue git checkout develop and godep go install.

If you want to build from an other branch bypass godep go install for go install and checkout the dependencies manually.