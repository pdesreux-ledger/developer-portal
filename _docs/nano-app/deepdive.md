---
title: Deepdive into the script
subtitle: Where's the On switch?
tags: []
toc: true
toc_sticky: true
author:
layout: doc_na
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

In this article we will explain how the script available in the [quickstart](../quickstart) works. You can skip this part if you don't want to go into more details.

We will provide a general tutorial for getting your BOLOS development environment set up, followed by some detailed explanations of the various components of the BOLOS SDKs and what userspace development entails. It is assumed that you have already read the [BOLOS Platform](../bolos-introduction) appendix and are somewhat familiar with the BOLOS architecture.


## Introduction

<!--  -->
{% include alert.html style="warning" text="Only Linux is supported as a development OS. For Windows and MacOS users, a Linux VM is recommended." %}
<!--  -->

Developing and / or compiling BOLOS applications requires the SDK matching the appropriate device (the Nano S or X SDK) as well as the following two compilers:

-   A standard ARM gcc to build the non-secure (STM32) firmware and link the secure (ST31) applications
-   A standard ARM clang (latest version) with [ROPI support](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.dui0491i/CHDCDGGG.html) to build the secure (ST31) applications, download it [here](https://releases.llvm.org/9.0.0/clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04.tar.xz)
-   Download a prebuilt gcc from [here](https://developer.arm.com/-/media/Files/downloads/gnu-rm/10-2020q4/gcc-arm-none-eabi-10-2020-q4-major-x86_64-linux.tar.bz2?revision=ca0cbf9c-9de2-491c-ac48-898b5bbc0443&la=en&hash=68760A8AE66026BCF99F05AC017A6A50C6FD832A)

## Setting up the Toolchain

The Makefiles used by our BOLOS applications look for gcc and clang installations using the `PATH` environment variable.

If you don't want to install specific versions of clang and gcc directly on your system, simply prepend their location in your `PATH` environment variable.

``` bash
## GCC
PATH=~/bolos-devenv/gcc-arm-none-eabi-10-2020-q4-major-linux/bin:$PATH

## Clang
PATH=~/bolos-devenv/clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04/bin:$PATH
```

Cross compilation headers are required and provided within the gcc-multilib and g++-multilib packages. To install them on a debian system:

``` bash
sudo apt install gcc-multilib g++-multilib
```

If you wish to load applications on your device, you will also need to add the appropriate `udev` rules.

``` bash
wget -q -O - https://raw.githubusercontent.com/LedgerHQ/udev-rules/master/add_udev_rules.sh | sudo bash
```

## Setting up the SDK

Now that you have your toolchain set up, you need to download / clone the SDK for the appropriate Ledger device you're working with. Make sure you checkout the tag matching your firmware version.

Ledger Nano S SDK: <https://github.com/LedgerHQ/nanos-secure-sdk>

Ledger Nano X SDK: <https://github.com/LedgerHQ/nanox-secure-sdk>

Finally, link the environment variable `BOLOS_SDK` to the SDK you downloaded.

``` bash
BOLOS_SDK='/path/to/sdk/'
```

When using the Makefile for our BOLOS apps, the Makefile will use the contents of the SDK to determine your target device ID (Ledger Nano S or X). Even if you aren't building an app, loading an app with the Makefile still requires you to have the SDK for the appropriate device linked to by `BOLOS_SDK`.

## Python Loader

Most apps use the Python loader, a Ledger-made Python library to communicate with Ledger devices. You can install it with `pip install ledgerblue`

