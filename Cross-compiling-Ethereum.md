# Cross compiling Ethereum

Developers usually have a preferred platform that they feel most comfortable
working in, with all the necessary tools, libraries and environments set up for
an optimal workflow. However, there's often need to build for either a different
CPU architecture, or an entirely different operating system; but maintaining a
development environment for each and switching between the them quickly becomes
unwieldy.

Here we present a very simple way to cross compile Ethereum to various operating
systems and architectures using a minimal set of prerequisites and a completely
containerized approach, guaranteeing that your development environment remains
clean even after the complex requirements and mechanisms of a cross compilation.

The currently supported target platforms are:

 - 32 bit, 64 bit and ARMv5 Linux
 - 32 bit and 64 bit Mac OSX
 - 32 bit and 64 bit Windows

Please note, that cross compilation does not replace a release build. Although
resulting binaries can usually run perfectly on the desired platform, compiling
on a native system with the specialized tools provided by the official vendor
can often result in more a finely optimized code.

## Cross compilation environment

Although the `go-ethereum` project is written in Go, it does include a bit of C
code shared between all implementations to ensure that all perform equally well,
including a dependency to the GNU Multiple Precision Arithmetic Library. Because
of these, Go cannot by itself compile to a different platform than the host. To
overcome this limitation, we will use [`xgo`](https://github.com/karalabe/xgo),
a Go cross compiler package based on Docker containers that has been architected
specifically to allow both embedded C snippets as well as simpler external C
dependencies during compilation.

The `xgo` project has two simple dependencies: Docker (to ensure that the build
environment is completely contained) and Go. On most platforms these should be
available from the official package repositories. For manually installing them,
please consult their install guides at [Docker](https://docs.docker.com/installation/)
and [Go](https://golang.org/doc/install) respectively. This guide assumes that these
two dependencies are met.

To install and/or update xgo, simply type:

    $ go get -u github.com/karalabe/xgo

You can test whether `xgo` is functioning correctly by requesting it to cross
compile itself and verifying that all cross compilations succeeded or not.

    $ xgo github.com/karalabe/xgo
    ...

    $ ls -al
    -rwxr-xr-x 1 root     root  2003880 May 12 20:56 xgo-darwin-386
    -rwxr-xr-x 1 root     root  2496768 May 12 20:56 xgo-darwin-amd64
    -rwxr-xr-x 1 root     root  2030416 May 12 20:56 xgo-linux-386
    -rwxr-xr-x 1 root     root  2524536 May 12 20:56 xgo-linux-amd64
    -rwxr-xr-x 1 root     root  2082648 May 12 20:56 xgo-linux-arm
    -rwxr-xr-x 1 root     root  2176000 May 12 20:56 xgo-windows-386.exe
    -rwxr-xr-x 1 root     root  2686976 May 12 20:56 xgo-windows-amd64.exe

## Building Ethereum

Cross compiling Ethereum is analogous to the above example, but a few additional
flags are required to satisfy the dependencies as well as select the specific
command we would like to build:

 - `--deps` is used to inject arbitrary C dependency packages and pre-build them
 - `--pkg` is used to select a sub-package if not the root is being built

Injecting the GNU Arithmetic Library dependency and selecting `geth` would be:

    $ xgo --deps=https://gmplib.org/download/gmp/gmp-6.0.0a.tar.bz2 \
          --pkg=cmd/geth                                            \
          github.com/ethereum/go-ethereum
    ...

    $ ls -al
    -rwxr-xr-x 1 root     root  12946188 May 12 21:07 geth-darwin-386
    -rwxr-xr-x 1 root     root  15407084 May 12 21:07 geth-darwin-amd64
    -rwxr-xr-x 1 root     root  17611290 May 12 21:07 geth-linux-386
    -rwxr-xr-x 1 root     root  20780686 May 12 21:07 geth-linux-amd64
    -rwxr-xr-x 1 root     root  16946493 May 12 21:07 geth-linux-arm
    -rwxr-xr-x 1 root     root  17418752 May 12 21:07 geth-windows-386.exe
    -rwxr-xr-x 1 root     root  20345856 May 12 21:07 geth-windows-amd64.exe

As the cross compiler needs to build all the dependencies as well as the main
project itself for each platform, it may take a while for the build to complete
(approximately 3-4 minutes on a Core i7 3770K machine).

### Fine tuning the build

By default Go, and inherently `xgo`, checks out and tries to build the master
branch of a source repository. However, more often than not, you'll probably
want to build a different branch from possibly an entirely different remote
repository. These can be controlled via the `--remote` and `--branch` flags.

To build the `develop` branch of the official `go-ethereum` repository instead
of the default `master` branch, you just need to specify it as an additional
command line flag (`--branch`):

    $ xgo --deps=https://gmplib.org/download/gmp/gmp-6.0.0a.tar.bz2 \
          --pkg=cmd/geth                                            \
          --branch=develop                                          \
          github.com/ethereum/go-ethereum

Additionally, during development you will most probably want to not only build
a custom branch, but also one originating from your own fork of the repository
instead of the upstream one. This can be done via the `--remote` flag:

    $ xgo --deps=https://gmplib.org/download/gmp/gmp-6.0.0a.tar.bz2 \
          --pkg=cmd/geth                                            \
          --remote=github.com/karalabe/go-ethereum                  \
          --branch=rpi-staging                                      \
          github.com/ethereum/go-ethereum

### Cross compiler notes

Currently `xgo` works with public repositories and import paths only. This means
that in order to cross compile your local modifications to different platforms,
you first must push it to a publicly fetchable URL (e.g. your GitHub fork of the
project).

Currently `xgo` does not feature a way to specify a single target platform for
which to build to, and rather will cross compile to all supported architectures
and operating systems. This may change in a future release, but for now it's good
to be aware of.
