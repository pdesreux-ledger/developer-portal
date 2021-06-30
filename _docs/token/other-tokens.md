---
title: Adding a new TRC10/20 or ASA token to Ledger Live
subtitle:
tags: []
toc: true
toc_sticky: true
author:
layout: doc_et
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

Adding an ERC20 token (Ethereum) is pretty straightforward, but it might be more difficult for other blockchains. For instance, on Algorand, both the Nano applications and the LedgerJS repository needs to be updated for Ledger Live to support a token.
This is due to the fact that tokens details are hardcoded within the Nano application instead of being streamed to the device.

## Adding your ASA token to Ledger live

To add your ASA you need to

- update the Nano app [github.com/LedgerHQ/app-algorand/blob/master/src/algo_asa.c](https://github.com/LedgerHQ/app-algorand/blob/master/src/algo_asa.c)
- update the ledgerjs package [github.com/LedgerHQ/ledgerjs/blob/master/packages/cryptoassets/data/asa.js](https://github.com/LedgerHQ/ledgerjs/blob/master/packages/cryptoassets/data/asa.js)

To ensure both these topics are handled properly, we invite you to contact our engineering team through the [Ledger's Developer Slack](https://ledger-dev.slack.com).

## Adding your TRC10/20 token to Ledger live

Work in progress
