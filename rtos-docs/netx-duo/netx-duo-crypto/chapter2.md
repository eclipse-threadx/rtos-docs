---
title: Chapter 2 - Installation and use of NetX Duo Crypto
description: This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo Crypto component.
---

# Chapter 2 - Installation and use of NetX Duo Crypto

This chapter describes installation, setup, and usage of the NetX Duo Crypto component.

## Product Distribution

NetX Duo Crypto is available at [https://github.com/eclipse-threadx/netxduo/](https://github.com/eclipse-threadx/netxduo/). The package includes source files, include files, and a PDF file that contains this document, as follows:

- **nx_crypto.h**: Public API header file NetX Duo Crypto module
- **nx_crypto_*.c/h**: C/H Source files for NetX Duo Crypto
- **nx_crypto_port.h**: C header file containing all development-tool and target specific data definitions and structures.
- **NetX_Crypto_User_Guide.pdf**: PDF description of NetX Duo Crypto Module.

## NetX Duo Crypto Installation

The entire distribution mentioned previously is available in **crypto_libraries** directory, present at root level of NetX Duo repository.

In order to use NetX Duo Crypto, the entire distribution mentioned previously should be copied to the same directory level where NetX is installed. For example, if NetX is installed in the directory "\threadx\arm7\NetX" then the nx_crypto*.* directories should be copied into "\threadx\arm7\NetXCrypto".

For NetX Duo Crypto to be used in standalone mode, the entire distribution mentioned previously should be copied to the application project. For example **crypto_libraries** directory should be copied to the application project or a library project with **crypto_libraries** directory should be created and linked to the application project. 

## Using NetX Duo Crypto

The application code must include the *nx_crypto.h*.  Once *nx_crypto.h* is included, the application code is then able to make the NetX Duo Crypto function calls specified later in this guide.

## Configuration Options

There are several configuration options for building NetX Duo Crypto. Following is a list of all options, where each is described in detail:

- **NX_CRYPTO_MAX_RSA_MODULUS_SIZE**: Defined, this option gives the maximum RSA modulus expected, in bits. The default value is 4096 for a 4096-bit modulus. Other values can be 3072, 2048, or 1024 (not recommended).
- **NX_CRYPTO_SELF_TEST**: Defined, enables self tests for NetX Duo Crypto module. **NX_CRYPTO_FIPS** symbol is now deprecated and renamed to **NX_CRYPTO_SELF_TEST**
- **NX_CRYPTO_STANDALONE_ENABLE**: Defined enables NetX Duo Crypto to be used in standalone mode (without Eclipse ThreadX). By default this symbol is not defined.
