---
layout: post
title:  "InstallToyBitcoinMiner"
date:   2018-07-10 12:34:56 +1000
blurb:  "Programmable money."
categories: bitcoin
---

# Linux VM

I used Slushpool or ckpool to mine solo, Ubuntu running via VirtualBox, and cgminer.

Setup an account on slushpool.com. The pool's url, your username and password are requested by cgminer when it starts.

VirtualBox running Ubuntu 14.04
Under Settings > Ports I added a USB port; USB 3.0, added the GekkoScience USB miner (plug it in)

Then in terminal:

1. Get a bunch of libraries:
```
sudo apt-get install -y build-essential git autoconf automake libtool pkg-config libcurl4-openssl-dev libudev-dev libc6-dev libncurses-dev libusb-1.0
```

The last three I added to original instructions as when I tried to build cgminer I was missing C headers - the compiler was complaining - and some other errors recommended I install the last two libraries there. i.e. read any error messages carefully. You might have to install some other libraries.

2. Get cgminer from github:
```
    mkdir -p git/vthoang; cd git/vthoang
    git clone https://github.com/vthoang/cgminer.git
    cd cgminer
```
    Note that this version has the drivers for the Gekko miner.

3. Build cgminer, including the gekko drivers (important):
```
    CFLAGS=”-O2 -march=native” ./autogen.sh --enable-gekko
    make -j 2
```
(that's the letter 'O' on the CFLAGS="-O2... line there, not the number zero)

4. Boot it up:
```
    sudo ./cgminer -o stratum+tcp://sg.stratum.slushpool.com:3333 -u your_user_name
```
    or
```
    sudo ./cgminer -o stratum_tcp://solo.ckpool.org:3333 -u 1spMAApJZV7q69pATUB1CNP11LZ6ejWV6 -p x
```
sudo because it was complaining about not having access to USB unless root.

ta-fucking-da.

# MacOS / OSX

A few extra step to get it running native in MacOS.

Install [Homebrew](https://brew.sh).

In terminal install some libraries:

```
Brew install libusb
Brew install pkg-config
Brew install libtool
Brew install autoconf
Brew install automake
```
Then build the miner.
```
CFLAGS="-O2 -march=native" ./autogen.sh --enable-gekko
Make -j 2
```

Finally, boot up the miner.

```
sudo ./cgminer -o stratum+tcp://sg.stratum.slushpool.com:3333 -u your_user_name
```
