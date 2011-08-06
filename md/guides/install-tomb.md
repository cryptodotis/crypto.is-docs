# Tomb Installation Guide

## Ubuntu

### Install Tomb from Source

First you must have git installed, use this [Github guide](http://help.github.com/linux-set-up-git/).  Once you have git installed, pull the source from The Crypto Project repostitory.

    git clone git://github.com/cryptodotis/Tomb.git

Next, you will need to install the following packages to build Tomb: 

    sudo apt-get install build-essential autoconf libtool gtk2.0-dev libnotify-dev zsh pinentry-curses pinentry-gtk2

Next, go into the Tomb source directory, configure, build, then install the Tomb.

    cd Tomb
    autoreconf -i
    ./configure
    make && sudo make install

### Install from Debian Repository

First, add the following lines to the bottom of your */etc/apt/sources.list* file to add the Dyne repository:

    deb http://apt.dyne.org/ubuntu dyne main
    deb-src http://apt.dyne.org/ubuntu dyne main

Next, you must add the signing key to your keyring, then add that keyring to *apt*:

    wget http://apt.dyne.org/software.pub
    gpg --import software.pub
    sudo apt-key add ~/.gnupg/pubring.gpg

Now you should be able to run an update.

    sudo apt-get update

And then install Tomb simply by running:

    sudo apt-get install tomb
