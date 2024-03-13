---
title: Chapter 1 - Introduction to NetX Duo Crypto
description: NetX Duo Crypto is a high-performance real-time implementation of cryptographic algorithms designed to provide data encryption and authentication services.
---

# Chapter 1 - Introduction to NetX Duo Crypto

NetX Duo Crypto is a high-performance real-time implementation of cryptographic algorithms designed to provide data encryption and authentication services. NetX Duo Crypto is designed to plug in for NetX Duo Secure TLS, DTLS, and IPsec modules. Applications may also use NetX Duo Crypto as a standalone module outside network security.

## NetX Duo Crypto Unique Features

NetX Duo Crypto is implemented in the standard C language (C99), compatible with virtually all C/C++ compilers. Its modular design allows an application to only link in the crypto algorithms it needs to use, therefore achieving minimal code size. The implementation is designed to work with most 32-bit microprocessors and uses only the basic math operations (addition, subtraction, multiplication, division, logical AND, OR, NOR, and bit shift operations). All these operations are used with 32-bit quantities, making NetX Duo Crypto portable across most 32-bit microprocessors. The implementation is specifically optimized to run on resource constrained microprocessors, targeting deeply embedded applications.

## Algorithms supported by NetX Duo Crypto

NetX Duo Crypto supports the following cryptographic algorithms. NetX Duo Crypto follows all general recommendations and basic requirements within the constraints of a real-time operating system and platforms requiring a small memory footprint and efficient execution.

| Algorithm       | Key Length (bits)      |
| --------------- | ---------------------- |
| AES(CBC, CTR)   | 128, 192, 256          |
| AES(XCBC)       | 128                    |
| AES-CCM 8       | 128                    |
| 3DES(CBC)       | 192                    |
| HMAC-SHA1       | Any length             |
| HMAC-SHA224     | Any length             |
| HMAC-SHA256     | Any length             |
| HMAC-SHA384     | Any length             |
| HMAC-SHA512     | Any length             |
| HMAC-SHA512/224 | Any length             |
| HMAC-SHA512/256 | Any length             |
| HMAC-MD5        | Any length             |
| RSA             | 1024, 2048, 3072, 4096 |

| Algorithm       | Digest Length (bits) | Block Size (bits) |
| --------------- | -------------------- | ----------------- |
| SHA1            | 160                  | 512               |
| SHA224          | 224                  | 512               |
| SHA256          | 256                  | 512               |
| SHA384          | 384                  | 1024              |
| SHA512          | 512                  | 1024              |
| MD5             | 128                  | 512               |
| HMAC-SHA1       | 160                  | 512               |
| HMAC-SHA224     | 224                  | 512               |
| HMAC-SHA256     | 256                  | 512               |
| HMAC-SHA384     | 384                  | 1024              |
| HMAC-SHA512     | 512                  | 1024              |
| HMAC-SHA512/224 | 224                  | 1024              |
| HMAC-SHA512/256 | 256                  | 1024              |
| HMAC-MD5        | 128                  | 512               |
| Elliptic Curve  | P192/224/256/384/521 |                   |

## NetX Duo Crypto Requirements

TBD: Memory Requirement.. Interrupt/re-entrant safe? Needs discussion

## NetX Duo Crypto Constraints

None.
