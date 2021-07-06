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

[Ledger App Builder](https://github.com/LedgerHQ/ledger-app-builder) is a container image which contains all dependencies to compile an application for Nano S/X. Please follow the instructions in the README to download it.

## Loading Apps on a Device

If you wish to load applications on your device, you will also need to add the appropriate `udev` rules.

``` bash
wget -q -O - https://raw.githubusercontent.com/LedgerHQ/udev-rules/master/add_udev_rules.sh | sudo bash
```

