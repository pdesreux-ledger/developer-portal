---
title: Start here
subtitle: Using Speculos
tags: []
toc: true
toc_sticky: true
author:
layout: doc_sp
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

Speculos emulates Ledger Nano S, Nano X and Blue apps on
standard desktop computers, without any hardware device.

Usage example: `./speculos.py apps/btc.elf`.


## Limitations

The emulator handles only a few syscalls made by common apps; for instance,
syscalls related to app install, firmware update or OS info can't be
implemented.

There is absolutely no guarantee that apps will have the same behavior on
hardware devices and Speculos:

- Invalid syscall parameters might throw an exception on a real device while
  being ignored on Speculos.
- Attempts to perform unaligned accesses when not allowed (eg. dereferencing a
  misaligned pointer) will cause an alignment fault on a hardware device.


## Security

Apps can make arbitrary Linux system calls (and use QEMU
[semihosting](../semihosting) features), thus don't run Speculos on
untrusted apps.

It's worth noting that the syscall implementation (`src/`) doesn't expect
malicious input. By the way, in Speculos, there is no privilege separation
between the app and the syscalls. This doesn't reflect the security of the
firmware on hardware devices where app and OS isolation is enforced.
