---
title: HD Key Generation
subtitle: The children of infinite trees
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

## Introduction

In [the previous section](../bg_master_seed) we discussed how Ledger devices use a master 24-word mnemonic seed to derive a theoretically infinite number of cryptographic secrets. Though it might seem impossible at first glance, this can be done using nothing more than some mathematical sorcery. The process used to do this is called hierarchical deterministic (HD) key generation.

The process for HD key generation used by all Ledger devices (and many other HD wallets) is defined by [BIP 32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki).

Here's how it works.

## The Master Node

Hierarchical deterministic key generation involves creating a theoretically infinite tree of cryptographic secrets. The root of this tree from which everything is generated is called the master node. The master node is derived directly from the master binary seed described in the previous section using HMAC-SHA512. The master node of your wallet is all the information you need to access an infinite hierarchy of private keys. As such, you should take good care to keep your mnemonic seed safe.

### Passphrases

Passphrases are a standard feature of BIP 39 supported by many HD wallets. Passphrases are an optional way of adding additional data to the master seed before deriving the master node of the HD wallet. By specifying a different passphrase (or no passphrase at all), the value of the master node is completely changed. As you will discover as you read the rest of this section, changing the master node of the HD wallet completely changes all of the information derived from it (for example, your Bitcoin addresses). In this way, you can keep the same master seed on a device, but use different passphrases to access completely unique, separated wallets.

## An Infinite Tree

Descending from the master node is a tree of an infinite number of private keys. All you need to determine a particular key is the master node (your mnemonic seed) and the location of the key in the tree (called its "path"). This tree is created using an incredibly powerful algorithm defined by BIP 32 called the CKD function. All you need to know about this magical algorithm is that it can be used to calculate any node on the HD tree given the node's position and the value of the master node. Essentially, the CKD function is applied to the master node a number of times in order to "scramble" its bits and output a brand new node. This algorithm is magical because **it is impossible to reverse**. Given a private key on your tree, it is impossible to go backwards up the tree and determine your master node.[^1]

## Child Key Derivation Function

The HD tree is made up of "nodes". Using the CKD function, many "child" nodes can be derived from a single "parent" node. Each node contains three pieces of information: a private key, a public key, and a chain code. In the case of a cryptocurrency wallet, the private key is the part that is used to sign transactions and the public key is used to generate the corresponding cryptocurrency address. The node's chain code is an extra little bit to prevent someone from determining the children of a node using only the node's public and private keys.

The CKD function has been designed in a way that provides great flexibility in the way child nodes are derived. Specifically, instead of requiring the public key, private key, and chain code to derive a child node, the CKD function can be used to derive child public keys from the parent public key and child private keys from the parent private key. Additionally, BIP 32 defines the concept of "hardened" child nodes. If a child node is "hardened", then its public key cannot be determined from its parent node's public key.

These are the transformations that the CKD function can perform:
-   parent private key & chain code ➞ child private key & chain code
-   parent public key & chain code ➞ child public key & chain code (unless the child node is hardened)
-   parent private key & chain code ➞ child public key & chain code

Note that it is impossible to derive a child private key from a parent public key. This model has some interesting consequences, mainly that the parent public key and chain code can be shared which allows someone to derive all child public keys (unless the child nodes are hardened), but no private keys.

For example, this model can be used to facilitate the process of auditing an HD wallet. By sharing the public key and chain code of all accounts in an HD wallet, one can give an auditor the ability to view all addresses in the wallet (without having the associated private keys). As such, the auditor could determine what money is going where, without having access to any private keys, meaning the auditor couldn't sign any transactions. We'll talk about what "accounts" are in the next section.

For a more detailed list and explanation of some of the use-cases made possible by this model, see [this section](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#use-cases) of the BIP 32 specification.

In the next section, `hd_use_cases`, we'll talk about how all of these features are used by HD wallets and other applications.

<!--  -->
{% include alert.html style="primary" text="If you'd like to play with BIP 39 mnemonics or BIP 32 derivation on a computer, take a look at this tool: <a href='https://iancoleman.io/bip39/' class='alert-link'> iancoleman.io/bip39 </a> " %}
<!--  -->

**Footnotes**

[^1]: Technically it isn't impossible to determine a node given the child node and the corresponding index, but there is no known attack to do this faster than a properly executed brute-force attack. See [this section of BIP 32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#security) for details.

