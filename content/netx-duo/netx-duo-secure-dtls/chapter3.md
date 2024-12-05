---
title: Chapter 3 - Functional description of NetX Duo Secure DTLS
description: This chapter contains a functional description of NetX Duo Secure DTLS.
---


## Execution Overview

This chapter contains a functional description of NetX Duo Secure DTLS. There are two primary types of program execution in a NetX Duo Secure DTLS application: initialization and application interface calls. 

NetX Duo Secure assumes the existence of ThreadX and NetX Duo. From ThreadX, it requires thread execution, suspension, periodic timers, and mutual exclusion facilities. From NetX Duo it requires the UDP and IP networking facilities and drivers.

## Datagram Transport Layer Security (DTLS) and Transport Layer Security (TLS)

NetX Duo Secure DTLS implements the Datagram Transport Layer Security protocol version 1.2 defined in RFC 6347. DTLS version 1.0 was defined in RFC 4347 and corresponded to TLS version 1.1. Due to DTLS being essentially an extension to TLS, it was decided that the next version would use the same version number as the corresponding TLS version. Thus, there is no DTLS version 1.1 as DTLS version 1.2 corresponds to TLS version 1.2.

> **Note:** NetX Duo Secure supports DTLS version 1.2. DTLS 1.0 (RFC 4347) is **not** currently supported.

*Secure Sockets Layer* (SSL) was the original name of TLS before it became a standard in RFC 2246 and "SSL" is often used as a generic name for the TLS protocols. The last version of SSL was 3.0, and TLS 1.0 is sometimes referred to as SSL version 3.1. All versions of the official "SSL" protocol are considered obsolete and insecure and currently NetX Duo Secure does not provide an SSL implementation.

TLS specifies a protocol to generate *session keys* which are created during the TLS *handshake* between a TLS client and server and those keys are used to encrypt data sent by the application during the TLS *session.*

DTLS is closely coupled with TLS, as the underlying security mechanisms are shared between the protocols. However, TLS is designed to work over a transport layer protocol that provides guarantees about packet delivery and order (almost always TCP in practice) and will not function over an unreliable protocol like UDP. It is precisely because of UDP that DTLS was introduced: DTLS was designed handle the unreliable nature of UDP and similar protocols. It does this by including ordering and reliability logic (e.g. retransmission of dropped data) similar to reliable protocols like TCP.

A complete discussion of TLS is included in Chapter 3 of the NetX Duo Secure TLS User's Guide, so this document will focus on the differences between TLS and DTLS.

### DTLS Record header

Any valid DTLS record must have a DTLS header, as shown in Figure 1. The header is the same as TLS with the addition of two new fields: the 16-bit *epoch* and the 48-bit *sequence number*, described below.

{{< figure src="../media/image2.png" title="Diagram of a DTLS record header." imgClass="img-responsive center-block" >}}

**Figure 1 - DTLS record header**

The fields of the TLS record header are defined as follows:

| TLS Header Field | Purpose  |
| ---------------- | --------- |
| **8-bit Message Type** | This field contains the type of DTLS record being sent. Valid types are as follows:<br>- ChangeCipherSpec: 0x14<br>- Alert: 0x15<br>- Handshake: 0x16<br>- Application Data: 0x17<br> |
| **16-bit Protocol Version** | This field contains the DTLS protocol version. Valid values are as follows:<br>- DTLS 1.1: 0xFEFD |
|  **16-bit Epoch** |  This field contains the DTLS "epoch" which is a counter that is incremented each time the encryption state is changed (e.g. when generating new session keys).  |
|  **48-bit Sequence Number** |  This field contains a sequence number which identifies this particular record. It is used by DTLS to maintain record ordering and check for retransmission need. |
|  **16-bit Length** |  This field contains the length of the data encapsulated in the DTLS record.  |

### DTLS Handshake Record header

Any valid DTLS handshake record must have a DTLS Handshake header, as shown in Figure 2.

{{< figure src="../media/image3.png" title="Diagram of a DTLS Handshake Record header." imgClass="img-responsive center-block" >}}

**Figure 2 - DTLS Handshake record header**

The fields of the DTLS Handshake record header are defined as follows:

| TLS Header Field | Purpose  |
| ---------------- | ------------------------------------------------ |
| **8-bit Message Type** | This field contains the type of DTLS record being sent. Valid types are as follows:<br>- ChangeCipherSpec: 0x14<br>- Alert: 0x15<br>- Handshake: 0x16<br>- Application Data: 0x17 |
|  **16-bit Epoch** | This field contains the DTLS "epoch" which is a counter that is incremented each time the encryption state is changed (e.g. when generating new session keys). |
|  **48-bit Sequence Number** | This field contains a sequence number which identifies this particular record. It is used by DTLS to maintain record ordering and check for retransmission need. |
|  **16-bit Protocol Version** | This field contains the DTLS protocol version. Valid values are as follows:<br>- DTLS 1.1: 0xFEFD |
| **16-bit Length** | This field contains the length of the data encapsulated in the DTLS record. |
| **8-bit Handshake Type** | This field contains the handshake message type. Valid values are as follows:<br>- HelloRequest: 0x00<br>- ClientHello: 0x01<br>- ServerHello: 0x02<br>- Certificate: 0x0B<br>- ServerKeyExchange: 0x0C<br>- CertificateRequest: 0x0D<br>- ServerHelloDone: 0x0E<br>- CertificateVerify: 0x0F<br>- ClientKeyExchange: 0x10<br>- Finished: 0x14 |
| **24-bit Length** | This field contains the length of the handshake message data. |
| **16-bit Sequence Number** | This field contains a sequence number. |

### The DTLS Handshake and DTLS Session

A typical DTLS handshake is shown in Figure 3. It is nearly identical to the typical TLS handshake with an important difference – when the ClientHello message is first sent, the server responds with a new DTLS-specific message, *HelloVerifyRequest* which contains a "cookie". The DTLS Client must respond with a second ClientHello message containing that cookie before the handshake can proceed. This mechanism was added to DTLS to prevent certain Denial of Service (DoS) attacks since UDP is a connectionless protocol (TCP requires a dedicated connection/port so TLS does not suffer from the same issue).

A DTLS handshake begins when the Client sends a *ClientHello* message to a DTLS server, indicating its desire to start a DTLS session. The message contains information about the encryption the client would like to use for the session, along with information used to generate the session keys later in the handshake. Until the session keys are generated, all messages in the DTLS handshake are not encrypted. As mentioned above, the DTLS Server may send a HelloVerifyRequest in response to the ClientHello, forcing the client to respond with a second updated ClientHello.

Upon receiving the second ClientHello message, the DTLS Server will verify the cookie and if correct will respond with a ServerHello message indicating a selection from the encryption options provided by the client. The ServerHello is followed by a Certificate message, in which the server provides a digital certificate to authenticate its identity to the client (if X.509 verification is used). Finally, the server sends a ServerHelloDone message to indicate it has no more messages to send. The server may optionally send other messages following the ServerHello and in some cases it may not send a Certificate message (such as when Pre-Shared Keys are used), hence the need for the ServerHelloDone message.

Once the client has received all the server's messages, it has enough information to generate the session keys. TLS/DTLS does this by creating a shared bit of random data called the *Pre-Master Secret*, which is a fixed-size and is used as a seed to generate all the keys needed once encryption is enabled. The Pre-Master Secret is encrypted using the public key algorithm (e.g. RSA) specified in the Hello messages (see below for information on public key algorithms) and the public key provided by the server in its certificate. An optional TLS/DTLS feature called Pre-Shared Keys (PSK) enables ciphersuites that do not use a certificate but instead use a secret value shared between the hosts (usually through physical transfer or other secured method). When PSK is enabled, the pre-shared secret key is used to generate the Pre-Master Secret. See the section on Pre-Shared Keys in "Authentication Methods" below.

In a usual TLS/DTLS handshake, the encrypted Pre-Master Secret is sent to the server in the ClientKeyExchange message. The server, upon receiving the ClientKeyExchange message, decrypts the Pre-Master Secret using its private key and proceeds to generate the session keys in parallel with the TLS/DTLS client.

Once the session keys are generated, all further messages can be encrypted using the private-key algorithm (e.g. AES) selected in the Hello messages. One final un-encrypted message called ChangeCipherSpec is sent by both the client and server to indicate that all further messages will be encrypted.

The first encrypted message sent by both the client and server is also the final TLS handshake message, called Finished. This message contains a hash of all the handshake messages received and sent. This hash is used to verify that none of the messages in the handshake have been tampered with or corrupted (indicating a possible breach of security).

Once the Finished messages are received and the handshake hashes are verified, the TLS/DTLS session begins, and the application begins sending and receiving data. All data sent by either side during the TLS/DTLS session is first hashed using the hash algorithm chosen in the Hello messages (to provide message integrity) and encrypted using the chosen private-key algorithm with the generated session keys.

Finally, a TLS/DTLS session can only be successfully ended if either the Client or Server chooses to do so. A truncated session is considered a security breach (since an attacker may be attempting to prevent all the data being sent from being received) so a special notification is sent when either side wants to end the session, called a CloseNotify alert. Both the client and server must send and process a CloseNotify alert for a successful session shutdown.

{{< figure src="../media/image4.png" title="Diagram of a typical DTLS handshake session." imgClass="img-responsive center-block" >}}

**Figure 3- Typical DTLS handshake**

### Initialization

The NetX Duo stack must be initialized prior to using NetX Duo Secure DTLS. Refer to the NetX Duo User Guide for information on how to properly initialize the TCP/IP stack for UDP operation.

Once NetX Duo UDP has been initialized, DTLS can be enabled. Internally, all DTLS network traffic and processing is handled by the NetX Duo stack without requiring user intervention. However, DTLS has some specific requirements that must be handled separately from the underlying network stack. DTLS Client operation these parameters are assigned to the DTLS control block called ***NX_SECURE_DTLS_SESSION***. For DTLS Server operation, control block is called ***NX_SECURE_DTLS_SERVER*** and it contains the infrastructure needed to handle multiple DTLS sessions on a single UDP port – note that this is different from TLS where a each TLS session is bound to a single TCP port.

The two DTLS modes, Server and Client, may be enabled in an application (but only one mode per NetX Duo socket), and each have their own specific requirements, detailed below.

### Initialization – DTLS Server

NetX Duo Secure DTLS Server mode differs from TLS Server mode due to the use of UDP for the underlying network transport protocol. With TCP, the port is bound to a single remote host for the duration of the TLS session. UDP has no notion of state with regard to the remote host so DTLS requests from different hosts will all be received on the same UDP interface. Therefore, DTLS must maintain session state rather than relying on the socket as with TLS and TCP. For this reason, the DTLS Server control block (NX_SECURE_DTLS_SERVER) maintains a mapping of remote host information (IP address and port) to DTLS sessions. All incoming data on the UDP socket assigned to a DTLS Server will be mapped to an existing or new DTLS session based on the remote host. For this reason, the DTLS server creation requires several additional parameters beyond what TLS and DTLS Client need.

In addition to the DTLS Server control block, TLS ciphersuites, and cipher scratchspace/metadata buffer, DTLS Servers require a buffer to maintain DTLS sessions and a packet reassembly buffer used to decrypt incoming DTLS records.

In addition to the session buffers, DTLS Servers require a *Digital Certificate*, which is a document used to identify the TLS server to the connecting TLS client, and the certificates corresponding *Private Key*, usually for the RSA encryption algorithm. The International Telecommunications Union X.509 standard specifies the certificate format used by TLS/DTLS and there are numerous utilities for creating X.509 digital certificates.

For NetX Duo Secure DTLS, the X.509 certificate must be binary-encoded using the Distinguished Encoding Rules (DER) format of ASN.1. DER is the standard TLS over-the-wire binary format for certificates.

The private key associated with the provided certificate must be in DER-Encoded PKCS#1 format. The private key is only used on the device and will never be transmitted over the wire. Keep private keys safe as they provide the security for TLS/DTLS communications!

To initialize the DTLS Server certificate, the application must provide a pointer to a buffer containing the DER-encoded X.509 certificate and optional DER-encoded PKCS#1 RSA private key data using the ***nx_secure_x509_certificate_initialize*** service,
which populates the **NX_SECURE_X509_CERT** structure with the appropriate certificate data for use by TLS.

Once the server certificate has been initialized, it must be added to the TLS control block using the ***nx_secure_dtls_server_local_certificate_add*** service.

Once the server's certificate has been added to the DTLS Server control block, the server can be used for secure DTLS communications (see example above).

### Initialization – DTLS Client

NetX Duo Secure DTLS Client mode is simple in operation compared to the DTLS server since there is only a single outgoing connection to the remote host over the UDP socket.

To setup a DTLS Client, it requires a *Trusted Certificate Store*, which is a collection of X.509-encoded digital certificates from trusted Certificate Authorities (CA's). These certificates are assumed by the DTLS protocol to be "trusted" and serve as the basis for authenticating certificates provided by DTLS server entities to the NetX Duo Secure DTLS Client application.

A trusted CA certificate may either be *self-signed* or signed by another CA, in which case that certificate is called an *Intermediate CA* (ICA). In a typical TLS/DTLS application, the server provides the ICA certificates along with its server certificate, but the only requirement for successful authentication is that a chain of issuers (certificates used to sign other certificates) can be traced from the server certificate back to a trusted CA certificate in the Trusted Certificate Store. This chain is known as a *chain of trust* or *certificate chain*.

To initialize a trusted CA or ICA certificate, the application must provide a pointer to a buffer containing the DER-encoded X.509 certificate using the ***nx_secure_x509_certificate_initialize*** service, which populates the **NX_SECURE_X509_CERT** structure with the appropriate certificate data for use by TLS.

The DTLS Client also needs space for the incoming server certificate to be allocated (assuming a Pre-Shared Key mode is not being used) and a buffer for assembling packets into DTLS records to be decrypted. These buffers are passed in as parameters to the ***nx_secure_dtls_session_create*** service (see API reference for more information).

Trusted certificates that have been initialized are then added to the created DTLS session control block using the ***nx_secure_dtls_session_trusted_certificate_add*** service. Failure to add a certificate will cause the DTLS Client session to fail as there will be no way for the DTLS protocol to authenticate remote server hosts.

Once the Trusted Certificate Store has been created the session may be used to establish a secure TLS Client connection.

### Application Interface Calls

NetX Duo Secure DTLS applications will typically make function calls from within application threads running under the ThreadX RTOS. Some initialization, particularly for the underlying network communications protocols (e.g. UDP and IP) may be called from ***tx_application_define***. See the NetX Duo User Guide for more information on initializing network communications.

DTLS makes heavy use of encryption routines which are processor-intensive operations. Generally, these operations will be performed within the context of calling thread.

### DTLS Session Start

DTLS requires an underlying transport-layer network protocol in order to function. The protocol typically used is TCP. In order to establish a NetX Duo Secure TLS session an **NX_UDP_SOCKET** must be created and passed into the ***nx_secure_dtls_client_session_start*** service for DTLS Clients.

DTLS Servers operate differently. The UDP socket used for incoming DTLS Client requests is contained within the NX_SECURE_DTLS_SERVER control block and is initialized in the call to ***nx_secure_dtls_server_create***, which takes the local UDP port as a parameter. The service ***nx_secure_dtls_server_start*** is then used to start the DTLS Server to handle incoming requests. All incoming requests are handled in callback routines provided to *nx_secure_dtls_server_create*: one for connections and one for receive notifications. It is up to the application to handle starting the DTLS session when a connection notification is received (the connect notify callback is invoked by DTLS) by calling ***nx_secure_dtls_server_session_start***. The application also must handle incoming data when the receive notify callback is invoked (which follows a completed DTLS handshake) by calling ***nx_secure_dtls_session_receive***. The details of this are provided in the example above and in the API reference for each of the above mentioned services.

### DTLS Packet Allocation

NetX Duo Secure DTLS uses the same packet structure as NetX Duo TCP (***NX_PACKET***) except that instead of calling the ***nx_packet_allocate*** service, the ***nx_secure_dtls_packet_allocate*** service must be called so that space for the DTLS header may be allocated properly.

### DTLS Session Send

Once the TLS session has started, the application may send data using the ***nx_secure_dtls_session_send*** service. The send service is
identical in use to the ***nx_udp_socket_send*** service, taking an ***NX_PACKET*** data structure containing the data being sent, a target IP address, and a target UDP port.

> **Important:** When sending data using nx_secure_dtls_session_send, it is important to use the same IP address and port that were used to establish the DTLS session, unless there is a mechanism in place to move the session to a new address and UDP port on-the-fly (this is not common).

Any data sent over DTLS will be encrypted by the NX Secure DTLS stack and the configured encryption routines before being sent.

### DTLS Session Receive

Once the DTLS session has started, the application may begin receiving data using the ***nx_secure_Dtls_session_receive*** service. Like the DTLS Session send, this service is identical in use to ***nx_udp_socket_receive***, except that the incoming data is decrypted and verified by the DTLS stack before being returned in the packet structure.

### TLS Session Close

Once a DTLS session is complete, both the DTLS client and server must send a CloseNotify alert to the other side to shut down the session. Both sides must receive and process the alert to ensure a successful session shutdown.

If the remote host sends a CloseNotify alert, any calls to the ***nx_secure_dtls_session_receive*** service will process the alert, send the corresponding alert back to the remote host, and return a value of ***NX_SECURE_TLS_SESSION_CLOSED***. Once the session is closed, any further attempts to send or receive data with that DTLS session will fail.

If the application wishes to close the TLS session, the ***nx_secure_dtls_session_end*** service must be called. The service will send the CloseNotify alert and process the response CloseNotify. If the response is not received, an error value of ***NX_SECURE_TLS_SESSION_CLOSE_FAIL*** will be returned, indicating that the DTLS session was not cleanly shutdown, a possible security breach.

### TLS/DTLS Alerts

TLS/DTLS is designed to provide maximum security, so any errant behavior in the protocol is considered a potential security breach. For this reason, any errors in message processing or encryption/decryption are considered fatal errors that terminate the handshake or session immediately.

While handling errors in a local application is relatively straightforward, the remote host needs to know that an error has occurred in order to properly handle the situation and prevent any further possible security breaches. For this reason, TLS/DTLS will send an *Alert* message to the remote host upon any error.

Alerts are treated in the same manner as any other TLS/DTLS messages and are encrypted during the session to prevent an attacker from gathering information from the type of alert provided. During the handshake, the alerts sent are limited in scope to limit the amount of information that could be obtained by a potential attacker.

The CloseNotify alert, used to close the TLS/DTLS session, is the only non-fatal alert. While it is considered an alert and sent as an alert message, a CloseNotify is unlike other alerts in that it does not indicate an error has occurred.

### TLS/DTLS Session Renegotiation and Resumption

TLS supports the notion of "renegotiation" which is simply a renegotiation of the TLS session parameters within the context of an existing TLS session.

TLS session *resumption* should not be confused with session *renegotiation*, despite some similarities. Where session *renegotiation* involves starting a new handshake within an existing TLS session, session *resumption* is a purely optional feature that involves restarting a closed TLS session without performing a complete TLS handshake.

NX Secure DTLS handles incoming renegotiation requests from remote hosts. It does **not** support session resumption. A more complete discussion of these features can be found in Chapter 3 of the NetX Duo Secure TLS User Guide.

### Protocol Layering

The TLS protocol (and therefore DTLS as well) fits into the networking stack between the transport layer (e.g. TCP or UDP) and the application layer. TLS is sometimes considered a transport-layer protocol (hence *Transport Layer* Security) but because it acts as an application with regard to the underlying network protocols it is sometimes grouped into the application layer.

TLS requires a transport layer protocol that supports in-order and lossless delivery, such as TCP. Due to this requirement, TLS cannot run on top of UDP since UDP does not guarantee delivery of datagrams. *DTLS* is a modified version of TLS, is used for applications that need the security of TLS over a datagram protocol like UDP.

{{< figure src="../media/image6.png" title="Diagram of a TLS protocol layering." imgClass="img-responsive center-block" >}}

**Figure 4- TCP/IP, UDP and TLS/DTLS protocol layers**

## Network Communications Security and Encryption

Securing communications over public networks and the Internet is a critically important topic and the subject of vast numbers of books, articles, and solutions. The topic is mind-bogglingly complex, but can be reduced to a simple idea: sending information over a network so that only the intended target can access or change that information. This breaks down into three important concepts: secrecy, integrity, and authentication. The TLS/DTLS protocol provides solutions for all three.

Encryption is used in different ways to provide secrecy, integrity, and authentication within the TLS and DTLS protocols. The encryption must be supplied to TLS or DTLS upon creation of a session or server instance as TLS provides a flexible framework for using encryption and not the encryption itself. NetX Duo Secure DTLS provides the necessary encryption routines for most applications so you do not have to be concerned about finding appropriate encryption.

A more detailed description of these topics can be found in Chapter 3 of the NetX Duo Secure TLS User Guide.

## TLS and DTLS Extensions

TLS (and therefore DTLS) provides a number of extensions that provide additional functionality for certain applications. These extensions are typically sent as part of the ClientHello or ServerHello messages, indicating to a remote host the desire to use an extension or providing additional details for use in establishing the secure TLS session.

NetX Duo Secure DTLS supports all of the extensions found in NetX Duo Secure TLS, and a complete description of those can be found in the NetX Duo Secure TLS User Guide, Chapter 3.

## Authentication Methods

TLS and DTLS provide the framework for establishing a secure connection between two devices over an insecure network, but part of the problem is knowing the identity of the device on the other end of that connection. Without a mechanism for authenticating the identity of remote hosts, it becomes a trivial operation for an attacker to pose as a trusted device.

Initially, it may seem that using IP addresses, hardware MAC addresses, or DNS would provide a relatively high level of confidence for identifying hosts on a network, but given the nature of TCP/IP technology and the ease with which addresses can be spoofed and DNS entries corrupted (e.g. through DNS cache poisoning), it becomes clear that TLS needs an additional layer of protection against fraudulent identities.

There are various mechanisms that can provide this extra layer of authentication for TLS, but the most common is the *digital certificate.* Other mechanisms include Pre-Shared Keys (PSK) and password schemes.

### Digital Certificates

Digital certificates are the most common method for authenticating a remote host in TLS. Essentially, a digital certificate is a document with specific formatting that provides identity information for a device on a computer network.

TLS normally uses a format called X.509, a standard developed by the International Telecommunication Union, though other formats of certificates may be used if the TLS hosts can agree on the format being used. X.509 defines a specific format for certificates and various encodings that can be used to produce a digital document. Most X.509 certificates used with TLS are encoded using a variant of ASN.1, another telecommunications standard. Within ASN.1 there are various digital encodings, but the most common encoding for TLS certificates is the Distinguished Encoding Rules (DER) standard. DER is a simplified subset of the ASN.1 Basic Encoding Rules (BER) that is designed to be unambiguous, making parsing easier. Over the wire, TLS certificates are usually encoded in binary DER, and this is the format that NetX Duo Secure expects for X.509 certificates.

Though DER-formatted binary certificates are used in the actual TLS protocol, they may be generated and stored in a number of different encodings, with file extensions such as .pem, .crt, and .p12. The different variants are used by different applications from different manufacturers, but generally all can be converted into DER using widely available tools.

The most common of the alternative certificate encodings is PEM. The PEM format (from Privacy-Enhanced Mail) is a base-64 encoded version of the DER encoding that is often used because the encoding results in printable text that can be easily sent using email or web-based protocols.

Generating a certificate for your NetX Duo Secure application is generally outside the scope of this manual, but the OpenSSL command-line tool ([www.openssl.org](http://www.openssl.org)) is widely available and can convert between most formats.

Depending on your application, you may generate your own certificates, be provided certificates by a manufacturer or government organization, or purchase certificates from a commercial certificate authority.

To use a digital certificate in your NetX Duo Secure application, you must first convert your certificate into a binary DER format and, optionally, convert the associated private key (the "private exponent" for RSA, for example) into a binary format, typically a PKCS#1-formatted, DER-encoded RSA key. Once the conversion is complete, it is up to you to load the certificate and private key onto the device. Possible options include using a flash-based file system or generating a C array from the data (using a tool such as "xxd" from Linux) and compiling the certificate and key into your application as constant data.

Once your certificate is loaded onto the device, the DTLS API can be used to associate your certificate with a DTLS session or server.

For details and examples on how to use X.509 certificates with NetX Duo Secure DTLS, see the section "Importing X.509 certificates into NetX Duo Secure" in the NetX Duo Secure TLS User Guide.

Refer to the following DTLS services in the API reference for more information:

- nx_secure_x509_certificate_initialize,
- nx_secure_dtls_session_local_certificate_add,
- nx_secure_dtls_server_local_certificate_add,
- nx_secure_dtls_session_local_certificate_remove,
- nx_secure_dtls_server_local_certificate_remove,
- nx_secure_dtls_session_trusted_certificate_add,
- nx_secure_dtls_server_trusted_certificate_add,
- nx_secure_dtls_session_trusted_certificate_remove
- nx_secure_dtls_server_trusted_certificate_remove

### TLS Client Certificate Specifics

DTLS Client implementations generally do not require a local certificate to be loaded onto the device. A local certificate is a certificate that identifies the local device. Specifically, a local certificate provides identity information for the device upon which the TLS/DTLS application is loaded. The exception to this is when Client Certificate Authentication is enabled, but this is less common.

A DTLS Client requires at least one trusted certificate to be loaded (more may be loaded if required), and space for a remote certificate to be allocated. A trusted certificate is a certificate that provides a basis for trust and authentication of the remote device, either directly or through a Public Key Infrastructure (PKI). The root of the chain of trust is usually called a Certification Authority or CA certificate. A remote certificate refers to the certificate sent by the remote host during the TLS handshake. It provides identity for that remote host and is authenticated by comparing it to a trusted certificate on the local device.

For more information on adding trusted certificates and allocating space for remote certificates, see the TLS API reference for the following services: nx_secure_dtls_session_create, nx_secure_dtls_session_trusted_certificate_add.

### TLS/DTLS Server Certificate Specifics

DTLS Server implementations generally do not require "trusted" certificates to be loaded onto the device or remote certificates to be allocated. The exception to this being when Client Certificate Authentication is enabled.

A TLS Server requires a "local" (or "identity") certificate to be loaded so the server can provide it to the remote client during the TLS handshake to authenticate the server to the client.

For more information about loading local certificates for use with NetX Duo TLS server applications, see the API reference for the following services: nx_secure_dtls_server_local_certificate_add, nx_secure_dtls_server_local_certificate_remove.


### Pre-Shared Keys (PSK)

An alternative mechanism for providing identification authentication in TLS is the notion of Pre-Shared Keys (PSK). Using a PSK ciphersuite removes the need to do the processor-intensive public-key encryption operations, a boon for resource-constrained embedded devices. The PSK replaces the certificate in the TLS/DTLS handshake and is used in place of the encrypted Pre-Master Secret for TLS/DTLS session key generation.

The PSK ciphersuites are limited in the sense that that a shared secret must be present on both devices before a TLS/DTLS session can be established. This means that the devices must have been loaded with that secret using some secure means other than a TLS PSK connection - PSKs may be updated over a TLS PSK connection, but the device must necessarily start with a PSK that is loaded through some other mechanism. For example, a sensor device and its gateway device could be loaded with PSKs in the factory before shipping, or a standard TLS connection (with a certificate) could be used to load the PSK.

PSK ciphersuites come in a couple of forms, described in RFC 4279. The first uses RSA or Diffie-Hellman keys which are used in the same manner as the public keys transmitted in the certificate in standard TLS handshakes. The second form, which is of more use in a resource-constrained environment, uses a PSK that is used to directly generate the session keys (for use by AES, for example), avoiding the use of the expensive RSA or Diffie-Hellman operations.

NetX Duo Secure supports the second form of PSK ciphersuites, enabling applications to remove all public-key cryptography code and memory usage. The PSK itself is not an AES key, but can be considered as being more like a password from which the actual keys are generated. There are few restrictions on what the PSK value can be, though longer values will provide more security (same as with passwords).

To use PSK with your NetX Duo Secure application, you must first define the global macro **NX_SECURE_ENABLE_PSK_CIPHERSUITES**. This is usually done through your compiler settings, but the definition can also be placed in the nx_secure_tls.h header file. With the macro defined, PSK ciphersuite support will be compiled into your NetX Duo Secure DTLS application.

With PSK support enabled, you can then use the DTLS API to set up PSKs for your application. Each PSK will require a PSK value (the actual secret "key" – keep this value safe), an "identity" value used to identify the specific PSK, and an "identity hint" that is used by a TLS server to choose a particular PSK value.

The PSK itself can be any binary value as it is never sent over a network connection. The PSK can be any value up to 64 bytes in length.

The identity and hint must be printable character strings formatted using UTF-8. The identity and hint values may be any length up to 128 bytes.

The identity and PSK form a unique pair that is loaded onto every device in the network that need to communicate with one another.

The "hint" is primarily used for defining specific application profiles to group PSKs by function or service. These values must be agreed upon in advance and are application dependent. As an example, the OpenSSL command-line server application (with PSK enabled) uses the default string "Client_identity", which must be provided by a TLS client in order to continue with the TLS handshake.

For more information on PSKs, see the NetX Duo Secure API reference for the following services: nx_secure_dtls_psk_add, nx_secure_dtls_server_psk_add.

## Importing X.509 certificates into NetX Duo Secure

Digital certificates are required for most TLS connections on the Internet. Certificates provide a method for authenticating previously unknown hosts over the Internet through the use of trusted intermediaries, usually called *Certificate Authorities* or CAs. To connect your NetX Duo Secure device with a commercial cloud service (such as Amazon Web Services), you will need to import certificates into your application by loading them onto your device.

Along with certificates, you will also sometimes need a *private key* that is associated with your certificate. In some applications (such as TLS Client when Client Certificate Authentication is not being used) the certificate alone will be sufficient, but if your certificate is being used to identify your device you will need a private key. Private keys are typically generated when you create your certificate and are stored in a separate file, often encrypted with a password.

For a detailed description of importing certificates into NetX Duo Secure applications, please refer to Chapter 3 in the NetX Duo Secure TLS User Guide.

## Client Certificate Authentication in NetX Duo Secure TLS

When using X.509 certificate authentication, the TLS/DTLS protocol requires that the DTLS Server instance provide a certificate for
identification, but by default the DTLS Client instance does not need to provide a certificate for authentication, using another form of authentication instead (e.g. a username/password combination). This matches the most common use of TLS on the Internet for Web sites. For example, an online retail site must prove to a potential customer using a web browser that the server is legitimate, but the user will use a login/password to access a specific account.

However, the default case is not always desirable, so TLS/DTLS optionally allows for the DTLS Server instance to request a certificate from the remote Client. When this feature is enabled, the DTLS Server will send a CertificateRequest message to the DTLS Client during the handshake. The Client must respond with a certificate of its own and a CertificateVerify message which contains a cryptographic token proving that the Client owns the matching private key associated with that certificate. If the verification fails or the certificate is not connected to a trusted certificate on the Server, the TLS handshake fails.

There are two separate cases for Client Certificate Authentication in TLS – the following sections cover both cases.

### Client Certificate Authentication for DTLS Clients

A DTLS Client may attempt a connection to a server that requests a certificate for client authentication. In this case the Client must provide a certificate to the server and verify that it owns the matching private key or the Server will terminate the DTLS handshake.

In NetX Duo Secure DTLS, there is no special configuration to support this feature but the application will have to provide a local identification certificate for the TLS Client instance using the *nx_secure_tls_session_local_certificate_add* service. If no certificate is provided by the application but the remote server is using Client Certificate Authentication and requests a certificate, the DTLS handshake will fail. The certificate provided to the DTLS Session with *nx_secure_dtls_session_local_certificate_add* must be recognized by the remote server in order to complete the DTLS handshake.

### Client Certificate Authentication for TLS Servers

The DTLS Server case for Client Certificate Authentication is slightly more complex than the DTLS Client case due to the feature being optional. In this case, the TLS Server needs to specifically request a certificate from the remote TLS Client, then process the CertificateVerify message to verify that the remote Client owns the matching private key, and then the Server must check that the certificate provided by the Client can be traced to a certificate in the local trusted certificate store.

In NetX Duo Secure TLS Server instances, Client Certificate Authentication is controlled by the *nx_secure_dtls_server_x509_client_verify_configure* and *nx_secure_dtls_server_x509_client_verify_disable* services.

To enable Client Certificate Authentication, an application must call *nx_secure_dtls_server_x509_client_verify_configure* with the DTLS Server session instance before calling *nx_secure_dtls_server_start*. The verification requires space to be allocated for incoming client certificates which is provided as a parameter to *nx_secure_dtls_server_x509_client_verify_configure.* Note that the buffer must be large enough to hold the maximum-size certificate chain provided by a client *times the number of DTLS server sessions*. Each server session requires space which will be allocated from the single provided buffer. Make sure the buffer is large enough or an error will occur if the provided Client certificate chain is too large.

When Client Certificate Authentication is enabled, the DTLS Server will request a certificate from the remote DTLS Client during the DTLS handshake. In NetX Duo Secure DTLS Server, the Client certificate is checked against the store of trusted certificates created with *nx_secure_dtls_server_trusted_certificate_add* by following the X.509 issuer chain. The remote Client must provide a chain that connects its identity certificate to a certificate in the trusted store or the DTLS handshake will fail. Additionally, if the CertificateVerify message processing fails, the DTLS handshake will also fail.

The signature methods used for the CertificateVerify method are fixed for TLS version 1.0 and TLS version 1.1, and are specified by the TLS Server in TLS version 1.2, upon which NetX Duo Secure DTLS is based. For DTLS 1.2, the signature methods supported generally follow the relevant methods supplied in the cryptographic method table, but typically RSA with SHA-256 (see the section "Cryptography in NetX Duo Secure TLS" for more information on initializing TLS with cryptographic methods).

## Cryptography in NetX Duo Secure TLS

TLS defines a protocol in which cryptography can be used to secure network communications. As such, it leaves the actual cryptography to be used fairly wide open for TLS users. The specification only requires a single ciphersuite to be implemented – in the case of TLS 1.2, that ciphersuite is TLS_RSA_WITH_AES_128_CBC_SHA, indicating the use of RSA for public-key operations, AES in CBC mode with 128-bit keys for
session encryption, and SHA-1 for message authentication hashes.

Being TLS 1.2-compliant, NetX Duo Secure enables the mandatory TLS_RSA_WITH_AES_128_CBC_SHA ciphersuite by default, but given the number of possible implementations for each of the cryptographic methods due to hardware capabilities and other considerations, NetX Duo Secure provides a generic cryptographic API that allows a user to specify which cryptographic methods to use with TLS.

> **Note:** The generic cryptographic API mechanism also allows users to implement their own ciphersuites, but this is recommended for advanced users who are familiar with the TLS ciphersuites and extensions. Please contact your Express Logic representative if you are interested in supporting your own ciphersuites.

Please see the NetX Duo Secure TLS User Guide, Chapter 3 for a detailed discussion about how to configure cryptographic methods for DTLS. The same process applies to both TLS and DTLS.
