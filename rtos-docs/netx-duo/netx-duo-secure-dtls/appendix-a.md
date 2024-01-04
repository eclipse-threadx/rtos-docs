---
title: Appendix A - NetX Duo Secure DTLS return/error codes
description: Lists the possible error codes that may be returned by NetX Duo Secure DTLS services.
---

# Appendix A: NetX Duo Secure DTLS return/error codes

## NetX Duo Secure TLS/DTLS return codes

Table 1 below lists the possible error codes that may be returned by NetX Duo Secure DTLS services. Note that the services may also return UDP or IP error codes – TLS values begin at 0x101 and TCP/IP/UDP values are below 0x100. X.509 return values start at 0x181. Refer to the NetX Duo TCP/IP/UDP documentation for information on IP and UDP return values and see below for X.509 values.

| **Error Name**                                        | **Value** | **Description**                                                                                                                                               |
| ----------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NX_SECURE_TLS_SUCCESS                              | 0x00      | Function returned successfully. (Same as NX_SUCCESS).                                                                                                        |
| NX_SECURE_TLS_SESSION_UNINITIALIZED               | 0x101     | TLS main loop called with uninitialized socket.                                                                                                               |
| NX_SECURE_TLS_UNRECOGNIZED_MESSAGE_TYPE          | 0x102     | TLS record layer received an unrecognized message type.                                                                                                       |
| NX_SECURE_TLS_INVALID_STATE                       | 0x103     | Internal error - state not recognized.                                                                                                                        |
| NX_SECURE_TLS_INVALID_PACKET                      | 0x104     | Internal error - received packet did not contain TLS data.                                                                                                    |
| NX_SECURE_TLS_UNKNOWN_CIPHERSUITE                 | 0x105     | The chosen ciphersuite is not supported - internal error for server, for client it means the remote host sent a bad ciphersuite (error or attack).            |
| NX_SECURE_TLS_UNSUPPORTED_CIPHER                  | 0x106     | In doing an encryption or decryption, the chosen cipher is disabled or unavailable.                                                                           |
| NX_SECURE_TLS_HANDSHAKE_FAILURE                   | 0x107     | Something in message processing during the handshake has failed.                                                                                              |
| NX_SECURE_TLS_HASH_MAC_VERIFY_FAILURE           | 0x108     | An incoming record had a MAC that did not match the one we generated.                                                                                         |
| NX_SECURE_TLS_TCP_SEND_FAILED                    | 0x109     | The outgoing TCP send of a record failed for some reason.                                                                                                     |
| NX_SECURE_TLS_INCORRECT_MESSAGE_LENGTH           | 0x10A     | An incoming message had a length that was incorrect (usually a length other than one in the header, as in certificate messages)                               |
| NX_SECURE_TLS_BAD_CIPHERSPEC                      | 0x10B     | An incoming ChangeCipherSpec message was incorrect.                                                                                                           |
| NX_SECURE_TLS_INVALID_SERVER_CERT                | 0x10C     | An incoming server certificate did not parse correctly.                                                                                                       |
| NX_SECURE_TLS_UNSUPPORTED_PUBLIC_CIPHER          | 0x10D     | A certificate provided by a server specified a public-key operation we do not support.                                                                        |
| NX_SECURE_TLS_NO_SUPPORTED_CIPHERS               | 0x10E     | Received a ClientHello with no supported ciphersuites.                                                                                                        |
| NX_SECURE_TLS_UNKNOWN_TLS_VERSION                | 0x10F     | An incoming record had a TLS version that isn't recognized.                                                                                                   |
| NX_SECURE_TLS_UNSUPPORTED_TLS_VERSION            | 0x110     | An incoming record had a valid TLS version, but one that isn't supported.                                                                                     |
| NX_SECURE_TLS_ALLOCATE_PACKET_FAILED             | 0x111     | An internal packet allocation for a TLS message failed.                                                                                                       |
| NX_SECURE_TLS_INVALID_CERTIFICATE                 | 0x112     | An X509 certificate did not parse correctly.                                                                                                                  |
| NX_SECURE_TLS_NO_CLOSE_RESPONSE                  | 0x113     | During a TLS session close, did not receive a CloseNotify from the remote host.                                                                               |
| NX_SECURE_TLS_ALERT_RECEIVED                      | 0x114     | The remote host sent an alert, indicating an error and closing the connection.                                                                                |
| NX_SECURE_TLS_FINISHED_HASH_FAILURE              | 0x115     | The Finish message hash received does not match the local generated hash - handshake corruption.                                                              |
| NX_SECURE_TLS_UNKNOWN_CERT_SIG_ALGORITHM        | 0x116     | A certificate during verification had an unsupported signature algorithm.                                                                                     |
| NX_SECURE_TLS_CERTIFICATE_SIG_CHECK_FAILED      | 0x117     | A certificate signature verification check failed - certificate data did not match signature.                                                                 |
| NX_SECURE_TLS_BAD_COMPRESSION_METHOD             | 0x118     | Received a Hello message with an unsupported compression method.                                                                                              |
| NX_SECURE_TLS_CERTIFICATE_NOT_FOUND              | 0x119     | In an operation on a certificate list, no matching certificate was found.                                                                                     |
| NX_SECURE_TLS_INVALID_SELF_SIGNED_CERT          | 0x11A     | The remote host sent a self-signed certificate and NX_SECURE_ALLOW_SELF_SIGNED_CERTIFICATES is not defined.                                              |
| NX_SECURE_TLS_ISSUER_CERTIFICATE_NOT_FOUND      | 0x11B     | A remote certificate was received with an issuer not in the local trusted store.                                                                              |
| NX_SECURE_TLS_OUT_OF_ORDER_MESSAGE              | 0x11C     | A DTLS message was received in the wrong order - a dropped datagram is the likely culprit.                                                                    |
| NX_SECURE_TLS_INVALID_REMOTE_HOST                | 0x11D     | A packet was received from a remote host that we do not recognize.                                                                                            |
| NX_SECURE_TLS_INVALID_EPOCH                       | 0x11E     | A DTLS message was received and matched to a DTLS session but it had the wrong epoch and should be ignored.                                                   |
| NX_SECURE_TLS_REPEAT_MESSAGE_RECEIVED            | 0x11F     | A DTLS message was received with a sequence number we have already seen, ignore it.                                                                           |
| NX_SECURE_TLS_NEED_DTLS_SESSION                  | 0x120     | A TLS session was used in a DTLS API that was not initialized for DTLS.                                                                                       |
| NX_SECURE_TLS_NEED_TLS_SESSION                   | 0x121     | A TLS session was used in a TLS API that was initialized for DTLS and not TLS.                                                                                |
| NX_SECURE_TLS_SEND_ADDRESS_MISMATCH              | 0x122     | Caller attempted to send data over a DTLS session with an IP address or port that did not match the session.                                                  |
| NX_SECURE_TLS_NO_FREE_DTLS_SESSIONS             | 0x123     | A new connection tried to get a DTLS session from the cache, but there were none free.                                                                        |
| NX_SECURE_DTLS_SESSION_NOT_FOUND                 | 0x124     | The caller searched for a DTLS session, but the given IP address and port did not match any entries in the cache.                                             |
| NX_SECURE_TLS_NO_MORE_PSK_SPACE                 | 0x125     | The caller attempted to add a PSK to a TLS session but there was no more space in the given session.                                                          |
| NX_SECURE_TLS_NO_MATCHING_PSK                    | 0x126     | A remote host provided a PSK identity hint that did not match any in our local store.                                                                         |
| NX_SECURE_TLS_CLOSE_NOTIFY_RECEIVED              | 0x127     | A TLS session received a CloseNotify alert from the remote host indicating the session is complete.                                                           |
| NX_SECURE_TLS_NO_AVAILABLE_SESSIONS              | 0x128     | No TLS sessions in a TLS object are available to handle a connection.                                                                                         |
| NX_SECURE_TLS_NO_CERT_SPACE_ALLOCATED           | 0x129     | No certificate space was allocated for incoming remote certificates.                                                                                          |
| NX_SECURE_TLS_PADDING_CHECK_FAILED               | 0x12A     | Encryption padding in an incoming message was not correct.                                                                                                    |
| NX_SECURE_TLS_UNSUPPORTED_CERT_SIGN_TYPE        | 0x12B     | In processing a CertificateVerifyRequest, no supported certificate type was provided by the remote server.                                                    |
| NX_SECURE_TLS_UNSUPPORTED_CERT_SIGN_ALG         | 0x12C     | In processing a CertificateVerifyRequest, no supported signature algorithm was provided by the remote server.                                                 |
| NX_SECURE_TLS_INSUFFICIENT_CERT_SPACE            | 0x12D     | Not enough certificate buffer space allocated for a certificate.                                                                                              |
| NX_SECURE_TLS_PROTOCOL_VERSION_CHANGED           | 0x12E     | The protocol version in an incoming TLS record did not match the version of the established session.                                                          |
| NX_SECURE_TLS_NO_RENEGOTIATION_ERROR             | 0x12F     | A HelloRequest message was received, but we are not re-negotiating.                                                                                           |
| NX_SECURE_TLS_UNSUPPORTED_FEATURE                 | 0x130     | A feature that was disabled was encountered during a TLS session or handshake.                                                                                |
| NX_SECURE_TLS_CERTIFICATE_VERIFY_FAILURE         | 0x131     | A CertificateVerify message from a remote Client failed to verify the Client certificate.                                                                     |
| NX_SECURE_TLS_EMPTY_REMOTE_CERTIFICATE_RECEIVED | 0x132     | The remote host sent an empty certificate message.                                                                                                            |
| NX_SECURE_TLS_RENEGOTIATION_EXTENSION_ERROR      | 0x133     | An error occurred in processing an or sending a Secure Renegotiation Indication extension.                                                                     |
| NX_SECURE_TLS_RENEGOTIATION_SESSION_INACTIVE     | 0x134     | A session renegotiation was attempting with a TLS session that was not active.                                                                                |
| NX_SECURE_TLS_PACKET_BUFFER_TOO_SMALL           | 0x135     | TLS received a record that was too large for the assigned packet buffer. The record could not be processed.                                                   |
| NX_SECURE_TLS_EXTENSION_NOT_FOUND                | 0x136     | A specified extension was not received from the remote host during the TLS handshake.                                                                         |
| NX_SECURE_TLS_SNI_EXTENSION_INVALID              | 0x137     | TLS received an invalid Server Name Indication extension.                                                                                                     |
| NX_SECURE_TLS_CERT_ID_INVALID                    | 0x138     | Application tried to add a server certificate with an invalid certificate ID value (likely 0).                                                                |
| NX_SECURE_TLS_CERT_ID_DUPLICATE                  | 0x139     | Application tried to add a server certificate with a certificate ID already present in the local store.                                                       |
| NX_SECURE_TLS_RENEGOTIATION_FAILURE               | 0x13A     | The remote host did not provide the Secure Renegotiation Indication Extension or the SCSV pseudo-ciphersuite so secure renegotiation cannot be performed.     |
| NX_SECURE_TLS_MISSING_CRYPTO_ROUTINE             | 0x13B     | In attempting to perform a cryptographic operation, one of the entries in the ciphersuite table (or one of its function pointers) was improperly set to NULL. |

**Table 1 – NetX Duo Secure TLS error return codes**

## NetX Duo Secure X.509 Return Codes

Table 2 below lists the possible error codes that may be returned by NetX Duo Secure X.509 services. Note that the services may also return other error codes. X.509 return values start at 0x181, TLS values begin at 0x101, and TCP/IP values are below 0x100. Refer to the NetX Duo TCP/IP documentation for information on TCP/IP return values and above for TLS return values.

| **Error Name**                                   | **Value** | **Description**                                                                                                        |
| ------------------------------------------------ | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| NX_SECURE_X509_SUCCESS                        | 0x00      | Successful return status. (Same as NX_SUCCESS)                                                                        |
| NX_SECURE_X509_MULTIBYTE_TAG_UNSUPPORTED    | 0x181     | We encountered a multi-byte ASN.1 tag - not currently supported.                                                       |
| NX_SECURE_X509_ASN1_LENGTH_TOO_LONG        | 0x182     | Encountered a length value longer than we can handle.                                                                  |
| NX_SECURE_X509_FOUND_NON_ZERO_PADDING      | 0x183     | Expected a padding value of 0 - got something different.                                                               |
| NX_SECURE_X509_MISSING_PUBLIC_KEY           | 0x184     | X509 expected a public key but didn't find one.                                                                        |
| NX_SECURE_X509_INVALID_PUBLIC_KEY           | 0x185     | Found a public key, but it is invalid or has an incorrect format.                                                      |
| NX_SECURE_X509_INVALID_CERTIFICATE_SEQUENCE | 0x186     | The top-level ASN.1 block is not a sequence - invalid X509 certificate.                                                |
| NX_SECURE_X509_MISSING_SIGNATURE_ALGORITHM  | 0x187     | Expecting a signature algorithm identifier, did not find it.                                                           |
| NX_SECURE_X509_INVALID_CERTIFICATE_DATA     | 0x188     | Certificate identity data is in an invalid format.                                                                     |
| NX_SECURE_X509_UNEXPECTED_ASN1_TAG          | 0x189     | We were expecting a specific ASN.1 tag for X509 format but we got something else.                                      |
| NX_SECURE_PKCS1_INVALID_PRIVATE_KEY         | 0x18A     | A PKCS#1 private key file was passed in, but the formatting was incorrect.                                            |
| NX_SECURE_X509_CHAIN_TOO_SHORT              | 0x18B     | An X509 certificate chain was too short to hold the entire chain during chain building.                                |
| NX_SECURE_X509_CHAIN_VERIFY_FAILURE         | 0x18C     | An X509 certificate chain was unable to be verified (catch-all error).                                                 |
| NX_SECURE_X509_PKCS7_PARSING_FAILED         | 0x18D     | Parsing an X.509 PKCS#7-encoded signature failed.                                                                     |
| NX_SECURE_X509_CERTIFICATE_NOT_FOUND        | 0x18E     | In looking up a certificate, no matching entry was found.                                                              |
| NX_SECURE_X509_INVALID_VERSION               | 0x18F     | A certificate included a field that isn't compatible with the given version.                                           |
| NX_SECURE_X509_INVALID_TAG_CLASS            | 0x190     | A certificate included an ASN.1 tag with an invalid tag class value.                                                   |
| NX_SECURE_X509_INVALID_EXTENSIONS            | 0x191     | A certificate included an extensions TLV but that did not contain a sequence.                                          |
| NX_SECURE_X509_INVALID_EXTENSION_SEQUENCE   | 0x192     | A certificate included an extension sequence that was invalid X.509.                                                   |
| NX_SECURE_X509_CERTIFICATE_EXPIRED           | 0x193     | A certificate had a "not after" field that was less than the current time.                                             |
| NX_SECURE_X509_CERTIFICATE_NOT_YET_VALID   | 0x194     | A certificate had a "not before" field that was greater than the current time.                                         |
| NX_SECURE_X509_CERTIFICATE_DNS_MISMATCH     | 0x195     | A certificate Common Name or Subject Alt Name did not match a given DNS TLD.                                           |
| NX_SECURE_X509_INVALID_DATE_FORMAT          | 0x196     | A certificate contained a date field that is not in a recognized format.                                               |
| NX_SECURE_X509_CRL_ISSUER_MISMATCH          | 0x197     | A provided CRL and certificate were not issued by the same Certificate Authority.                                      |
| NX_SECURE_X509_CRL_SIGNATURE_CHECK_FAILED  | 0x198     | A CRL signature check failed against its issuer.                                                                       |
| NX_SECURE_X509_CRL_CERTIFICATE_REVOKED      | 0x199     | A certificate was found in a valid CRL and has therefore been revoked.                                                 |
| NX_SECURE_X509_WRONG_SIGNATURE_METHOD       | 0x19A     | In attempting to validate a signature the signature method did not match the expected method.                          |
| NX_SECURE_X509_EXTENSION_NOT_FOUND          | 0x19B     | In looking for an extension, no extension with a matching ID was found.                                                |
| NX_SECURE_X509_ALT_NAME_NOT_FOUND          | 0x19C     | A name was searched for in a subjectAltName extension but was not found.                                               |
| NX_SECURE_X509_INVALID_PRIVATE_KEY_TYPE    | 0x19D     | Private key type given was unknown or invalid.                                                                         |
| NX_SECURE_X509_NAME_STRING_TOO_LONG        | 0x19E     | Passed a name string that was too long for an internal buffer (DNS name, etc…).                                        |
| NX_SECURE_X509_EXT_KEY_USAGE_NOT_FOUND    | 0x19F     | In searching an Extended Key Usage extension, the specified key usage OID was not found.                               |
| NX_SECURE_X509_KEY_USAGE_ERROR              | 0x1A0     | To be returned by the application callback if there is a failure in key usage during a certificate verification check. |

**Table 2 – NetX Duo Secure X.509 error return codes**
