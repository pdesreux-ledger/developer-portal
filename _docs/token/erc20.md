---
title: Adding a new ERC20 token to Ledger Live
subtitle:
tags: [featured]
toc: true
toc_sticky: true
author:
layout: doc_et
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

First, make sure your token isn’t already listed by checking the [repository](https://github.com/LedgerHQ/ledgerjs/tree/master/packages/cryptoassets/data).

The easiest way for you to request a token addition is to make a request to our [customer success team](https://support.ledger.com/hc/en-us/requests/new) and select Ledger Products - Technical Inquiries. Please make sure to share with us the details of your ERC20 token, and a link to Etherscan to allow us to double check the information.

### What will Ledger do ?

We will review the list of tokens on a bi-montly basis and will apply our own internal signature process. Once signed, the new tokens will become available after a Ledger Live update. Ledger reserves the right to decide which token will be listed.

### How do I add my token icon ?

Ledger Live relies on [this repository](https://github.com/LedgerHQ/ledger-live-common/tree/master/src/data/icons/svg) to manage coins and tokens icons. Please make a PR to the [repository](https://github.com/LedgerHQ/ledger-live-common/tree/master/src/data/icons/svg) to get your token icon visible on Ledger Live.

### Token on the Ledger support website

The list of supported assets on the [website](https://www.ledger.com/supported-crypto-assets/) is managed through an internal Ledger process. For the moment it’s difficult to guarantee the exact mapping between tokens available on Ledger Live and listed on the website. The true list of supported assets is visible [here](https://github.com/LedgerHQ/ledgerjs/tree/master/packages/cryptoassets/data).  We will soon improve our process so that the [website](https://www.ledger.com/supported-crypto-assets/) reflects exactly what is available on the Ledger Live software. In the meantime, the website list contains less tokens than the actual list.
