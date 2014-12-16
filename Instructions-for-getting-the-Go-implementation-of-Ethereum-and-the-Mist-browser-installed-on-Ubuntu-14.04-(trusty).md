##Step 1 - A Clean Slate
It is not essential but it can be helpful for keeping everything tidy to have a separate installation for development purposes, though this is not essential, if you intend to do development work it may be that from time to time you have to start from scratch if there is a big change in the source, but also, from a known beginning you can follow these steps or modify them if necessary to account for any changes, and within about 45 minutes have Go 1.4 (or later), QT, the prerequisites for both, and on top of that you can then have a clean, tidy and orderly environment within which you can tinker with or develop the Go language Ethereum implementation.

For this, I decided to go with a standard USB startup disk for 14.04, with a 4gb persistence file. First step is to update, as at the writing of this guide about 400Mb is required to update all the packages to current. The update obviously can't change the boot environment but this isn't important for how this is done, and usually people will work from a proper install. I am doing it this way for clarity and for being able to document from scratch the setup process.

##Prerequisites
First, the prerequisites for installation. In this guide I will only discuss how to do this with the most current LTS version of Ubuntu, 14.04. I consider this to be the most accessible way for most people to get this up and running, I will leave other platforms to those who have or prefer to use them.

Go Ethereum depends on the secp256k1 C implementation, which means that you are required to have GMP headers installed:

    sudo apt-get install libgmp3-dev libreadline6-dev

##Enable all core software repositories
For anyone following precisely the process I am following to get this set up on a USB startup disk, you need to go through the software sources. This is called 'software and updates' in Ubuntu 14.04, you need to enable all of the by-default unselected categories of the repository. Once these repository branches are enabled you can install mercurial. Much the same will probably apply to a standard in-place installation of 14.04 as well, you may have enabled some of them in particular by selecting to install the installation of the mp3 codec and other 'nonfree' that will be a bit different from what is not enabled in a usb startup disk with a persistence file. There is no harm in having all of them enabled, though some are not necessary for this project, a blanket enabling of them all will ensure that everything is available.

In order to install a few of the other required packages, the mercurial revision control tool is needed:

    sudo apt-get install mercurial

##Installing Go 1.4 from Source
The next step is to install Go itself. Because I am of the opinion it is always better to use the most up to date version, since sooner or later the platform will move onto this anyway, and I know that Ethereum and the Mist browser work in this environment from previous installations, next step is to download and build the source of the Go 1.4 package. 

The archive of Go versions can be found here: http://golang.org/dl/ and for this guide I will instruct you to download and build Go from source. 

To download the 1.4 package, execute the following command in a shell:

    wget https://storage.googleapis.com/golang/go1.4.src.tar.gz
    tar zxvf go1.4.src.tar.gz
    cd go/src
    ./make.bash

This will then start the process of building and installing Go 1.4, which will take a few minutes.

I have downloaded and unpacked the go package to ~/go, and the binary for go will, after the build process, be found in ~/go/bin/ To enable you to perform the rest of the steps you will need to set up a number of paths and environment variables in your shell. I use the Bash shell, so the place to add the following commands is in ~/.bashrc - at the end is the best place, probably.

    export PATH=$PATH:~/go/bin:~/gopath/bin
    export GOROOT=~/go
    export GOPATH=~/gopath

You will need to create the directory ~/gopath. The go package will create the rest for you, the go binary path for built packages will end up in ~/gopath - the reason being is that you cannot set the gopath to be the same as the goroot, the go executable will refuse to do anything. This aspect of go configuration is not clear in the instructions but once the program flags the error you then have to make a solution. In my case, I have called the gopath, gopath, for simplicity.

I should note that both the ethereum wiki and the go wiki do not give clear and simple instructions for how to set up a go environment, it is somewhat arbitrary but I think the structure I have suggested is logical. There is also issues later on with setting working directories for the actual executables of Mist and Ethereum CLI, I will discuss this later.

In previous attempts I have been very ad-hoc as to how I do this but in this guide I am aiming to simplify and make sure every step is done in the most logical possible way. Everything will live within the 'go' folder which in my procedure I suggest you place in the root of your user profile folder, /home/<username>

Next, there is a whole collection of other prerequisites, mainly g++ and QT 5 that need to be installed. As described on this page: https://github.com/ethereum/cpp-ethereum/wiki/Building-on-Ubuntu#install-dependencies - these are the installation commands you will need to proceed from here:

If you have left off previously in this process, first get everything up to date:

    sudo apt-get update && sudo apt-get upgrade

then the following will pull down and install everything you need to finish the process:

    sudo apt-get install build-essential g++-4.8 git cmake libboost-all-dev
    sudo apt-get install automake unzip libgmp-dev libtool libleveldb-dev yasm libminiupnpc-dev libreadline-dev scons
    sudo apt-get install libncurses5-dev libcurl4-openssl-dev wget
    sudo apt-get install qtbase5-dev qt5-default qtdeclarative5-dev libqt5webkit5-dev
    sudo apt-get install libjsoncpp-dev libargtable2-dev

For completeness, I am giving everything required to build the CLI client and the GUI browser, if you only want the CLI, you won't need to install all the stuff to do with QT5, but I will leave that to as an exercise for the reader, my objective in this document is to enable one to build and install both. It will not harm to install these extras if you only want to install/work on the CLI, but this is mandatory if you want to see Mist running, or to be able to start working on it.

The other advantage of following the procedure I am describing, is that you will be seeing the latest, bleeding edge version of the Go client as hosted on github. Ethereum is not really yet in a state for a release anyway, though there is a PPA that can install all of this for you, I aim this guide to those who want to either just see the most up to date state of development, or want to participate in the development process. When the full release is out, it will be possible to get all this done with a few apt-get install commands.

We are still not ready to actually pull down and compile ethereum or go, the following commands must be executed to complete the process:

    sudo add-apt-repository ppa:ubuntu-sdk-team/ppa
    sudo apt-get update
    sudo apt-get install ubuntu-sdk qtbase5-private-dev qtdeclarative5-private-dev libqt5opengl5-dev

Note that the last command for me flagged that there was not a public key to verify the packages. I am not sure if the following command is necessary to enable you to pull and build the ethereum package or not, it was advised in the event of a dependency problem. I had this dependency problem but I think that was because I didn't do the previous installation first. If the previous commands fail, this will make it work:

    sudo apt-get install aptitude; sudo aptitude install ubuntu-sdk

##Installing Ethereum and Mist
At last you will now be finished with all the prerequisites. The following commands will build ethereum CLI and the Mist GUI client for you:

    go get -u github.com/ethereum/go-ethereum/ethereum

    go get -u -a github.com/ethereum/go-ethereum/mist

The other gotcha to be aware of is that mist does not automatically look in the right location for its GUI assets. for this reason you have to launch it using the following command:

    cd $GOPATH/src/github.com/ethereum/go-ethereum/mist && mist

To eliminate the need to remember this cumbersome command, I recommend you create the following file in $GOPATH/bin :

    #!/bin/bash
    cd $GOPATH/src/github.com/ethereum/go-ethereum/mist && mist

I called the file 'misted' and you have to make it executable so you don't have to call the shell to execute it:

    chmod +x $GOPATH/bin/misted

Enjoy playing with ethereum and mist, and I hope this guide will also help improve the accessibility of ethereum development to interested people. Every step in this could be automated, directly, as it is, and I will write this up into a script, but I suspect that there is unnecessary commands in here somewhere, this is just the ad-hoc solution that works based on all the information I found.