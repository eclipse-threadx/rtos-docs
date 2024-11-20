---
title: Chapter 3 - Functional description of NetX Duo Crypto
description: This chapter contains a functional description of NetX Duo Crypto.
author: TiejunMS
ms.author: tizho
ms.date: 03/08/2024
ms.topic: article
ms.service: rtos
---


## Execution Overview

This chapter contains a functional description of NetX Duo Crypto. There are two primary types of program execution in a NetX Duo Crypto application: initialization and application interface calls.

*NetX Duo Crypto can be used as a standalone cryptographic library, or can be used with ThreadX, NetX, and/or NetX Secure.*

## AES

- **Algorithm Standard:**:  NetX Duo Crypto implements AES according to NIST FIPS 197, which can be found at: [http://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf](http://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf)
- **Key Lengths Supported**: 128, 192, 256
- **Modes Supported**:
  - CBC, CTR, (Key length 128-, 192-, 256-bit)
  - XCBC (key length 128-bit only),
  - CCM8 (key length 128-bit only)
- **Memory Requirements**: Application specifies input buffer and output buffer and an AES control structure. The AES control structure maintains AES algorithm state between calls to the API. The input buffer contains data to be encrypted or decrypted, and can be arbitrary size. The output buffer is used by AES to store data being processed by AES. The output buffer size must be no smaller than the input buffer size, and must be a multiple of 16 bytes, the AES block size. The input and output buffers must be contiguous memory and may not overlap, except in the special case of encrypting in-place (using the same memory for input and output). When encrypting in-place, the output buffer starts at exactly the same location as the input buffer, and must be no smaller than the input buffer. When AES encryption operates in-place no extra scratch memory is required.

## 3DES

- **Algorithm Standard**: NetX Duo Crypto implements Tripple DES(TDES, also known as 3DES) according to NIST Special Publication 800-67 rev 2: *"Recommendataion for the Triple Data Encryption Algorithm (TDES) Block Cipher"*, which can be found at: [https://csrc.nist.gov/publications/detail/sp/800-67/rev-2/final](https://csrc.nist.gov/publications/detail/sp/800-67/rev-2/final)
- **Key Length Supported**: 64 * 3 = 192
- **Memory Requreiment:**: None

In NetX Duo Crypto, the term "3DES" is used interchangeably with "TDES".

## MD5

- **Algorithm Standard**: NetX Duo Crypto implements MD5 according to RFC 1321: *"The MD5 Message-Digest Algorithm"*
- **Memory Requirement**: The application must supply an MD5 control block structure, used to maintain state between MD5 operations.

## SHA1, SHA256/512

- **Algorithm Standard:** NetX Duo Crypto implements SHA1/256/512 according to NIST FIPS publication 180-4: "*Secure Hash Standard*", which can be found at: [http://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf](http://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf)
- **Hash block size:**:
  - SHA1: 160 bits hash value
  - SHA 224: 224 bits hash value
  - SHA 256: 256 bits hash value
  - SHA 384: 384 bits hash value
  - SHA 512: 512 bits hash value
  - SHA 512/224: 224 bits hash value
  - SHA 512/256: 256 bits hash value

  In NetX Duo Crypto, SHA256 routines are used to hadn SHA256 and SHA224. SHA512 routines are used to hand SHA512, SHA384, SHA512/224 and SHA512/256.
- **Memory Requirement:** The application must provide a SHA control block structure for maintaining state between operations.

## RSA

- **Standard:** NetX Duo Crypto implements RSA according to the standard "*PKCS #1 v2.2: RSA Cryptography Standard*", which is published as RFC 8017 and can also be found at: [https://www.emc.com/collateral/white-papers/h11300-pkcs-1v2-2-rsa-cryptography-standard-wp.pdf](https://www.emc.com/collateral/white-papers/h11300-pkcs-1v2-2-rsa-cryptography-standard-wp.pdf)
- **Memory Requirement:** The application must provide an RSA control block structure for maintaining state between operations and to provide necessary "scratch" buffer space for intermediate calculations.

## HMAC

- **Standard:** NetX Duo Crypto implements HMAC according to FIPS PUB 198-1: "*The Keyed-Hash Message Authentication Code (HMAC)*", which can be found at: [https://csrc.nist.gov/csrc/media/publications/fips/198/1/final/documents/fips-198-1_final.pdf](https://csrc.nist.gov/csrc/media/publications/fips/198/1/final/documents/fips-198-1_final.pdf)
- **Memory Requirement:** The application must provide an HMAC control block structure for maintaining state between operations. The actual control block supplied depends on the desired underlying hash operation (e.g. SHA1, MD5).

## Elliptic Curve

- **Standard:** NetX Duo Crypto implements Elliptic Curve. The supported named curves are (prime field only):
  - P-192
  - P-224
  - P-256
  - P-384
  - P-521

   > [!TIP]
   > Uncompressed format is supported. See section 2.3.3 and 2.3.4 of SEC1-v1: [http://www.secg.org/sec1-v2.pdf](http://www.secg.org/sec1-v2.pdf)

- **Memory Requirement:** None

## ECDSA

- **Standard:** NetX Duo Crypto implements ECDSA according to FIPS PUB 186-4: "*Digital Signature Standard (DSS)*", which can be found at: [https://nvlpubs.nist.gov/nistpubs/fips/nist.fips.186-4.pdf](https://nvlpubs.nist.gov/nistpubs/fips/nist.fips.186-4.pdf)
- **Memory Requirement:** The application must provide an ECDSA control block structure for maintaining state between operations.

## ECDH

> [!IMPORTANT]
> In Eclipse ThreadX, ECDH routines should only be used for ECDHE cryptography as ECDH with a static private key requires input point validation to be secure.

- **Standard:** NetX Duo Crypto implements ECDH according to FIPS PUB 800-56Ar2: "Recommendation for Pair-Wise Key Establishment Schemes Using Discrete Logarithm Cryptography", which can be found at: [https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-56ar2.pdf](https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-56ar2.pdf)
- **Memory Requirement:** The application must provide an ECDH control block structure for maintaining state between operations.

## DRBG

- **Standard:** NetX Duo Crypto implements DRBG according to FIPS PUB 800-90Ar1: "Recommendation for Random Number Generation Using Deterministic Random Bit Generators", which can be found at: [https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf)
- **Memory Requirement:** The application must provide an DRBG control block structure for maintaining state between operations.

## FIPS-Compliant

NetX Duo Crypto FIPS 140-2