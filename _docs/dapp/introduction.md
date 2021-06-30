---
title: Integrating DApps
subtitle:
tags: []
toc: true
toc_sticky: true
author:
layout: doc_da
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

## Introduction

By having your DApp supported in Ledger DApp browser, you increase the security and ease of use for your users, while simultaneously having access to over 3 million Ledger Live users.

Main steps to easily turn your DApp into a Live DApp:

1. Web3provider actions
2. Adapt your code and test it
3. Build your Nano Application Plugin
4. Submit your DApp for review

## How-to

Ledger has a [DApp browser](https://github.com/LedgerHQ/ledger-live-platform-apps) to host your DApp.

One of the most useful features of this DApp Browser is to log-in to the user automatically from their Ledger Live Ethereum account(s), therefore removing the step to manually “Connect Wallet”.

As a DApp developer, you must add the specific iframe-provider from Ledger to your web3 provider list.

You’ll find below the steps needed to have your application compatible with Ledger Live DApp Browser.



### Add a new web3 provider

The package is available here [npmjs.com/package/@ledgerhq/iframe-provider](https://www.npmjs.com/package/@ledgerhq/iframe-provider). This simply diverts all RPC calls made from the DApp to the parent window using the PostMessage API. It does not do any Ledger-specific computing. It could in the future become a standard, to work out-of-the-box with other wallets.


### Server prerequisite

The server must allow DApps to be embedded in iframes. To know more on how to proceed, have a look at [this documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors).

You can easily authorize your DApp to be embedded only by Ledger Live DApp Browser by specifying the DApp Browser URL.

[Learn more](https://blog.ledger.com/paraswap-defi/) about the rationale behind this design choice.


### Automatic instancing

The DApp must run differently if it is called from within an iframe or not. If it is, directly initialize the web3 instance with the iframe-provider. This way, there is no web3modal connect flow at all, and users are instantly connected to the wallet of the parent window. To detect  if a call is made from within an iframe or not, use  a query param such as  `?embed=true`.



### Testing locally
A tool is available to test the provider integration. It is a DApp browser empty shell that uses mock data. To use it, put your URL in the

`https://ledger-live-platform-apps.vercel.app/app/dapp-browser?mock=true&url=<Your_URL>`

For example, this is how it is used with Paraswap:

[https://ledger-live-platform-apps.vercel.app/app/dapp-browser?mock=true&url=https://paraswap.io?embed=true](https://ledger-live-platform-apps.vercel.app/app/dapp-browser?mock=true&url=https://paraswap.io?embed=true)


Note the page information content depends on the chosen account in the drop-down list in the page header.

<!--  -->
{% include alert.html style="success" text="The first requirement is that the DApp must work flawlessly within this test tool." %}
<!--  -->


### UI/UX optimization

- Please make sure to handle some basic edge cases.
  For instance, what happens when you click on the “log out” or “disconnect” button in your Dapp. Will it be automatically logged back in the Ledger Live iframe-provider, or redirected to the standard list of providers (Metamask, connect wallet, and such…) ?
- We currently have limited blockchain support. In the event your Dapp supports several EVM compatible blockchain (like Polygon or BSC), please remove the network selection option to only allow users to use the Ethereum network. We are in the process of adding other networks, but this will take some time.
- Please remove any references to third party services that are not part of the Ledger ecosystem. You are free however to redirect to Ledger Live Buy or Swap services.
- UI must be responsive and available for both Desktop and Mobile screen sizes.


### Nano Application Plugin

The security model Ledger Nano devices rely on is “What you see is what you sign”.
The end user must verify and approve a transaction, by carefully verifying the information displayed on the Nano screen, before applying the signature.

The purpose of the Nano (BOLOS) application is specifically to handle the parsing and displaying of transaction details to the end user.

We refer to blind signing when the user is not able to properly verify the transaction details.
This is often the case when users interact with smart contracts. Indeed, as each smart contract interaction is different, there is no easy way for the BOLOS application to parse the message to sign easily.

To solve this problem with a scalable approach, Ledger designed the concept of _plugin_. Plugins are some highly specific applications handling dedicated smart contract methods, therefore turning unverifiable contract data into humanly verifiable details on the Nano screen.

<!-- ------------- Image ------------- -->
<div style="text-align:center">
<img width="680" src="../images/plugin.png">
</div>
<!-- --------------------------------- -->

Ledger designed and implemented the first Ethereum plugin : Paraswap.

<video controls muted preload='none' poster='../images/paraswap.png' ><source src="../../../uploads/videos/DA/paraswap.mp4" type='video/mp4'></video><br>

You’ll find the relevant documentation on how to make your own plugin [here](https://github.com/LedgerHQ/app-ethereum/blob/named-external-plugins/doc/ethapp_plugins.asc).

<!--  -->
{% include alert.html style="success" text="The second mandatory requirement to get your DApp officially supported by Ledger is using a plugin to allow the user to verify the transaction details on the Nano device." %}
<!--  -->


## Release Process


Dapp will be made available under two separated categories:

1. Officially supported
2. Experimental Dapp

The Dapps deployed under the Officially supported section will be made available to any Ledger Live users. They will also benefit from Ledger marketing strategy (website dedicated page, educational content, podcast and more).

The Dapps deployed under the Experimental Dapp section are hidden by default. They are mainly used for testing purposes. To get access to this section, a specific setting needs to be toggled under Ledger Live setting, and the user must consent to the security risk associated with the use of such a feature. Ledger will never promote or advertise these Dapps.


## Contact us

If you want to know more, or simply get in touch, reach out to us on [Slack](https://join.slack.com/t/ledger-dev/shared_invite/zt-iskfi3kl-CXw9Uz2dOOYSLKe_e4tcmw). You may also submit your Dapp and Nano application plugin [here](https://forms.gle/JP7qMQUBh4pSe77w9).
