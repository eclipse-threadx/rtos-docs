---
title: Chapter 1 - Introduction to NetX Duo Secure
description: This chapter contains an introduction to NetX Duo Secure and a description of its applications and benefits.
---


NetX Duo Secure is a high-performance real-time implementation of cryptographic network security standards including TLS/SSL designed exclusively for embedded ThreadX-based applications. This chapter contains an introduction to NetX Duo Secure and a description of its applications and benefits.

## NetX Duo Secure Unique Features

Unlike most other TLS implementations, NetX Duo Secure was designed from the ground up to support a wide variety of embedded hardware platforms and scales easily from small micro-controller applications to the most powerful embedded processors available. The code is written with the limited resources of embedded systems in mind, and provides a number of configuration options to reduce the memory footprint needed to provide secure network communications over TLS.

## RFCs Supported by NetX Duo Secure 

NetX Duo Secure supports the following protocols related to TLS. The list is not necessarily comprehensive as there are numerous RFCs pertaining to TLS and cryptography. NetX Duo Secure follows all general recommendations and basic requirements within the constraints of a real-time operating system with small memory footprint and efficient execution.

| RFC      | Description                                                                                                 | Page |
|----------|-------------------------------------------------------------------------------------------------------------|------|
| RFC 2104 | HMAC: Keyed-Hashing for Message Authentication                                                              | 33   |
| RFC 2246 | The TLS Protocol Version 1.0                                                                                | 19   |
| RFC 3268 | Advanced Encryption Standard (AES) Ciphersuites for Transport Layer Security (TLS)                          | 31   |
| RFC 3447 | Public-Key Cryptography Standards (PKCS) #1: RSA Cryptography Specifications Version 2.1                    | 32   |
| RFC 4279 | Pre-Shared Key Ciphersuites for TLS                                                                         | 39   |
| RFC 4346 | The Transport Layer Security (TLS) Protocol Version 1.1                                                     | 19   |
| RFC 5246 | The Transport Layer Security (TLS) Protocol Version 1.2                                                     | 19   |
| RFC 5280 | X.509 PKI Certificates (v3)                                                                                 | 41   |
| RFC 5746 | Transport Layer Security (TLS) Renegotiation Indication Extension                                           |      |
| RFC 5869 | HMAC-based Extract-and-Expand Key Derivation Function (HKDF)                                                | 19   |
| RFC 6066<sup>1</sup> | Transport Layer Security (TLS) Extensions: Extension Definitions                                            | 19   |
| RFC 6234 | US Secure Hash Algorithms (SHA and SHA-based HMAC and HKDF)                                                 | 33   |
| RFC 8443 | Elliptic Curve Cryptography (ECC) Cipher Suites for Transport Layer Security (TLS) Versions 1.2 and Earlier |      |
| RFC 8446 | The Transport Layer Security (TLS) Protocol Version 1.3                                                     | 19   |

1. As of version 6.0 only the Server Name Indication (SNI) extension from RFC 6066 is fully supported.

## NetX Duo Secure Requirements

In order to function properly, the NetX Duo Secure run-time library requires that a NetX IP instance has already been created. In addition, and depending on the application, one or more DER-encoded X.509 Digital Certificates will be required, either to identify a TLS instance or to verify certificates coming from a remote host. The NetX Duo Secure package has no further requirements.

## NetX Duo Secure Constraints

The NetX Duo Secure protocol implements the requirements of the RFC 5246 Standard(s) for TLS 1.2 and RFC 8446 for TLS 1.3, as well as providing optional (disabled by default) backwards-compatibility with RFCs 4346 (TLS 1.1) and 2246 (TLS 1.0). However, there are the following constraints:

- Only ciphersuites using SHA-256 are supported for TLS 1.2 and TLS 1.3. In versions prior to TLS 1.2, the TLS handshake uses a fixed hashing routine to verify TLS handshake messages are not tampered with. Starting with version 1.2, the handshake uses the hash routine provided with the ciphersuite. TLS does not know ahead of time what hash routine will be used and must cache the handshake messages. By fixing the hash to SHA-256, NetX Duo Secure TLS can function with a smaller RAM footprint than other TLS implementations. This limitation will be removed in a future version once the memory usage can be properly addressed. *IMPORTANT NOTE: This limitation applies **only** to ciphersuite choice. X.509 certificate signatures are not subject to the same limitation and any of the supported hash routines may be used.
- Due to the nature of embedded devices, some applications may not have the resources to support the maximum TLS record size of 16KB. NetX Duo Secure can handle 16KB records on devices with sufficient resources. The TLS reassembly buffer (see API reference for *nx_secure_tls_session_packet_buffer_set*) **may** be set to a size smaller than 16KB at the risk of interoperability problems. If a valid TLS record is received that is larger than the reassembly buffer, NetX Duo Secure TLS will abort the TLS session with an error. In general, the buffer should always be set to at least 18KB (16KB TLS record size + 2KB for encryption padding) and only made smaller in controlled settings (e.g. the remote host guarantees a maximum TLS record size).
  > **Note:** In general, the packet reassembly buffer should never be smaller than the TLS maximum record size. However, if the characteristics of the remote host are well known (e.g. in a completely closed system) the size can be reduced to regain some RAM space.
- Minimal certificate verification. NetX Duo Secure will perform basic X.509 chain verification on a certificate to assure that the certificate is valid and signed by a trusted Certificate Authority, and can provide the certificate Common Name for the application to compare against the Top-Level Domain Name of the remote host. If a real-time clock is available, it can be used to verify the certificate expiration date (see API nx_secure_tls_session_time_function_set). However, verification of certificate extensions and other data is the responsibility of the application implementer.
- Software-based cryptography is processor-intensive. The NetX Duo Secure software-based cryptographic routines have been optimized for performance, but depending on the power of the target processor, that performance may result in very long operations. When hardware-based cryptography is available, it should be used for optimal performance of NetX Duo Secure TLS.
- TLS Handshake Record fragmentation is unsupported. If certain TLS handshake record messages are too large, they may be split across multiple TLS records – NetX Duo Secure TLS currently treats this as an error. The memory requirements for embedded systems mean that the larger handshake record message likely cannot be handled anyway but the limitation could lead to errors when communicating with certain TLS hosts that use excessively large certificate chains.
- TLS server does not support dynamic certificate selection when there are multiple certificates in the local store. 
- X509 Certificate KeyUsage is not observed. 
- ECDH-based ciphersuites are not supported. Use ECDHE instead.
