---
title: Application Debug
subtitle: Making testing easier
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


<!--  -->
{% include alert.html style="warning" text="The <a href='../deepdive/' class='alert-link'>BOLOS development environment</a> is required for the following article. It applies only for the Nano S, with its SE (Secure Element) firmware either in version 1.5.5 or 1.6.0." %}
<!--  -->


It is possible to install a debugging firmware on the device's MCU (Microcontroller) that will enable printing text outputs from the device to a terminal. To do so, follow these steps:

1. Download the [updater](https://drive.google.com/open?id=1pbqIDDuamfsvFuEkduCyOFq8mW0HZmeQ) and the [debug firmware](https://drive.google.com/open?id=1hTZKqlwKjx51vdqda8SRp_80Yx3lPizb) and install the Python Loader with `pip install ledgerblue`.

2. Exit any instance of Ledger Live or any other program able to communicate with a Ledger device.

3. Plug your Nano S to your computer while keeping the left button pressed. Keep it pressed until the screen displays `BOOTLOADER`.

4. Open a terminal and move to the directory containing the files downloaded at step 1.

5.  Install the updater (only if you MCU firmware is not already in version 1.11, otherwise just go to step 6):

    ```sh
    python3 -m ledgerblue.loadMCU --targetId 0x01000001 --fileName blup_0.11_misc_m1.hex --nocrc
    ```

    Wait until `BOOTLOADER` is displayed again on the device's screen.

6.  Install the debugging firmware:

    ```sh
    python3 -m ledgerblue.loadMCU --targetId 0x01000001 --fileName mcu_1.11-printf_over_0.11.hex --reverse --nocrc
    ```

If you can notice a small `dbg` block at the bottom of the screen, it's a success !

<!-- ------------- Image ------------- -->
<!-- --------------------------------- -->
<figure>
<img src="../images/debug_nano.jpg" class="align-center" alt="A Nano S with the debugging firmware" /><figcaption aria-hidden="true">A Nano S with special debug firmware</figcaption>
</figure>

Uninstalling this debugging firmware is also very easy:
1. Download the [normal firmware](https://drive.google.com/open?id=1YfdU1dNycojdtuKU_hHctLFzJZzhDFuY).
2. Repeat the installation from step 2 to 5.
3. Flash the normal firmware with this command:
    ```sh
    python3 -m ledgerblue.loadMCU --targetId 0x01000001 --fileName mcu_1.11_over_0.11.hex --reverse --nocrc
    ```

The `dbg` block should now be gone.

## PRINTF macro

The debugging firmware enables the `PRINTF` macro, however you have to define it in your app's Makefile. To do so, add this line in your Makefile: `DEFINES += HAVE_SPRINTF HAVE_PRINTF PRINTF=screen_printf`

Usually, `PRINTF` is already defined to void, with this line `DEFINES += PRINTF\(...\)=`. Check if `PRINTF` is already defined somewhere else in your Makefile, and comment out this definition so it doesn't override the one that you just set.

<!--  -->
{% include alert.html style="warning" text="The <code>PRINTF</code> macro is a debugging feature, it is not intended to be used in production. When compiling an application for release purpose, please verify that <code>PRINTF</code> is disabled in your Makefile: put back the line <code>DEFINES += PRINTF\(...\)=</code> and comment out the other one." %}
<!--  -->

<!--  -->
{% include alert.html style="warning" text="The <code>PRINTF</code> macro can only be used in between successive calls to <code>io_exchange</code>. Calling it outside of <code>io_exchange</code> will result in unexpected behaviour. <code>PRINTF</code> sends a status on the <a href='../bolos-hardware-architecture/#seproxyhal' class='alert-link'> SEPH </a>. Only one status can be sent in a row, otherwise the SEPH crashes. For this reason, don't use <code>PRINTF</code> just after status-sending calls, such as <code>UX_DISPLAY</code>. This macro packs a call to <i>io_seproxyhal_display</i> and is often the reason for application crashes. The only solution is to move your call to <code>PRINTF</code> somewhere else in your code." %}
<!--  -->


This macro can be used in your code like a classical `printf` function from `stdio.h`. However, it is improved with a neat feature to easily print byte arrays:

``` c++
uint8_t buffer[4] = {0xDE, 0xAD, 0xBE, 0xEF};

// PRINTF(string, array length, array);
// .*H for uppercase, .*h for lowercase
PRINTF("What a lovely buffer:\n %.*H \n\n", 4, buffer);
PRINTF("I prefer it lower-cased:\n %.*h \n", 4, buffer);
```

<!-- ------------- Image ------------- -->
<!-- --------------------------------- -->
<figure>
<img src="../images/deadbeef.png" class="align-center" alt="Result of the example code printed inside a terminal" /><figcaption aria-hidden="true">Result of the example code printed inside a terminal</figcaption>
</figure>

## Console Printing

The `PRINTF` macro triggers messages from the MCU, to the host computer, through the USB link. [USBTool](https://drive.google.com/open?id=16D5vlrbczmBxqpDJml6QUV0RGWs7aZeZ) reads these messages and prints their payload in a terminal.

1. Download [USBTool](https://drive.google.com/open?id=16D5vlrbczmBxqpDJml6QUV0RGWs7aZeZ)
2. Unzip the file and execute this command: `./usbtool -v 0x2c97 log`

When you launch your app on your Nano S, every `PRINTF` will be printed on the host computer, allowing you to debug your program more easily.

## PIN bypass

In Ledger app development, it is necessary to enter your PIN code each time you install an unsigned app. However, BOLOS supports installing a custom developer certificate. With a custom certificate, you can avoid having to retype your PIN each time you adjust your app. Here are the steps for the Ledger Nano S:

1.  Generate a public / private keypair using the following command:

        foo@bar:~$ python3 -m ledgerblue.genCAPair
        Public key : 0495331cb86e961fc9cb5792a97737e4204b58be99321dcec463cec3057b3304e9875614004e6e540ab0610a1339fae22df6f6f3ec594912b409d69b72f0eaf390
        Private key: 6c1f1df852255e113b23c2e977d6b5c3ea2aaf411f05a5fdab87a3e8f44468ee

2. Enter in recovery mode on your Ledger Nano S. Do this by unplugging it then holding down the right button (near the hinge, away from USB port) while plugging it in again. `recovery mode` should then appear on the screen. Enter your pin and continue.

3. Load your public key onto the Ledger Nano S. Paste the public key generated at step 1 after `--public`. You may need to adjust the `--targetId` constant to match your device. Find the right targetId for your device [here](https://gist.github.com/TamtamHero/b7651ffe6f1e485e3886bf4aba673348). Notice that we must include a `--name` parameter containing the name of the custom certificate (any string will do):

        python3 -m ledgerblue.setupCustomCA --targetId 0x31100004 --public yourPublicKey --name dev

    If you receive the error `Invalid status 6985`, review [this link](https://github.com/LedgerHQ/blue-loader-python/issues/42) and go back to step 2. The above command is the simplest that can work however some developers may wish to use the optional `--rootPrivateKey` option to specify a secure channel encryption key (which is the private key generated at step 1) or use the `--name` option for convenient labeling according to local convention.

4. Once you have loaded your custom certificate, load an app you compiled onto your Ledger device to see if you are able to bypass the PIN. Before you try it, set the `SCP_PRIVKEY` environment variable to contain the private key generated at step 1 in your shell or `.bashrc`:

        export SCP_PRIVKEY=yourPrivateKey

    and rebuild/load your app.


<!--  -->
{% include alert.html style="warning" text="A Ledger device with a custom CA installed can not pass the Ledger Genuine Check, which is required to install applications from the Ledger Live. To make it pass the check, uninstall your custom CA and all the applications installed through it." %}
<!--  -->

**Uninstalling a custom CA is very quick:**

1. Enter in recovery mode on your Ledger Nano S: unplug the device and then plug it back while holding down the right button (near the hinge, away from USB port). `recovery mode` appears on the screen. Enter your pin and continue.

2.  Type this command in your terminal:

        foo@bar:~$ python3 -m ledgerblue.resetCustomCA --targetId 0x31100004

    Find the right targetId for your device [here](https://gist.github.com/TamtamHero/b7651ffe6f1e485e3886bf4aba673348).
