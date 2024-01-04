---
title: Chapter 1 - Introduction to NetX Duo Secure DTLS
description: NetX Duo Secure DTLS is a real-time implementation of the Datagram Transport Layer Security protocol designed for embedded ThreadX-based applications.
---

# Chapter 1: Introduction to NetX Duo Secure DTLS

NetX Duo Secure DTLS is a high-performance real-time implementation of of the Datagram Transport Layer Security protocol designed exclusively for embedded ThreadX-based applications. This chapter contains an introduction to NetX Duo Secure DTLS and a description of its applications and benefits.

## NetX Duo Secure Unique Features

Unlike most other TLS/DTLS implementations, NetX Duo Secure was designed from the ground up to support a wide variety of embedded hardware platforms and scales easily from small micro-controller applications to the most powerful embedded processors available. The code is written with the limited resources of embedded systems in mind, and provides a number of configuration options to reduce the memory footprint needed to provide secure network communications over TLS or DTLS.

## RFCs Supported by NetX Duo Secure

NetX Duo Secure supports the following protocols related to TLS and DTLS. The list is not necessarily comprehensive as there are numerous RFCs pertaining to TLS/DTLS and cryptography. NetX Duo Secure follows all general recommendations and basic requirements within the constraints of a real-time operating system with small memory footprint and efficient execution.


| RFC | Description |
| --- | ----------- |
| RFC 6347 | Datagram Transport Layer Security Version 1.2. |
| RFC 2246 | The TLS Protocol Version 1.0|
| RFC 4346 | The Transport Layer Security (TLS) Protocol Version 1.1 |
| RFC 5246 | The Transport Layer Security (TLS) Protocol Version 1.2 |
| RFC 5280 | X.509 PKI Certificates (v3) |
| RFC 3268 | Advanced Encryption Standard (AES) Ciphersuites for Transport Layer Security (TLS) |
| RFC 3447 | Public-Key Cryptography Standards (PKCS) #1: RSA Cryptography Specifications Version 2.1 |
| RFC 2104 | HMAC: Keyed-Hashing for Message Authentication |
| RFC 6234 | US Secure Hash Algorithms (SHA and SHA-based HMAC and HKDF) |
| RFC 4279 | Pre-Shared Key Ciphersuites for TLS |

## NetX Duo Secure DTLS Requirements

In order to function properly, the NetX Duo Secure run-time library requires that a NetX IP instance has already been created. In addition, and depending on the application, one or more DER-encoded X.509 Digital Certificates will be required, either to identify a TLS/DTLS instance or to verify certificates coming from a remote host. The NetX Duo Secure package has no further requirements.

## NetX Duo Secure DTLS Constraints

The NetX Duo Secure DTLS protocol implements the requirements of the RFC 6347 Standard(s) for DTLS 1.2. However, there are the following constraints:

1. Due to the nature of embedded devices, some applications may not have the resources to support the maximum TLS/DTLS record size of 16KB. NetX Duo Secure can handle 16KB records on devices with sufficient resources.
2. Minimal certificate verification. NetX Duo Secure will perform basic X.509 chain verification on a certificate to assure that the     certificate is valid and signed by a trusted Certificate Authority, and can provide the certificate Common Name for the application to compare against the Top-Level Domain Name of the remote host. However, verification of certificate extensions and other data is the responsibility of the application implementer.
3. Software-based cryptography is processor-intensive. The NetX Duo Secure software-based cryptographic routines have been optimized for performance, but depending on the power of the target processor, that performance may result in very long operations. When hardware-based cryptography is available, it should be used for optimal performance of NetX Duo Secure DTLS.
