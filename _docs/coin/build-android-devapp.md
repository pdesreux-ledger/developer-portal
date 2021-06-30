---
title: Building Android Dev App
subtitle:
tags: []
author:
layout: doc_ci
---


#### Sections in this article
{:.no_toc}
* TOC
{:toc}

<!-- 2021-04-07 written by Henri Ly -->


## Android Studio

[Download](https://developer.android.com/studio) and install Android Studio.

Go by default installation or custom it if you know what you are doing.

## Clone ledger-live-mobile repository and install it

Install dependencies using brew

```sh
brew install ruby watchman yarn
```

Then clone ledger-live-mobile, and cd into the folder.

```sh
git clone git@github.com:LedgerHQ/ledger-live-mobile.git
cd ledger-live-mobile
```

You can now use the yarn command.

```sh
yarn
```

Once it has finished, keep the terminal up and go back to Android Studio

## Creating an emulator

Go back to Android Studio and `Open an Existing Project`. The targeted folder is : `.../ledger-live-mobile/android`

It takes some time to load the project. During this time, create an emulator (or you can use your own android device).

Click the AVD Manager icon on the upper right of Android Studio :
<!-- ------------- Image ------------- -->
<div style="text-align:center">
<img width="120" src="../../../uploads/images/CI/avd_manager_icon.png" ></div>
<!-- --------------------------------- -->

Create the device that you want to have for test, don't go too low on API level or it may not work.

Then start the created device in the AVD manager.



## Run ledger-live-mobile in the emulator

Go back to your terminal, and add the Android sdk path.
Locate your SDK path, and add it to your env, the expected path is :
`export ANDROID_SDK_ROOT=/Users/<username>/Library/Android/sdk`

If you have some modification you want to test from your live-common you can

`yalc add @ledger-live/common` (if you have `yalc publish` your live-common)
and rerun
`yarn install`

Then you should be able to :

`yarn android` and it should be running on your created device

