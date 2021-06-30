---
title: Debug - how to use GDB
subtitle:
tags: []
author:
layout: doc_sp
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}


## Debug an app thanks to GDB:

```sh
./speculos.py -d apps/btc.elf &
./tools/debug.sh apps/btc.elf
```

## Some useful tricks:

- Use the `-t` (`--trace`) argument to trace every syscalls.
- [Semihosting](../semihosting) features can be used as an additional debug mechanism.
