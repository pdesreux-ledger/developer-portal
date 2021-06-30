---
title: Adding *MyCoin* to Ledger Live Desktop and Mobile
subtitle:
tags: []
author:
layout: doc_ci
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

***

## Ledger Live Desktop

1. Fork [github.com/LedgerHQ/ledger-live-desktop](https://github.com/LedgerHQ/ledger-live-desktop)
2. Clone the repo

The clone will be used to test your modifications locally, using `yalc`.


### Adding the crypto in LLD

1. In  `live-common-set-supported-currencies.js` add the name you gave in the table. E.g., `mycoin`
2. Add the error translation keys in `static/i18n/en`.

### Running the Desktop projet

```sh
git clone <fork-live-desktop>
cd ledger-live-desktop
yalc add @ledgerhq/live-common
yarn install
```

If it is the first time you use `yalc` for live-common do this:

```sh
cd ledger-live-common
yalc publish
```

## Ledger Live  Mobile

1. Fork [github.com/LedgerHQ/ledger-live-mobile](https://github.com/LedgerHQ/ledger-live-mobile)
2. Clone the repo

The clone will be used to test your modifications locally, using `yalc`.

### Adding the crypto in LLM

1. Similarly step as for instructions for LLD, except it is in file `live-common-setup.js`.
2. Add the error translation keys in  `src/locales/en/common.json`

### Running the Mobile projet on iOS (Mac only)

The prerequisites to running the project are found in the README file.

```sh
git clone <fork-live-mobile>
cd ledger-live-mobile
yalc add @ledgerhq/live-common
yarn install
yarn run ios
yarn start
```

### Running the Mobile projet on Android

1. Launch the emulator or plug-in your telephone.
2. Use an Android environment such as Android Studio

```sh
git clone <fork-live-mobile>
cd ledger-live-mobile
yalc add @ledgerhq/live-common
yarn install
yarn run android
yarn start
```
