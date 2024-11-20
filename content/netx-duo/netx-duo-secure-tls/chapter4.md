---
title: Chapter 4 - Description of NetX Duo Secure services
description: This chapter contains a description of all NetX Duo Secure services (listed below) in alphabetic order.
---


This chapter contains a description of all NetX Duo Secure services (listed below) in alphabetic order.

In the "Return Values" section in the following API descriptions, values in **BOLD** are not affected by the **NX_SECURE_DISABLE_ERROR_CHECKING** macro that is used to disable API error checking, while non-bold values are completely disabled.

- [nx_secure_crypto_table_self_test](#nx_secure_crypto_table_self_test)
  - Perform self_test on the crypto methods
- [nx_secure_module_hash_compute](#nx_secure_module_hash_compute)
  - Computes hash value using user_supplied hash function
- [nx_secure_tls_active_certificate_set](#nx_secure_tls_active_certificate_set)
  - Set the active identity certificate for a NetX Duo Secure TLS Session
- [nx_secure_tls_client_psk_set](#nx_secure_tls_client_psk_set)
  - Set a Pre_Shared Key for a NetX Duo Secure TLS Client Session
- [nx_secure_tls_initialize](#nx_secure_tls_initialize)
  - Initializes the NetX Duo Secure TLS module]
- [nx_secure_tls_local_certificate_add](#nx_secure_tls_local_certificate_add)
  - Add a local certificate to NetX Duo Secure TLS Session
- [nx_secure_tls_local_certificate_find](#nx_secure_tls_local_certificate_find)
  - Find a local certificate in a NetX Duo Secure TLS Session by Common Name
- [nx_secure_tls_local_certificate_remove](#nx_secure_tls_local_certificate_remove)
  - Remove local certificate from NetX Duo Secure TLS Session
- [nx_secure_tls_metadata_size_calculate](#nx_secure_tls_metadata_size_calculate)
  - Calculate size of cryptographic metadata for a NetX Duo Secure TLS Session
- [nx_secure_tls_packet_allocate](#nx_secure_tls_packet_allocate)
  - Allocate a packet for a NetX Duo Secure TLS Session
- [nx_secure_tls_psk_add](#nx_secure_tls_psk_add)
  - Add a Pre_Shared Key to a NetX Duo Secure TLS Session
- [nx_secure_tls_remote_certificate_allocate](#nx_secure_tls_remote_certificate_allocate)
  - Allocate space for the certificate provided by a remote TLS host
- [nx_secure_tls_remote_certificate_buffer_allocate](#nx_secure_tls_remote_certificate_buffer_allocate)
  - Allocate space for all certificates provided by a remote TLS host
- [nx_secure_tls_remote_certificate_free_all](#nx_secure_tls_remote_certificate_free_all)
  - Free space allocated for incoming certificates
- [nx_secure_tls_server_certificate_add](#nx_secure_tls_server_certificate_add)
  - Add a certificate specifically for TLS servers using a numeric identifier
- [nx_secure_tls_server_certificate_find](#nx_secure_tls_server_certificate_find)
  - Find a certificate using a numeric identifier
- [nx_secure_tls_server_certificate_remove](#nx_secure_tls_server_certificate_remove)
  - Remove a local server certificate using a numeric identifier
- [nx_secure_tls_session_certificate_callback_set](#nx_secure_tls_session_certificate_callback_set)
  - Set up a callback for TLS to use for additional certificate validation
- [nx_secure_tls_session_client_callback_set](#nx_secure_tls_session_client_callback_set)
  - Set up a callback for TLS to invoke at the beginning of a TLS Client handshake
- [nx_secure_tls_session_x509_client_verify_configure](#nx_secure_tls_session_x509_client_verify_configure)
  - Enable client X.509 verification and allocate space for client certificates
- [nx_secure_tls_session_client_verify_disable](#nx_secure_tls_session_client_verify_disable)
  - Disable Client Certificate Authentication for a NetX Duo Secure TLS Session
- [nx_secure_tls_session_client_verify_enable](#nx_secure_tls_session_client_verify_enable)
  - Enable Client Certificate Authentication for a NetX Duo Secure TLS Session
- [nx_secure_tls_session_create](#nx_secure_tls_session_create)
  - Create a NetX Duo Secure TLS Session for secure communications
- [nx_secure_tls_session_delete](#nx_secure_tls_session_delete)
  - Delete a NetX Duo Secure TLS Session
- [nx_secure_tls_session_end](#nx_secure_tls_session_end)
  - End an active NetX Duo Secure TLS Session
- [nx_secure_tls_session_packet_buffer_set](#nx_secure_tls_session_packet_buffer_set)
  - Set the packet reassembly buffer for a NetX Duo Secure TLS Session
- [nx_secure_tls_session_packet_pool_set](#nx_secure_tls_session_packet_pool_set)
  - Configure a custom packet pool to be used in a NetX Duo Secure TLS Session
- [nx_secure_tls_session_protocol_version_override](#nx_secure_tls_session_protocol_version_override)
  - Override the default TLS protocol version for a NetX Duo Secure TLS Session
- [nx_secure_tls_session_receive](#nx_secure_tls_session_receive)
  - Receive data from a NetX Duo Secure TLS Session
- [nx_secure_tls_session_renegotiate_callback_set](#nx_secure_tls_session_renegotiate_callback_set)
  - Assign a callback that will be invoked at the beginning of a session renegotiation
- [nx_secure_tls_session_renegotiate](#nx_secure_tls_session_renegotiate)
  - Initiate a session renegotiation handshake with the remote host
- [nx_secure_tls_session_reset](#nx_secure_tls_session_reset)
  - Clear out and reset a NetX Duo Secure TLS Session
- [nx_secure_tls_session_send](#nx_secure_tls_session_send)
  - Send data through a NetX Duo Secure TLS Session
- [nx_secure_tls_session_server_callback_set](#nx_secure_tls_session_server_callback_set)
  - Set up a callback for TLS to invoke at the beginning of a TLS Server handshake
- [nx_secure_tls_session_sni_extension_parse](#nx_secure_tls_session_sni_extension_parse)
  - Parse a Server Name Indication (SNI) extension received from a TLS Client
- [nx_secure_tls_session_sni_extension_set](#nx_secure_tls_session_sni_extension_set)
  - Set a Server Name Indication (SNI) extension DNS name to send to a remote Server
- [nx_secure_tls_session_start](#nx_secure_tls_session_start)
  - Start a NetX Duo Secure TLS Session
- [nx_secure_tls_session_time_function_set](#nx_secure_tls_session_time_function_set)
  - Assign a timestamp function to a NetX Duo Secure TLS Session
- [nx_secure_tls_trusted_certificate_add](#nx_secure_tls_trusted_certificate_add)
  - Add trusted certificate to NetX Duo Secure TLS Session
- [nx_secure_tls_trusted_certificate_remove](#nx_secure_tls_trusted_certificate_remove)
  - Remove trusted certificate from NetX Duo Secure TLS Session
- [nx_secure_x509_certificate_initialize](#nx_secure_x509_certificate_initialize)
  - Initialize X.509 Certificate for NetX Duo Secure TLS
- [nx_secure_x509_common_name_dns_check](#nx_secure_x509_common_name_dns_check)
  - Check DNS name against X.509 Certificate
- [nx_secure_x509_crl_revocation_check](#nx_secure_x509_crl_revocation_check)
  - Check X.509 Certificate against a supplied Certificate Revocation List (CRL)]
- [nx_secure_x509_dns_name_initialize](#nx_secure_x509_dns_name_initialize)
  - Initialize an X.509 DNS name structure
- [nx_secure_x509_extended_key_usage_extension_parse](#nx_secure_x509_extended_key_usage_extension_parse)
  - Find and parse an X.509 extended key usage extension in an X.509 certificate
- [nx_secure_x509_extension_find](#nx_secure_x509_extension_find)
  - Find and return an X.509 extension in an X.509 certificate
- [nx_secure_x509_key_usage_extension_parse](#nx_secure_x509_key_usage_extension_parse)
  - Find and parse an X.509 Key Usage extension in an X.509 certificate

## nx_secure_crypto_table_self_test

Perform self-test on the crypto methods

### Prototype

```C
UINT nx_secure_crypto_table_self_test(
                                  const NX_SECURE_TLS_CRYPTO *crypto_table,
                                  VOID *metadata, UINT metadata_size);
```

### Description

This service runs through the crypto method self tests to validate. The self test is available only if NetX Duo Secure library is built with the symbol NX_SECURE_POWER_ON_SELF_TEST_MODULE_INTEGRITY_CHECK defined.

For each supported crypto method, the self test provides pre-defined input data, and verified the output matches the pre-defined expected value.

NetX Duo Secure crypto self test supports the following algorithms and key sizes:

- DES: encryption and decryption
- Triple DES (3DES): encryption and decryption
- AES: 128-, 192-, 256-bit key size, encryption and decryption, in CBC mode and counter mode.
- HMAC-MD5: authentication and hash computation
- HMAC-SHA: SHA1-96, SHA1-160, SHA2-256, SHA2-384, SHA2- 512, authentication and hash computation
- MD5: authentication
- Pseudo-random Function (PRF):
PRF_HMAC_SHA1 and PRF_HMAC_SHA2-256
- RSA: 1024-, 2048-, 4096-bit RSA power-modulus operation
- SHA: SHA1 (96- and 160-bit), SHA2 (256bit, 384bit, 512bit) authentication

This function has the built-in vectors for the crypto algorithms listed above. However it only tests the ones listed in the *cipher_table* passed into this function. For example, for a TLS session uses only the ciphersuite TLS_RSA_WITH_AES_128_CBC_SHA, this function will perform self test on the RSA (1024-, 2048-, 4096-bit), AES-CBC (128-bit) and SHA1.

### Parameters

- **crypto_table** Pointer to the crypto table being used by the TLS session. This is the same crypto_table being passed into the ***nx_secure_tls_session_create()*** call.
- **metadata** Pointer to space for cryptography metadata area. .
- **metadata_size** Size of the metadata buffer.

### Return Values

- **NX_SECURE_TLS_SUCCESS** (0x00) Successfully tested the crypto methods provided.
- **NX_PTR_ERROR** (0x07) Invalid crypto method structure
- **NX_NOT_SUCCESSFUL** (0x43) Crypto self test fails.

### Allowed From

Initialization, Threads

### Example

```C
/* crypto_tls_ciphers is the cipher table for the TLS session. */
/* metadata_buffer is the memory area used by the cipher functions. */

/* The following function verifies the supplied ciphersuties using the built-in
   self test. */

if (nx_secure_crypto_table_self_test(&crypto_tls_ciphers, metadata_buffer,
    sizeof(metadata_buffer)))
{
    printf("Crypto self test failed!\r\n");
    exit(0);
}
else
{
    printf("Crypto self test succeed!\r\n");
}

/* Now the ciphersuites are successfully test, it can be used in
   nx_secure_tls_session_create() */
```

### See Also

- nx_secure_tls_session_create

## nx_secure_module_hash_compute

Computes hash value using user-supplied hash function

### Prototype

```C
UINT nx_secure_module_hash_compute(
                      NX_CRYPTO_METHOD *hamc_ptr,
                      UINT start_address, UINT end_address,
                      UCHAR *key, UINT key_length,
                      VOID *metadata, UINT metadata_size,
                      UCHAR *output_buffer, UINT output_buffer_size,
                      UINT *actual_size);
```

### Description

This function computes the hash value of the data stream in the specified memory area, using supplied HMAC crypto method and the key string. The module hash compute function is available only if NetX Duo Secure library is built with the following symbol being defined: NX_SECURE_POWER_ON_SELF_TEST_MODULE_INTEGRITY_CHECK

### Parameters

- **hmac_ptr** Pointer to the HMAC crypto method being used for the hash value computation.
- **start_address** The starting address of the data buffer
- **end_address** The ending address of the data buffer. Note that hash computation does not cover the data at this end_address.
- **key** The key string being used in the HMAC computation.
- **key_length** The size of the key string, in bytes
- **metadata** Pointer to space used by the HMAC algorithm.
- **metadata_size** Size of the metadata buffer.
- **output_buffer** The memory location where the hash output is being stored at.
- **output_buffer_size** The available space of the output buffer, in bytes
- **actual_size** Returned by the function indicating the actual number of bytes being written into the output_buffer.

### Return Values

- **0** Successfully computed the hash value.
- **1** The hash computation failed.

### Allowed From

Initialization, Threads

### Example

```C
/* Define the HMAC SHA256 method */
NX_CRYPTO_METHOD hmac_sha256 =
{
    NX_CRYPTO_AUTHENTICATION_HMAC_SHA2_256,    /* HMAC SHA256 algorithm  */
    0,                                         /* Key size, not used     */
    0,                                         /* IV size, not used      */
    NX_CRYPTO_HMAC_SHA256_ICV_FULL_LEN_IN_BITS,/* Transmitted ICV size   */
    NX_CRYPTO_SHA2_BLOCK_SIZE_IN_BYTES,        /* Block size in bytes    */
    sizeof(NX_CRYPTO_SHA256_HMAC),             /* Metadata size in bytes */
    _nx_crypto_method_hmac_sha256_init,        /* HMAC SHA256 init       */
    _nx_crypto_method_hmac_sha256_cleanup,     /* HMAC SHA256 cleanup    */
    _nx_crypto_method_hmac_sha256_operation    /* HMAC SHA256 operation  */
};

/* Define the hash key being used. */
const CHAR hash_key = "my hash key";

/* Define starting address. */
UINT starting_address = 0x10000;

/* Define the ending address.
   Note that the hash computation covers the memory location
   before the ending address. */
UINT ending_address = 0x11000;

/* Declare the output buffer. */
#define OUTPUT_BUFFER_SIZE
UCHAR output_buffer[OUTPUT_BUFFER_SIZE];

UINT actual_size;

/* Declare 1K bytes of metadata buffer area. */
UCHAR metadata_buffer[1024];

/* Compute the hash value of the data between 0x10000 – 0x10FFF */
nx_secure_module_hash_compute(&hmac_sha256,
                              starting_address, ending_address,
                              hash_key, strlen(hash_key),
                              metadata_buffer, sizeof(metadata_buffer),
                              output_buffer, OUTPUT_BUFFER_SIZE, &actual_size);

/* If this function returns "0", the hash value has been computed and is stored
   in output_buffer. */
```

### See Also

- nx_secure_crypto_method_self_test

## nx_secure_tls_active_certificate_set

Set the active identity certificate for a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_active_certificate_set(
                   NX_SECURE_TLS_SESSION *tls_session,
                   NX_SECURE_X509_CERT *certificate);
```

### Description

This service is intended to be called from within a session callback (see nx_secure_tls_session_client_callback_set and nx_secure_tls_session_server_callback_set). When called with a previously-initialized NX_SECURE_X509_CERT structure, that certificate will be used instead of the default identity certificate. In most cases, the certificate must have been added to the local store (see nx_secure_tls_local_certificate_add) or the TLS handshake may fail.

This service is intended to allow TLS to support multiple identity certificates. This is useful for a TLS server that services multiple network addresses so the server can pick an appropriate certificate to
provide to the remote client depending on the client's entrypoint. For a TLS client, this routine may be used to change the certificate sent to a remote server at runtime after the server has identified itself in the TLS handshake (this is more rare than the TLS server use-case).

In the case where multiple certificates may share the same X.509 distinguished name, certificates will need to be added using nx_secure_tls_server_certificate_add, which introduces a numeric identifier separate from the certificate.

### Parameters

- **session_ptr** Pointer to a TLS Session instance passed into the session callback.
- **certificate** Pointer to an initialized X.509 certificate that is to be used for the current session.

### Return Values

- **NX_SUCCESS** (0x00) Successful assignment of certificate to session.
- **NX_PTR_ERROR** (0x07) Invalid TLS session or certificate pointer.

### Allowed From

Threads

### Example

```C
#define TLS_SNI_SERVER_NAME "www.example.com"
#define TLS_DEFAULT_SERVER_CERT_ID 2
#define TLS_EXAMPLE_CERT_ID 3


/* Callback for ClientHello extensions processing. */
static ULONG tls_server_callback(NX_SECURE_TLS_SESSION *tls_session,
                                 NX_SECURE_TLS_HELLO_EXTENSION *extensions,
                                 UINT num_extensions)
{
NX_SECURE_X509_DNS_NAME dns_name;
INT compare_value;
UINT status;
NX_SECURE_X509_CERT *cert_ptr;

    /* Grab a pointer to our default certificate in case the SNI extension is not
       available or the specified server name is unknown. */
    nx_secure_tls_server_certificate_find(tls_session, &cert_ptr,
                                     TLS_DEFAULT_SERVER_CERT_ID);


    /* Process Server Name Indication extension. */
    status = _nx_secure_tls_session_sni_extension_parse(tls_session, extensions,
                                                    num_extensions, &dns_name);

    /* If no extension found, that is OK. Use default certificate. */
    if(status == NX_SUCCESS)
    {
          /* SNI extension found, select an active certificate based on the server
             name. */

          /* Make sure our SNI name matches. */
          compare_value = memcmp(dns_name.nx_secure_x509_dns_name,
                                TLS_SNI_SERVER_NAME, strlen(TLS_SNI_SERVER_NAME));

          if(compare_value == 0 && dns_name.nx_secure_x509_dns_name_length !=
             strlen(TLS_SNI_SERVER_NAME))
          {
             /* Found a match, find the certificate we want to use. */
             _nx_secure_tls_server_certificate_find(tls_session, &cert_ptr,
                                                      TLS_EXAMPLE_CERT_ID);
          }
          else
          {
             /* No matching server name found. The application may choose to simply
                provide the default certificate (and probably resulting in an error
                in the remote client) or returning an error here to end the
                handshake. A user-defined non-zero error code will be sufficient –
                the error code will abort the TLS handshake and be returned to the
                caller that started the server. */
                return(0x500);
          }
      }
      else
      {
      }
      /* Set the certificate we want to use. */
      nx_secure_tls_active_certificate_set(tls_session, cert_ptr);

      /* Return success to continue TLS handshake. */
      return(NX_SUCCESS);
}

/* Sockets, sessions, certificates defined in global static space to preserve
   application stack. */
NX_SECURE_TLS_SESSION server_tls_session;

/* Application where TLS session is started. */
void main()
{
    /* Create a TLS session for our socket. Ciphers and metadata defined
       elsewhere. See nx_secure_tls_session_create reference for more
       information. */
    status =  nx_secure_tls_session_create(&server_tls_session,
                                           &nx_crypto_tls_ciphers,
                                           server_crypto_metadata,
                                           sizeof(server_crypto_metadata));

    /* Check for error.  */
    if(status)
    {
        printf("Error in function nx_secure_tls_session_create: 0x%x\n", status);
    }

    /* Setup our packet reassembly buffer. See
       nx_secure_tls_session_packet_buffer_set for more information. */
    nx_secure_tls_session_packet_buffer_set(&server_tls_session,
                                      server_packet_buffer,
                                      sizeof(server_packet_buffer));


    /* Set callback for server TLS extension handling. */
    _nx_secure_tls_session_server_callback_set(&server_tls_session,
                                              tls_server_callback);

    /* Initialize our certificates – certificate data defined elsewhere. See
       section "Importing X.509 Certificates into NetX Duo Secure" for more
       information. */
    nx_secure_x509_certificate_initialize(&default_certificate,
                                      default_cert_der,
                                      default _cert_der_len, NX_NULL, 0,
                                      default_cert_key_der,
                                      default_cert_key_der_len,
                                      NX_SECURE_X509_KEY_TYPE_RSA_PKCS1_DER);

    /* Add the certificate to the local store using a numeric ID. */
    nx_secure_tls_server_certificate_add(&server_tls_session,
                                         &default_certificate,
                                         TLS_DEFAULT_SERVER_CERT_ID);

    /* Alternative identity certificate, needs to have a private key! */
    nx_secure_x509_certificate_initialize(&example_certificate,
                                      example_cert_der,
                                      example cert_der_len, NX_NULL, 0,
                                      example_cert_key_der,
                                      example_cert_key_der_len,
                                      NX_SECURE_X509_KEY_TYPE_RSA_PKCS1_DER);

    /* Add the certificate to the local store using a numeric ID. */
    nx_secure_tls_server_certificate_add(&server_tls_session,
                                         &example_certificate,
                                         TLS_EXAMPLE_CERT_ID);

    /* Now we can start the TLS session as normal. */
       …
}
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_local_certificate_add
- nx_secure_tls_session_client_callback_set
- nx_secure_tls_session_server_callback_set
- nx_secure_tls_server_certificate_add
- nx_secure_tls_server_certificate_find
- nx_secure_tls_server_certificate_remove

## nx_secure_tls_client_psk_set

Set a Pre-Shared Key for a NetX Duo Secure TLS Client Session

### Prototype

```C
UINT  nx_secure_tls_client_psk_set(NX_SECURE_TLS_SESSION *session_ptr,
                              UCHAR *pre_shared_key, UINT psk_length,
                              UCHAR *psk_identity, UINT identity_length,
                              UCHAR *hint, UINT hint_length);
```

### Description

This service adds a Pre-Shared Key (PSK), its identity string, and an identity hint to a TLS Session control block, and sets that PSK to be used in subsequent TLS Client connections. The PSK is used in place of a digital certificate when PSK ciphersuites are enabled and used.

In this case, the PSK is associated with a specific remote TLS Server with which the TLS Client wishes to communicate. The PSK set through this API will be provided to the remote TLS Server host during the next TLS handshake.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **pre_shared_key** The actual PSK value.
- **psk_length** The length of the PSK value.
- **psk_identity** A string used to identify this PSK value.
- **identity_length** The length of the PSK identity.
- **hint** A string used to indicate which group of PSKs to choose from on a TLS server.
- **hint_length** The length of the hint string.

### Return Values

- **NX_SUCCESS** (0x00) Successful addition of PSK.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.
- **NX_SECURE_TLS_NO_MORE_PSK_SPACE** (0x125) Cannot add another PSK.

### Allowed From

Threads

### Example

```C
/* PSK value.  */
UCHAR psk[] = { 0x1a, 0x2b, 0x3c, 0x4d };

/* Add PSK to TLS session.  */
status =  nx_secure_tls_client_psk_set(&tls_session, psk, sizeof(psk), "psk_1", 4,
"Client_identity", 15);


/* If status is NX_SUCCESS the PSK was successfully added.  */
```

### See Also

- nx_secure_tls_psk_add
- nx_secure_x509_certificate_initialize
- nx_secure_tls_session_create
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_local_certificate_add

## nx_secure_tls_initialize

Initializes the NetX Duo Secure TLS module

### Prototype

```C
VOID nx_secure_tls_initialize(VOID);
```

### Description

This service initializes the NetX Duo Secure TLS module. It must be called before other NetX Duo Secure services can be accessed.

### Parameters

None

### Return Values

None

### Allowed From

Initialization, Threads

### Example

```C
/* Initializes the TLS module. */
Nx_secure_tls_initialize();
```

### See Also

- nx_secure_tls_session_create

## nx_secure_tls_local_certificate_add

Add a local certificate to NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_local_certificate_add(
              NX_SECURE_TLS_SESSION *session_ptr,
              NX_SECURE_X509_CERT *certificate_ptr);
```

### Description

This service adds an initialized NX_SECURE_X509_CERT structure instance to the local store of a TLS session. This certificate may used by the TLS stack to identify the device during the TLS handshake (if it was marked as an identity certificate during initialization of the certificate structure using nx_secure_x509_certificate_initialize), or as an issuer as part of a certificate chain provided to the remote host during the TLS handshake.

If multiple local certificates with the same Common Name are needed, certificates may be added using the *nx_secure_tls_server_certificate_add* service (see warning below).

A certificate is **required** for TLS Server mode.

A certificate is *optional* for TLS Client
mode.

> **Important:** *This API should not be used with the same TLS session when using nx_secure_tls_server_certificate_add. The server certificate API uses a unique numeric identifier for each certificate, and nx_secure_tls_local_certificate_add indexes based on the X.509 Common Name. The local certificate services provide a convenient alternative to the numeric identifier for applications that use only a single identity certificate – by using the Common Name, the application need not keep track of numeric identifiers.*

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **certificate_ptr** Pointer to an initialized TLS Certificate instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful addition of certificate.
- **NX_INVALID_PARAMETERS** (0x4D) Tried to add an invalid or duplicate certificate.
- **NX_PTR_ERROR** (0x07) Invalid TLS session or certificate pointer.

### Allowed From

Threads

### Example

```C
/* Initialize certificate structure. */
status =  nx_secure_x509_certificate_initialize(&certificate, certificate_data,
500, private_key, 64);

/* Add certificate to TLS session.  */
status =  nx_secure_tls_local_certificate_add(&tls_session, &certificate);


/* If status is NX_SUCCESS the certificate was successfully added.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_session_create
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_local_certificate_remove
- nx_secure_tls_local_certificate_find

## nx_secure_tls_local_certificate_find

Find a local certificate in a NetX Duo Secure TLS Session by Common Name

### Prototype

```C
UINT  nx_secure_tls_local_certificate_find(NX_SECURE_TLS_SESSION
                        *session_ptr, NX_SECURE_X509_CERT
                        **certificate, UCHAR *common_name, UINT
                        name_length);
```

### Description

This service finds a certificate in the local device certificate store of a TLS session and returns a pointer to the NX_SECURE_X509_CERT structure in the store. The common_name parameter and it's length (name_length) are used to identify the certificate in the store by matching the certificate's X.509 Subject Common Name field.

If more than one certificate with the same Common Name is present, only
the first one will be returned – use
*nx_secure_tls_server_certificate_find*
instead.

> **Important:** *This API should not be used with the same TLS session when using nx_secure_tls_server_certificate_add. The server certificate API uses a unique numeric identifier for each certificate, and nx_secure_tls_local_certificate_add indexes based on the X.509 Common Name. The local certificate services provide a convenient alternative to the numeric identifier for applications that use only a single identity certificate – by using the Common Name, the application need not keep track of numeric identifiers.*

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **certificate** Return Pointer to matched certificate.
- **common_name** Common Name string to match (DNS name).
- **name_length** Length of common_name string data.

### Return Values

- **NX_SUCCESS** (0x00) Certificate was found and pointer returned in "certificate" parameter.
- **NX_SECURE_TLS_CERTIFICATE_NOT_FOUND** (0x119) No certificate with the supplied Common Name was found.
- **NX_PTR_ERROR** (0x07) Invalid TLS session, certificate pointer, or common name string.

### Allowed From

Threads

### Example

```C
NX_SECURE_X509_CERT *certificate_ptr;

/* Initialize certificate structure. */
status =  nx_secure_x509_certificate_initialize(&certificate, certificate_data,
500, private_key, 64);

/* Add certificate to TLS session.  */
status =  nx_secure_tls_local_certificate_add(&tls_session, &certificate);


/* If status is NX_SUCCESS the certificate was successfully added.  */

status = nx_secure_tls_local_certificate_find(&tls_session, &certificate_ptr,
                                      "common name", strlen("common name"));

/* If status is NX_SUCCESS the variable "certificate_ptr" points to a certificate
   structure containing a certificate with the Common Name "common name". */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_session_create
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_local_certificate_remove
- nx_secure_tls_local_certificate_add

## nx_secure_tls_local_certificate_remove

Remove local certificate from NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_local_certificate_remove(NX_SECURE_TLS_SESSION
                  *session_ptr, UCHAR *common_name, UINT
                  common_name_length);
```

### Description

This service removes a local certificate instance from a TLS session, keyed on the Common Name field in the certificate.

> **Important:** *This API should not be used with the same TLS session when using nx_secure_tls_server_certificate_add. The server certificate API uses a unique numeric identifier for each certificate, and nx_secure_tls_local_certificate_add indexes based on the X.509 Common Name. The local certificate services provide a convenient alternative to the numeric identifier for applications that use only a single identity certificate – by using the Common Name, the application need not keep track of numeric identifiers.*

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **common_name** The Common Name value of the certificate to be removed.
- **common_name_length** The length of the Common Name string.

### Return Values

- **NX_SUCCESS** (0x00) Successful addition of certificate.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.
- **NX_SECURE_TLS_CERTIFICATE_NOT_FOUND** (0x119) Certificate was not found.

### Allowed From

Threads

### Example

```C
/* Add certificate to TLS session.  */
status =  nx_secure_tls_local_certificate_remove(&tls_session,
                                                "www.example.com", 15);


/* If status is NX_SUCCESS the certificate was successfully removed.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_session_create
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_local_certificate_add

## nx_secure_tls_metadata_size_calculate

Calculate size of cryptographic metadata for a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_metadata_size_calculate(
                        const NX_SECURE_TLS_CRYPTO *crypto_table,
                        ULONG *metadata_size);
```

### Description

This service calculates and returns the size of the cryptographic metadata needed for a particular TLS session and TLS cryptography table (see the section "Initializing TLS with Cryptographic Methods" for more information on the cryptographic cipher table).

This service should be called with the desired cryptographic table to calculate the size of the metadata buffer passed into nx_secure_tls_session_create.

### Parameters

- **crypto_table** Pointer to a complete NetX Duo Secure TLS cryptography table.
- **metadata_size** The output of the size calculation in bytes.

### Return Values

- **NX_SUCCESS** (0x00) Successful calculation of metadata size.
- **NX_PTR_ERROR** (0x07) Invalid crypto table or return size pointer.

### Allowed From

Threads

### Example

```C
/* Pointer to the platform-specific cipher table. */
extern nx_crypto_tls_ciphers;

/* Return variable for metadata size. */
ULONG crypto_metadata_size;

/* Calculate metadata size.  */
status =  nx_secure_tls_metadata_size_calculate(&nx_crypto_tls_ciphers,
                                                &crypto_metadata_size);


/* If status is NX_SUCCESS then crypto_metadata_size contains the size of the
   metadata buffer for the table nx_crypto_tls_ciphers.  */
```

### See Also

- nx_secure_tls_session_create

## nx_secure_tls_packet_allocate

Allocate a packet for a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_packet_allocate(NX_SECURE_TLS_SESSION *session_ptr,
                                    NX_PACKET_POOL *pool_ptr,
                                    NX_PACKET **packet_ptr,
                                    ULONG wait_option);
```

### Description

This service allocates an NX_PACKET for the specified active TLS session from the specified NX_PACKET_POOL. This service should be called by the application to allocate data packets to be sent over a TLS connection. The TLS session must be initialized before calling this service.

The allocated packet is properly initialized so that TLS header and footer data may be added after the packet data is populated. The behavior is otherwise identical to *nx_packet_allocate*.

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **pool_ptr** Pointer to an NX_PACKET_POOL from which to allocate the packet.
- **packet_ptr** Output pointer to the newly-allocated packet.
- **wait_option** Suspension option for packet allocation.

### Return Values

- **NX_SUCCESS** (0x00) Successful packet allocate.
- **NX_SECURE_TLS_ALLOCATE_PACKET_FAILED** (0x111) Underlying packet allocation failed.
- **NX_SECURE_TLS_SESSION_UNINITIALIZED** (0x101) The supplied TLS session was not initialized.

### Allowed From

Threads

### Example

```C
/* Allocate a new TLS packet from the previously created packet pool and
previously initialized TLS session.   */

status = nx_secure_tls_packet_allocate(&tls_session, &pool_0, &packet_ptr,
                                       NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the newly allocated packet pointer is found in the
variable packet_ptr.  */
```

### See Also

- nx_tcp_socket_receive
- nx_secure_x509_certificate_initialize
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_session_start
- nx_secure_tls_session_delete
- nx_secure_tls_session_receive
- nx_secure_tls_session_send
- nx_secure_tls_session_end
- nx_secure_tls_session_create

## nx_secure_tls_psk_add

Add a Pre-Shared Key to a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_psk_add(NX_SECURE_TLS_SESSION *session_ptr,
                            UCHAR *pre_shared_key, UINT psk_length,
                            UCHAR *psk_identity, UINT
                            identity_length, UCHAR *hint, UINT
                            hint_length);
```

### Description

This service adds a Pre-Shared Key (PSK), its identity string, and an identity hint to a TLS Session control block. The PSK is used in place of a digital certificate when PSK ciphersuites are enabled and used.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **pre_shared_key** The actual PSK value.
- **psk_length** The length of the PSK value.
- **psk_identity** A string used to identify this PSK value.
- **identity_length** The length of the PSK identity.
- **hint** A string used to indicate which group of PSKs to choose from on a TLS server.
- **hint_length** The length of the hint string.

### Return Values

- **NX_SUCCESS** (0x00) Successful addition of PSK.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.
- **NX_SECURE_TLS_NO_MORE_PSK_SPACE** (0x125)  Cannot add another PSK.

### Allowed From

Threads

### Example

```C
/* PSK value.  */
UCHAR psk[] = { 0x1a, 0x2b, 0x3c, 0x4d };

/* Add PSK to TLS session.  */
status =  nx_secure_tls_psk_add(&tls_session, psk, sizeof(psk), "psk_1", 4,
"Client_identity", 15);


/* If status is NX_SUCCESS the PSK was successfully added.  */
```

### See Also

- nx_secure_tls_client_psk_set
- nx_secure_x509_certificate_initialize
- nx_secure_tls_session_create
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_local_certificate_add

## nx_secure_tls_remote_certificate_allocate

Allocate space for the certificate provided by a remote TLS host

### Prototype

```C
UINT  nx_secure_tls_remote_certificate_allocate(
                 NX_SECURE_TLS_SESSION *session_ptr,
                 NX_SECURE_X509_CERT *certificate_ptr,
                 UCHAR *raw_certificate_buffer,
                 UINT raw_buffer_size);
```

### Description

This service adds an uninitialized NX_SECURE_X509_CERT structure instance to a TLS session for the purpose of allocating space for certificates provided by a remote host during a TLS session. The remote certificate data is parsed by NetX Duo Secure TLS and that information is used to populate the certificate structure instance provided to this function. Certificates added in this manner are placed in a linked list.

If it is expected that the remote host will provide multiple certificates, this function should be called repeatedly to allocate space for all certificates. The additional certificates are added to the end of the certificate linked list.

Failure to allocate a remote certificate will cause TLS Client mode to fail during the TLS handshake unless a Pre-Shared Key (PSK) ciphersuite is in use.

The *raw_certificate_buffer* parameter points to space allocated to store the incoming remote certificate. Typical certificates with RSA keys of 2048 bits using SHA-256 for signatures are in the range of 1000-2000 bytes. The buffer should be large enough to at least hold that size, but depending on the remote host certificates may be significantly smaller or larger. Note that if the buffer is too small to hold the incoming certificate, the TLS handshake will end with an error.

For TLS Server mode, a remote certificate allocation is needed only if client certificate authentication is enabled.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **certificate_ptr** Pointer to an uninitialized X.509 Certificate instance.
- **raw_certificate_buffer** Pointer to a buffer to hold the un-parsed certificate received from the remote host.
- **raw_buffer_size** Size of the raw certificate buffer.

### Return Values

- **NX_SUCCESS** (0x00) Successful allocation of certificate.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.
- **NX_SECURE_TLS_INSUFFICIENT_CERT_SPACE** (0x12D) The supplied buffer was too small.
- **NX_INVALID_PARAMETERS** (0x4D) Tried to add an invalid certificate.

### Allowed From

Threads

### Example

```C
/* Certificate control block. */
NX_SECURE_X509_CERT certificate;

/* Buffer space to hold the incoming certificate. */
UCHAR certificate_buffer[2000];

/* Add certificate to TLS session.  */
status =  nx_secure_tls_remote_certificate_allocate(&tls_session, &certificate,
                                                    certificate_buffer,
                                                    sizeof(certificate_buffer));


/* If status is NX_SUCCESS the certificate was successfully allocated.  */
```

### See Also

- nx_secure_x509_certificate_initialize,
- nx_secure_tls_session_create

## nx_secure_tls_remote_certificate_buffer_allocate

Allocate space for all certificates provided by a remote TLS host

### Prototype

```C
UINT  nx_secure_tls_remote_certificate_buffer_allocate(
                  NX_SECURE_TLS_SESSION *session_ptr,
                  UINT certs_number, VOID *certificate_buffer,
                  ULONG buffer_size);
```

### Description

This service allocates space to process incoming certificate chains from remote server hosts in order to perform X.509 authentication and verification in a TLS Client instance. For TLS Server mode, remote certificate allocation is needed only if client X.509 certificate authentication is enabled – for TLS Server instances the service *nx_secure_tls_session_x509_client_verify_configure* should be used instead.

Failure to allocate remote certificates will cause TLS Client mode to fail during the TLS handshake unless a Pre-Shared Key (PSK) ciphersuite is in use.

The *certificate_buffer* parameter points to space allocated to store the incoming remote certificates and the control blocks needed for those certificates. The buffer will be divided by the number of certificates (*certs_number*) with an equal portion given to each certificate. The *buffer_size*  parameter indicates the size of the buffer. The space needed can be found with the following formula:

```C
buffer_size = (<expected max number of certificates in chain>) *
                 (sizeof(NX_SECURE_X509_CERT) + <max cert size>)
```

Typical certificates with RSA keys of 2048 bits using SHA-256 for signatures are in the range of 1000-2000 bytes. The buffer should be large enough to at least hold that size for each certificate, but depending on the remote host certificates may be significantly smaller or larger. Note that if the buffer is too small to hold the incoming certificate, the TLS handshake will end with an error.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session  instance.
- **certs_number** Number of certificates to allocate from the  provided buffer.
- **certificate_buffer** Pointer to a buffer to hold certificates received  from a remote host.
- **buffer_size** Size of the certificate buffer.

### Return Values

- **NX_SUCCESS** (0x00) Successful allocation of certificate.
- **NX_PTR_ERROR** (0x07) Invalid TLS session or buffer pointer.
- **NX_SECURE_TLS_INSUFFICIENT_CERT_SPACE** (0x12D) The supplied buffer was too small.
- **NX_INVALID_PARAMETERS** (0x4D) The buffer was too small to hold  the desired number of certificates.

### Allowed From

Threads

### Example

```C
/* Buffer space to hold the incoming certificates. */
#define CERTIFICATE_NUMBER    (2)
#define CERTIFICATE_SIZE      (2000)
#define BUFFER_SIZE           (CERTIFICATE_NUMBER * \
                              (sizeof(NX_SECURE_X509_CERT) + CERTIFICATE_SIZE))
UCHAR certificate_buffer[BUFFER_SIZE];

/* Add certificate to TLS session.  */
status =  nx_secure_tls_remote_certificate_buffer_allocate(&tls_session,
                                                      CERTIFICATE_NUMBER,
                                                      certificate_buffer,
                                                      BUFFER_SIZE);


/* If status is NX_SUCCESS the buffer was successfully allocated.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_session_create

## nx_secure_tls_remote_certificate_free_all

Free space allocated for incoming certificates

### Prototype

```C
UINT  nx_secure_tls_remote_certificate_free_all(
                  NX_SECURE_TLS_SESSION *session_ptr);
```

### Description

This service is used to free all of the certificate buffers allocated to a particular TLS Session by nx_secure_tls_remote_certificate_allocated by returning them to that session's free certificate space. This may be necessary if an application reuses a TLS session object without deleting it and recreating it with nx_secure_tls_session_delete and nx_secure_tls_session_create.

Note that the remote certificate space is recovered automatically when the TLS session is reset as happens in nx_secure_tls_session_end so most applications should not need to call this service.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful operation.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.
- **NX_INVALID_PARAMETERS** (0x4D) Internal error – certificate store likely corrupt.

### Allowed From

Threads

### Example

```C
/* Certificate control block. */
NX_SECURE_X509_CERT certificate;

/* Buffer space to hold the incoming certificate. */
UCHAR certificate_buffer[2000];

/* Add certificate to TLS session.  */
status =  nx_secure_tls_remote_certificate_allocate(&tls_session, &certificate,
                                                    certificate_buffer,
                                                    sizeof(certificate_buffer));


/* If status is NX_SUCCESS the certificate was successfully allocated.  */

/* … TLS session setup, TCP connection, etc… */

/* End TLS session. */
nx_secure_tls_session_end(&tls_session, NX_WAIT_FOREVER);

/* Free up certificate buffers for next connection. */
nx_secure_tls_remote_certificate_free_all(&tls_session);
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_session_create
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_session_end

## nx_secure_tls_server_certificate_add

Add a certificate specifically for TLS servers using a numeric identifier

### Prototype

```C
UINT  nx_secure_tls_server_certificate_add(
                  NX_SECURE_TLS_SESSION *session_ptr,
                  NX_SECURE_X509_CERT *certificate, UINT cert_id);
```

### Description

This service is used to add a certificate to a TLS session's local store (see nx_secure_tls_local_certificate_add) using a numeric identifier instead of indexing the store using the X.509 Subject (Common Name) within the certificate. The numeric identifier is separate from the certificate and allows multiple certificates to be added as identity certificates to a TLS server, as well as allowing multiple certificates with the same Common Name to be added to the same TLS session local store. This same service can be used for client certificates, but it is rare for a TLS client to have multiple identity certificates.

The cert_id parameter is a non-zero positive integer assigned by the application. The actual value does not matter (other than zero) but it must be unique in the store for the provided TLS session. The cert_id value can be used to find and remove certificates from the local store using nx_secure_tls_server_certificate_find and nx_secure_tls_server_certificate_remove, respectively.

> **Important:** *This API should not be used with the same TLS session when using nx_secure_tls_local_certificate_add. The nx_secure_tls_server_certificate_add API uses a unique numeric identifier for each certificate, and local certificate services index based on the X.509 Common Name. The server certificate services allow multiple certificates with shared data (especially Common Name) to exist in the same local store – this is useful for a server with multiple identities.*

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **certificate** Pointer to a previously initialized X.509 certificate instance.
- **cert_id** Positive, non-zero, relatively unique certificate ID number.

### Return Values

- **NX_SUCCESS** (0x00)Successful operation.
- **NX_PTR_ERROR** (0x07) Invalid TLS session orcertificate pointer.
- **NX_SECURE_TLS_CERT_ID_INVALID** (0x138) The provided certificate ID had An invalid value (likely 0).
- **NX_SECURE_TLS_CERT_ID_DUPLICATE** (0x139) The provided certificate ID was already present in the local store.
- **NX_INVALID_PARAMETERS(0x4D)** Internal error – certificate store likely corrupt.

### Allowed From

Threads

### Example

```C
/* Certificate control block. */
NX_SECURE_X509_CERT certificate;


/* Add certificate to TLS session.  */
status =  nx_secure_tls_server_certificate_add(&tls_session, &certificate, 0x12);


/* If status is NX_SUCCESS the certificate was successfully added with the ID
0x12.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_local_certificate_add
- nx_secure_tls_active_certificate_set
- nx_secure_tls_server_certificate_find
- nx_secure_tls_server_certificate_remove

## nx_secure_tls_server_certificate_find

Find a certificate using a numeric identifier

### Prototype

```C
UINT  nx_secure_tls_server_certificate_find(
                  NX_SECURE_TLS_SESSION *session_ptr,
                  NX_SECURE_X509_CERT **certificate, UINT cert_id);
```

### Description

This service is used to find a certificate in a TLS session's local store (see nx_secure_tls_local_certificate_add) using a numeric identifier instead of indexing the store using the X.509 Subject (Common Name) within the certificate.

The cert_id parameter is a non-zero positive integer assigned by the application when the certificate is added to the TLS session local store using nx_secure_tls_server_certificate_add.

> **Important:** *This API should not be used with the same TLS session when using nx_secure_tls_local_certificate_add. The nx_secure_tls_server_certificate_add API uses a unique numeric identifier for each certificate, and local certificate services index based on the X.509 Common Name. The server certificate services allow multiple certificates with shared data (especially Common Name) to exist in the same local store – this is useful for a server with multiple identities.*

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **certificate** Pointer to an X.509 certificate pointer to return a reference to the found certificate.
- **cert_id** Non-zero positive certificate ID value.

### Return Values

- **NX_SUCCESS** (0x00)Successful operation.
- **NX_PTR_ERROR** (0x07) Invalid TLS session or certificate pointer.
- **NX_SECURE_TLS_CERTIFICATE_NOT_FOUND** (0x119) The provided certificate ID did not match any in the local store of the provided TLS session.

### Allowed From

Threads

### Example

```C
NX_SECURE_X509_CERT *certificate;


/* Find certificate in TLS session.  */
status =  nx_secure_tls_server_certificate_find(&tls_session, &certificate, 0x12);


/* If status is NX_SUCCESS the certificate was successfully found and a reference
returned in the certificate parameter.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_local_certificate_add
- nx_secure_tls_active_certificate_set
- nx_secure_tls_server_certificate_add
- nx_secure_tls_server_certificate_remove

##  nx_secure_tls_server_certificate_remove

Remove a local server certificate using a numeric identifier

### Prototype

```C
UINT  nx_secure_tls_server_certificate_remove(
                  NX_SECURE_TLS_SESSION *session_ptr, UINT cert_id);
```

### Description

This service is used to remove a certificate from a TLS session's local store (see nx_secure_tls_local_certificate_add) using a numeric identifier instead of indexing the store using the X.509 Subject (Common Name) within the certificate.

The cert_id parameter is a non-zero positive integer assigned by the application when the certificate is added to the TLS session local store using nx_secure_tls_server_certificate_add.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **cert_id** Non-zero positive certificate ID value.

### Return Values

- **NX_SUCCESS** (0x00)Successful operation.
- **NX_PTR_ERROR** (0x07) Invalid TLS session.
- **NX_SECURE_TLS_CERTIFICATE_NOT_FOUND** (0x119) The provided certificate ID did not match any in the local store of the provided TLS session.

### Allowed From

Threads

### Example

```C
/* Certificate control block. */
NX_SECURE_X509_CERT certificate;


/* Add certificate to TLS session with id 0x12.  */
status =  nx_secure_tls_server_certificate_add(&tls_session, &certificate, 0x12);
/* Use certificate in TLS session, etc… */

/* Remove certificate from TLS session with id 0x12.  */
status =  nx_secure_tls_server_certificate_remove(&tls_session, 0x12);


/* If status is NX_SUCCESS the certificate was successfully removed.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_local_certificate_add
- nx_secure_tls_active_certificate_set
- nx_secure_tls_server_certificate_add
- nx_secure_tls_server_certificate_find

## nx_secure_tls_session_alert_value_get

Get the TLS alert value and level sent by the remote host

### Prototype

```C
UINT  nx_secure_tls_session_alert_value_get(
                   NX_SECURE_TLS_SESSION *session_ptr,
                   UINT *alert_level, UINT *alert_value);
```

### Description

This service is used to retrieve the TLS alert level and value when the remote host sends an alert in response to some problem or error.

The values of the alert_level and alert_value parameters are only valid if this function is called immediately following a TLS API call that returned a status of NX_SECURE_TLS_ALERT_RECEIVED (0x114) indicating that an alert was received from the remote host.

Note that if the local host TLS sends an alert, the returned error codes are far more descriptive of the actual error than the TLS alert itself because TLS alert values are intentionally left ambiguous to prevent certain types of attack (such as a "padding oracle" attack or similar).

The alert level only takes one of two values: NX_SECURE_TLS_ALERT_LEVEL_WARNING (0x1) or NX_SECURE_TLS_ALERT_LEVEL_FATAL (0x2). In general, only the CloseNotify Alert (used to indicate a successful end to a TLS session) will be given a level of "Warning" though some extension configuration situations may also be considered warnings. The vast majority of alerts will be "Fatal" indicating a potential security failure and resulting in immediate closure of the TLS connection (handshake or session).

The TLS alert values are defined in the TLS RFCs, here is the list from RFC 5246 (TLSv1.2) for reference:

| Alert Name                     | Value | Description                                                                                                                                                  |
| ---------------------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| close_notify                  | 0     | No error, indicates successful session end                                                                                                                   |
| unexpected_message            | 10    | TLS received an unexpected or out-of-order message                                                                                                           |
| bad_record_mac               | 20    | Decryption and/or MAC verification failed                                                                                                                    |
| decryption_failed_RESERVED   | 21    | **DEPRECATED** Decryption failed (deprecated due to padding oracle attacks)                                                                                      |
| record_overflow               | 22    | A record was received that is larger than the TLS maximum record size                                                                                        |
| decompression_failure         | 30    | A problem was encountered in compression/decompression of a TLS record                                                                                       |
| handshake_failure             | 40    | Some unspecified handshake error occurred that isn't covered by a different alert                                                                            |
| no_certificate_RESERVED      | 41    | **DEPRECATED** in TLS (SSL only)                                                                                                                                 |
| bad_certificate               | 42    | A certificate that was received contained invalid formatting or signatures                                                                                   |
| unsupported_certificate       | 43    | A certificate was received that was of an invalid type (e.g. signing-only certificate used for TLS server authentication)                                    |
| certificate_revoked           | 44    | The certificate status (as provided by a CRL or OCSP) was indicated as being "revoked"                                                                       |
| certificate_expired           | 45    | The received certificate was outside it's valid date range (either not valid yet or expired)                                                                 |
| certificate_unknown           | 46    | Some unknown certificate issue was encountered that was not covered by other alerts                                                                          |
| illegal_parameter             | 47    | Some configuration or negotiated value in the TLS handshake was invalid or out of range                                                                      |
| unknown_ca                    | 48    | The received identity certificate could not be traced via a certificate chain to a trusted root CA certificate.                                              |
| access_denied                 | 49    | Indicates a valid certificate was received but application access control indicated that the certificate was invalid for the requested resources.            |
| decode_error                  | 50    | Some field or value in a TLS header or handshake message was out of range or invalid, leading to a failure in decoding of a TLS record.                      |
| decrypt_error                 | 51    | A signature or Finished message hash during the TLS handshake could not be verified.                                                                         |
| export_restriction_RESERVED  | 60    | DEPRECATED in TLSv1.2                                                                                                                                        |
| protocol_version              | 70    | The TLS protocol version negotiated during the handshake is recognized but not supported (e.g. TLSv1.0 was presented but not enabled).                       |
| insufficient_security         | 71    | Sent whenever a handshake fails due to a lack of secure ciphers (e.g. key size is too small for the application requirements)                                |
| internal_error                | 80    | Some non-TLS error (e.g. memory allocation problems, application issues) occurred resulting in a broken TLS session.                                         |
| user_canceled                 | 90    | Returned if the TLS session is cancelled by a user or application before the handshake is complete (similar to CloseNotify).                                 |
| no_renegotiation              | 100   | Indicates that the remote host is not willing to perform TLS renegotiation handshakes in response to a renegotiation request.                                 |
| unsupported_extension         | 110   | Sent if a TLS Client receives a ServerHello containing extensions not explicitly asked for in the initial ClientHello (indicating the server has a problem). |

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **alert_level** Return the level of the received alert (warning or fatal).
- **alert_value** Return the value of the received alert (see table).

### Return Values

- **NX_SUCCESS** (0x00) Successful operation.
- **NX_PTR_ERROR** (0x07) Invalid TLS session.

### Allowed From

Threads

### Example

```C
/* Return values. */
UINT status, alert_level, alert_value;


/* Start a TLS session.  */
status =  nx_secure_tls_session_start(&tls_session, &tcp_socket, NX_WAIT_FOREVER);

/* Check for "alert received" error. */
if(status == NX_SECURE_TLS_ALERT_RECEIVED)
{

        /* Get the alert level and value. */
        status =  nx_secure_tls_session_alert_value_get(&tls_session,
                                             &alert_level, &alert_value);

        /* Print out the received alert level and value. */
        printf("Alert received. Value: %d, Level: %d\n", alert_value,
                alert_level);
}
else if(status != NX_SUCCESS)
{
    /* Additional error handling. */
}
```

### See Also

- nx_secure_tls_session_start
- nx_secure_tls_session_send
- nx_secure_tls_session_receive

## nx_secure_tls_session_certificate_callback_set

Set up a callback for TLS to use for additional certificate validation

### Prototype

```C
UINT  nx_secure_tls_ session_certificate_callback_set (
                  NX_SECURE_TLS_SESSION *tls_session,
                  ULONG (*func_ptr)(NX_SECURE_TLS_SESSION *session,
                                    NX_SECURE_X509_CERT *certificate));
```

### Description

This service assigns a function pointer to a TLS session that TLS will invoke when a certificate is received from a remote host, allowing the application to perform validation checks such as DNS validation, certificate revocation, and certificate policy enforcement.

NetX Duo Secure TLS will perform basic X.509 path validation on the certificate before invoking the callback to assure that the certificate can be traced to a certificate in the TLS trusted certificate store, but all other validation will be handled by this callback.

The callback provides the TLS session pointer and a pointer to the remote host identity certificate (the leaf in the certificate chain). The callback should return NX_SUCCESS if all validation is successful, otherwise it should return an error code indicating the validation failure. Any value other than NX_SUCCESS will cause the TLS handshake to immediately abort.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **func_ptr** Pointer to the certificate validation callback function.

### Return Values

- **NX_SUCCESS** (0x00) Successful allocation of Function pointer.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.

### Allowed From

Threads

### Example

```C
/* Callback routine used to perform additional checks on a certificate. */
ULONG certificate_callback(NX_SECURE_TLS_SESSION *session, NX_SECURE_X509_CERT
*certificate)
{
    /* Certificate validation checking goes here. */
    return(NX_SUCCESS);
}

/* In TLS setup routine. */
{
        /* Add callback to TLS session.  */
        status =  nx_secure_tls_session_certificate_callback_set(&tls_session,
                                                            certificate_callback);

        /* If status is NX_SUCCESS the certificate callback was added.  */
}
```

### See Also

- nx_secure_tls_session_create
- nx_secure_x509_common_name_dns_check
- nx_secure_x509_crl_revocation_check

## nx_secure_tls_session_client_callback_set

Set up a callback for TLS to invoke at the beginning of a TLS Client handshake

### Prototype

```C
UINT  nx_secure_tls_ session_client_callback_set (
                  NX_SECURE_TLS_SESSION *tls_session,
                  ULONG (*func_ptr)(NX_SECURE_TLS_SESSION *tls_session,
                                    NX_SECURE_TLS_HELLO_EXTENSION
                                    *extensions, UINT num_extensions));
```

### Description

This service assigns a function pointer to a TLS session that TLS will invoke when a TLS Client handshake has received a ServerHelloDone message. The callback function allows an application to process any TLS extensions from the received ServerHello message that require input or decision making.

The callback is executed with the invoking TLS session control block and an array of NX_SECURE_TLS_HELLO_EXTENSION objects. The array of extension objects is intended to be passed into a helper function that will find and parse a specific extension. Currently, there are no specific extensions supported in NetX Duo Secure that require TLS Client input, but the callback is available for application designers to handle custom or new extensions that may become available. For an example helper function that parses TLS extensions provided in hello messages, see *nx_secure_tls_session_sni_extension_parse*.

The client callback may also be used to select the active identity certificate using *nx_secure_tls_active_certificate_set* for the TLS Client in the event that the remote server has requested a certificate and provided information to allow the TLS Client to select a specific certificate. See the reference for nx_secure_tls_active_certificate_set for more information.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **func_ptr** Pointer to the TLS Client callback function.

### Return Values

- **NX_SUCCESS** (0x00) Successful allocation of function pointer.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.

### Allowed From

Threads

### Example

```C
/* Callback routine used to process ServerHello extensions and perform other
   runtime actions at the beginning of a TLS handshake (such as selecting an
   identify certificate to provide to the server). */

ULONG tls_client_callback(NX_SECURE_TLS_SESSION *session,
                          NX_SECURE_TLS_HELLO_EXTENSION *extensions, UINT
                          num_extensions)
{

    /* Extension parsing would go here. */

    /* Set an active identity certificate – the certificate should have been added
       to the local store at application start with
       nx_secure_tls_local_certificate_add. */
    nx_secure_tls_active_certificate_set(session, &global_identity_certificate);

    return(NX_SUCCESS);
}

/* TLS setup routine. */
UINT tls_setup(NX_SECURE_TLS_SESSION *tls_session)
{
    /* Add callback to TLS session.  */
    status =  nx_secure_tls_session_client_callback_set(tls_session,
                                                        client_callback);

    /* If status is NX_SUCCESS the callback was added and will be invoked once
       a ServerHelloDone message is received. */

    return(status);
}
```

### See Also

- nx_secure_tls_session_create
- nx_secure_tls_session_server_callback_set
- nx_secure_tls_active_certificate_set

## nx_secure_tls_session_x509_client_verify_configure

Enable client X.509 verification and allocate space for client certificates

### Prototype

```C
UINT  nx_secure_tls_session_x509_client_verify_configure(
                  NX_SECURE_TLS_SESSION *session_ptr,
                  UINT certs_number, VOID *certificate_buffer,
                  ULONG buffer_size);
```

### Description

This service enables the optional X.509 Client Authentication for a TLS Server instance. It also allocates the space needed to process incoming certificate chains from the remote client host. The certificates supplied by the remote client will be verified against the TLS server instance's trusted certificates, assigned with the service *nx_secure_tls_trusted_certificate_add.*

For TLS Client mode, remote certificate allocation is required and the service *nx_secure_tls_remote_certificate_buffer_allocate* should be used instead. Enabling X.509 Client Authentication on a TLS Client instance will have no effect.

The *certificate_buffer* parameter points to space allocated to store the incoming remote certificates and the control blocks needed for those certificates. The buffer will be divided by the number of certificates (*certs_number*) with an equal portion given to each certificate. The *buffer_size* parameter indicates the size of the buffer. The space needed can be found with the following formula:

```C
buffer_size = (<expected max number of certificates in chain>) *
             (sizeof(NX_SECURE_X509_CERT) + <max cert size>)
```

Typical certificates with RSA keys of 2048 bits using SHA-256 for signatures are in the range of 1000-2000 bytes. The buffer should be large enough to at least hold that size for each certificate, but depending on the remote host certificates may be significantly smaller or larger. Note that if the buffer is too small to hold the incoming certificate, the TLS handshake will end with an error.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **certs_number** Number of certificates to allocate from the provided buffer.
- **certificate_buffer** Pointer to a buffer to hold certificates received from a remote host.
- **buffer_size** Size of the certificate buffer.

### Return Values

- **NX_SUCCESS** (0x00)Successful allocation of certificate.
- **NX_PTR_ERROR** (0x07) Invalid TLS session or buffer pointer.
- **NX_SECURE_TLS_INSUFFICIENT_CERT_SPACE** (0x12D) The supplied buffer was too small.
- **NX_INVALID_PARAMETERS** (0x4D) The buffer was too small to hold the desired number of certificates.

### Allowed From

Threads

### Example

```C
/* Buffer space to hold the incoming certificates. */
#define CERTIFICATE_NUMBER    (2)
#define CERTIFICATE_SIZE      (2000)
#define BUFFER_SIZE          (CERTIFICATE_NUMBER * \
                                (sizeof(NX_SECURE_X509_CERT) + CERTIFICATE_SIZE))
UCHAR certificate_buffer[BUFFER_SIZE];

/* Enable X.509 Client verification and allocate certificate space in our TLS
   Server session.  */
status =  nx_secure_tls_session_x509_client_verify_configure(&tls_session,
                                                    CERTIFICATE_NUMBER,
                                                    certificate_buffer,
                                                    BUFFER_SIZE);


/* If status is NX_SUCCESS the buffer was successfully allocated.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_session_create
- nx_secure_tls_remote_certificate_buffer_allocate

## nx_secure_tls_session_client_verify_disable

Disable Client Certificate Authentication for a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_session_client_verify_disable(
                              NX_SECURE_TLS_SESSION *session_ptr);
```

### Description

This service disables Client Certificate Authentication for a specific TLS session. See nx_secure_tls_session_client_verify_enable for more information.

### Parameters

- **session_ptr** Pointer to a TLS Session instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful state change.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.

### Allowed From

Threads

### Example

```C
/* Disable client certificate authentication for this TLS session.   */
status = nx_secure_tls_session_client_verify_disable(&tls_session);

/* Any new TLS Server sessions using the tls_session control block will not
request certificates from the remote TLS client.  */
```

### See Also

- nx_secure_tls_session_client_verify_enable
- nx_secure_tls_session_start
- nx_secure_tls_session_create

## nx_secure_tls_session_client_verify_enable

Enable Client Certificate Authentication for a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_session_client_verify_enable(
                                NX_SECURE_TLS_SESSION *session_ptr);
```

### Description

This service enables Client Certificate Authentication for a specific TLS session. Enabling Client Certificate Authentication for a TLS Server instance will cause the TLS Server to request a certificate from any remote TLS Client during the initial TLS handshake. The certificate received from the remote TLS Client is accompanied by a CertificateVerify message, which the TLS Server uses to verify that the Client owns the certificate (has access to the private key associated with that certificate).

If the provided certificate can be verified and traced back to a certificate in the TLS Server trusted certificate store via an X.509 certificate chain, the remote TLS Client is authenticated and the handshake proceeds. In case of any errors in processing the certificate or CertificateVerify message, the TLS handshake ends with an error.

> **Note:** *The TLS Server must have at least one certificate in its trusted store added with nx_secure_tls_trusted_certificate_add or authentication will always fail.*

### Parameters

- **session_ptr** Pointer to a TLS Session instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful state change.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.

### Allowed From

Threads

### Example

```C
/* Add a trusted certificate so the TLS Server has something to verify against. */
status = nx_secure_tls_trusted_certificate_add(&tls_session,
                                               &trusted_certificate);

/* Disable client certificate authentication for this TLS session.   */
status = nx_secure_tls_session_client_verify_enable(&tls_session);

/* Any new TLS Server sessions using the tls_session control block will now
request and verify certificates from the remote TLS client.  */
```

### See Also

- nx_secure_tls_session_client_verify_enable
- nx_secure_tls_session_start
- nx_secure_tls_session_create
- nx_secure_tls_trusted_certificate_add

## nx_secure_tls_session_create

Create a NetX Duo Secure TLS Session for secure communications

### Prototype

```C
UINT  nx_secure_tls_session_create(NX_SECURE_TLS_SESSION *session_ptr
                                   NX_SECURE_TLS_CRYPTO *cipher_table,
                                   VOID *encryption_metadata_area,
                                   ULONG encryption_metadata_size);
```

### Description

This service initializes an NX_SECURE_TLS_SESSION structure instance for use in establishing secure TLS communications over a network connection.

The method takes a NX_SECURE_TLS_CRYPTO object that is populated with the available cryptographic methods to be used for TLS. The *encryption_metadata_area* points to a buffer allocated for use by TLS for the "metadata" used by the cryptographic methods in the NX_SECURE_TLS_CRYPTO table for calculations. The size of the table can be determined by using the nx_secure_tls_metadata_size_calculate service. See the section "Cryptography in NetX Duo Secure TLS" in Chapter 3 for more details.

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **cipher_table** Pointer to TLS cryptographic methods.
- **encryption_metadata_area** Pointer to space for cryptography metadata.
- **encryption_metadata_size** Size of the metadata buffer.

### Return Values

- **NX_SUCCESS** (0x00)Successful initialization of the TLS session.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.
- **NX_INVALID_PARAMETERS** (0x4D) The metadata buffer was too small for the given methods.
- **NX_SECURE_TLS_UNSUPPORTED_CIPHER** (0x106) A required cipher method for the enabled version of TLS was not supplied in the cipher table.

### Allowed From

Threads

### Example

```C
/* Reference the platform-specific TLS cryptographic method table. */
extern const NX_SECURE_TLS_CRYPTO nx_crypto_tls_ciphers;

/* Declare a buffer for the cryptographic metadata. The size should be calculated
   using nx_secure_tls_metadata_size_calculate. */
UCHAR crypto_metadata[4500];

/* Initialize TLS session.  */
status =  nx_secure_tls_session_create(&tls_session,
                                       &nx_crypto_tls_ciphers,
                                       crypto_metadata,
                                       sizeof(crypto_metadata));


/* If status is NX_SUCCESS the TLS Session was successfully initialized.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_metadata_size_calculate
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_session_start
- nx_secure_tls_session_end
- nx_secure_tls_session_send
- nx_secure_tls_session_receive
- nx_secure_tls_session_delete
- Cryptography in NetX Duo Secure TLS in Chapter 3

## nx_secure_tls_session_delete

Delete a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_session_delete(NX_SECURE_TLS_SESSION *session_ptr);
```

### Description

This service deletes a TLS session represented by an NX_SECURE_TLS_SESSION structure instance and releases all system resources owned by that session instance.

### Parameters

- **session_ptr** Pointer to a TLS Session instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful initialization of the TLS session.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.

### Allowed From

Threads

### Example

```C
/* Delete TLS session.  */
status =  nx_secure_tls_session_delete(&tls_session);


/* If status is NX_SUCCESS the TLS Session was successfully deleted.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_session_start
- nx_secure_tls_session_end
- nx_secure_tls_session_send
- nx_secure_tls_session_receive
- nx_secure_tls_session_create

## nx_secure_tls_session_end

End an active NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_session_end(NX_SECURE_TLS_SESSION *session_ptr,
                                    ULONG wait_option);
```

### Description

This service ends a TLS session represented by an NX_SECURE_TLS_SESSION structure instance by sending the TLS CloseNotify message to the remote host. The service then waits for the remote host to respond with its own CloseNotify message.

If the remote host does not send a CloseNotify message, TLS considers this an error and a possible security breach, so checking the return value is important for a secure connection. The **wait_option** parameter can be used to control how long the service should wait for the response before returning control to the calling thread.

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **wait_option** Indicates how long the service should wait for the response from the remote host.

### Return Values

- **NX_SUCCESS** (0x00) Successful initialization of the TLS session.
- **NX_SECURE_TLS_NO_CLOSE_RESPONSE** (0x113) Did not receive a response from the remote host before timing out.
- **NX_SECURE_TLS_ALLOCATE_PACKET_FAILED** (0x111) Could not allocate a packet to send the CloseNotify message.
- **NX_SECURE_TLS_TCP_SEND_FAILED** (0x109) Could not send the CloseNotify message over the TCP socket.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.

### Allowed From

Threads

### Example

```C
/* End TLS session.  */
status =  nx_secure_tls_session_end(&tls_session, NX_WAIT_FOREVER);


/* If status is NX_SUCCESS the TLS Session was successfully ended.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_session_start
- nx_secure_tls_session_delete
- nx_secure_tls_session_send
- nx_secure_tls_session_receive
- nx_secure_tls_session_create

## nx_secure_tls_session_packet_buffer_set

Set the packet reassembly buffer for a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_session_packet_buffer_set(
                                    NX_SECURE_TLS_SESSION *session_ptr,
                                    UCHAR *buffer_ptr,
                                    ULONG buffer_size);
```

### Description

This service associates a packet reassembly buffer to a TLS session. In order to decrypt and parse incoming TLS records, the data in each record must be assembled from the underlying TCP packets. TLS records can be up to 16KB in size (though typically are much smaller) so may not fit into a single TCP packet.

If you know the incoming message size will be smaller than the TLS record limit of 16KB, the buffer size can be made smaller, but in order to handle unknown incoming data the buffer should be made as large as possible. If an incoming record is larger than the supplied buffer, the TLS session will end with an error.

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **buffer_ptr** Pointer to buffer for TLS to use for packet reassembly.
- **buffer_size** Size of the supplied buffer in bytes.

### Return Values

- **NX_SUCCESS** (0x00) Successful initialization of the TLS session.
- **NX_INVALID_PARAMETERS** (0x4D) Invalid buffer or TLS session pointer.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.

### Allowed From

Threads

### Example

```C
/* Buffer for TLS packet reassembly. */
UCHAR tls_packet_buffer[16384];
/* Assign buffer to TLS session.  */
status =  nx_secure_tls_session_packet_buffer_set(&tls_session, tls_packet_buffer,
                                                  sizeof(tls_packet_buffer));


/* If status is NX_SUCCESS the buffer was successfully added.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_session_start
- nx_secure_tls_session_delete
- nx_secure_tls_session_send
- nx_secure_tls_session_receive
- nx_secure_tls_session_create

## nx_secure_tls_session_packet_pool_set

Configure a custom packet pool to be used in a NetX Duo Secure TLS Session.

### Prototype

```C
UINT nx_secure_tls_session_packet_pool_set(NX_SECURE_TLS_SESSION *tls_session,
                                           NX_PACKET_POOL *packet_pool);
```

### Description

This service configures an optional packet pool to a TLS session. By default the packet pool in the IP instance is used to allocate packets for TLS handshake and decryption. The user can create another packet pool for a TLS session when necessary.

### Parameters

- **tls_session** Pointer to a TLS Session instance.
- **packet_pool** Pointer to a custom packet pool.

### Return Values

- **NX_SUCCESS** (0x00) Successful packet pool set.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.
- **NX_SECURE_TLS_SESSION_UNINITIALIZED** (0x101) The supplied TLS session was not initialized.

### Allowed From

Threads

### Example

```C
NX_PACKET_POOL custom_packet_pool;

status = nx_packet_pool_create(&custom_packet_pool, "TLS pool", 1536,
                               pool_memory_ptr, pool_size);

/* Set the packet pool to the TLS session. */
status = nx_secure_tls_session_packet_pool_set(&tls_session, &custom_packet_pool);

/* If status is NX_SUCCESS, the packet pool was successfully set. */
```

### See Also

- nx_secure_tls_session_packet_buffer_set
- nx_secure_tls_session_start
- nx_secure_tls_session_delete
- nx_secure_tls_session_create

## nx_secure_tls_session_protocol_version_override

Override the default TLS protocol version for a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_session_protocol_version_override(
                              NX_SECURE_TLS_SESSION *session_ptr,
                              USHORT protocol_version);
```

### Description

This service overrides the default (newest) TLS protocol version used by a particular session. This enables NetX Duo Secure TLS to use an older version of TLS for a specific TLS Session without disabling newer TLS versions at compile time. This may be useful in applications that may need to communicate with an older host that does not support the newest version of TLS.

> **Important:** *As of version 5.11SP3, NetX Duo Secure TLS supports RFC 7507 (see note below) and any override to an older version with this API will result in a fallback SCSV being sent in the ClientHello. If the server supports a newer version of TLS and the fallback SCSV is included in the ClientHello, that server will abort the TLS handshake with an "Inappropriate Fallback" alert. The SCSV is only sent when the version override is an older version of TLS than is enabled (e.g. if you override the version to TLS 1.2, no SCSV will be sent).*

Valid values for the protocol_version parameter are the following macros:
NX_SECURE_TLS_VERSION_TLS_1_0,
NX_SECURE_TLS_VERSION_TLS_1_1, and
NX_SECURE_TLS_VERSION_TLS_1_2.

The macros NX_SECURE_TLS_DISABLE_TLS_1_1 and NX_SECURE_TLS_ENABLE_TLS_1_0 can be used to control the versions of TLS that are compiled into the application. TLS version 1.2 is always enabled.

Note that if the remote host does not support the supplied version, the connection will fail – only the supplied override version will be negotiated by NetX Duo Secure TLS.

> **Important:** RFC 7507: TLS Fallback SCSV. This RFC was introduced to mitigate a security problem that was originally caused by servers that improperly handled protocol downgrade negotiation and instead rejected otherwise valid ClientHello messages. In an attempt to remain compatible with these old servers, some TLS client applications started to retry failed handshakes with an older TLS version (e.g. TLS 1.2 failed so try TLS 1.1). This workaround however introduced a new problem – an attacker could force a client to downgrade by artificially introducing a network or packet error causing the server connection to fail – even when the server supported the newer TLS version. By downgrading to an older version the attacker could exploit weaknesses in that version (SSLv3<sup>1</sup> in particular is weak to the POODLE attack). To prevent this situation, RFC 7507 introduced the "fallback SCSV," a pseudo-ciphersuite<sup>2</sup> sent in the ClientHello that notifies a TLS server when a TLS client is using a TLS version that is not the newest version it supports. This way, a server that supports a newer version can reject a ClientHello containing the fallback SCSV and prevent the downgrade attack from succeeding.

1. NetX Duo Secure does not implement SSLv3 due to the existence of known serious weaknesses such as POODLE.

2. A pseudo-ciphersuite, or SCSV (Signaling Cipher Suite Value), is a reserved ciphersuite number that is used to signal enabled TLS implementations about features that were not available in older TLS versions. The fallback SCSV and the TLS_EMPTY_RENEGOTIATION_INFO_SCSV (RFC 5746) are examples.

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **protocol_version** TLS version macro for the specific TLS version to use.

### Return Values

- **NX_SUCCESS** (0x00) Successful state change.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.
- **NX_SECURE_TLS_UNSUPPORTED_TLS_VERSION** (0x110) Known but unsupported TLS version.
- **NX_SECURE_TLS_UNKNOWN_TLS_VERSION** (0x10F) Invalid protocol version.

### Allowed From

Threads

### Example

```C
/* Set the protocol version to be used to TLSv1.1. */
status = nx_secure_tls_session_protocol_version_override(&tls_session,
                                              NX_SECURE_TLS_VERSION_TLS_1_1);

/* This TLS Session will use TLSv1.1 for the handshake and TLS session. */
status = nx_secure_tls_session_start(&tls_session, &tcp_socket, NX_WAIT_FOREVER);
```

### See Also

- nx_secure_tls_session_start
- nx_secure_tls_session_create

## nx_secure_tls_session_receive

Receive data from a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_session_receive(NX_SECURE_TLS_SESSION *session_ptr,
                                    NX_PACKET **packet_ptr,
                                    ULONG wait_option);
```

### Description

This service receives data from the specified active TLS session, handling the decryption of that data before providing it to the caller in the NX_PACKET parameter. If no data is queued in the specified session, the call suspends based on the supplied wait option.

> **Important:** *If NX_SUCCESS is returned, the application is responsible for releasing the received packet when it is no longer needed.*

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **packet_ptr** Pointer to an allocated TLS packet pointer.
- **wait_option** Indicates how long the service should wait for a packet from the remote host before returning.

### Return Values

- **NX_SUCCESS** (0x00) Successful initialization of the TLS session.
- **NX_NO_PACKET** (0x01) No data received.
- **NX_NOT_CONNECTED** (0x38) The underlying TCP socket is no longer connected.
- **NX_SECURE_TLS_HASH_MAC_VERIFY_FAILURE** (0x108) A received message failed an authentication hash check.
- **NX_SECURE_TLS_UNKNOWN_TLS_VERSION** (0x10F) A received message contained an unknown protocol version in its header.
- **NX_SECURE_TLS_ALERT_RECEIVED** (0x114) Received a TLS alert from the remote host.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.
- **NX_SECURE_TLS_SESSION_UNINITIALIZED** (0x101) The supplied TLS session was not initialized.

### Allowed From

Threads

### Example

```C
/* Receive a packet from an active TLS session previously created and started with
nx_secure_tls_session_start. Wait until a packet is received, blocking otherwise.
*/
status =  nx_secure_tls_session_receive(&tls_session, &packet_ptr,
NX_WAIT_FOREVER);


/* If status is NX_SUCCESS the received packet is pointed to by  "packet_ptr". */
```

### See Also

- nx_tcp_socket_receive
- nx_secure_x509_certificate_initialize
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_session_start
- nx_secure_tls_session_delete
- nx_secure_tls_session_send
- nx_secure_tls_session_end
- nx_secure_tls_session_create

## nx_secure_tls_session_renegotiate_callback_set

Assign a callback that will be invoked at the beginning of a session renegotiation

### Prototype

```C
UINT  nx_secure_tls_ session_renegotiate_callback_set (
                  NX_SECURE_TLS_SESSION *tls_session,
                  ULONG (*func_ptr)(struct NX_SECURE_TLS_SESSION_struct
                  *session));
```

### Description

This service assigns a callback to a TLS session that will be invoked whenever the first message of a session renegotiation handshake is received from the remote host.

The callback function is intended as a notification to the application that a renegotiation handshake is beginning – the application may choose to terminate the TLS session by returning any non-zero value from the callback which will cause TLS to end the TLS session with an error. If the application wishes to proceed with the renegotiation, the callback should return NX_SUCCESS.

> **Note:** *Due to the semantics of TLS renegotiation, the callback will be invoked for NetX Duo Secure TLS Clients whenever a HelloRequest is received from the remote server, but not when the client initiates the renegotiation. On NetX Duo Secure TLS Servers, the callback will be invoked whenever a renegotiation ClientHello is received (any ClientHello received in the context of an active TLS session). This means that the callback will be invoked whether the remote host or the local application has initiated the renegotiation because the TLS server will send a HelloRequest to which the remote client will respond.*

NetX Duo Secure TLS implements the Secure Renegotiation Indication Extension from RFC 5746 to ensure that renegotiation handshakes are not subject to man-in-the-middle attacks.

### Parameters

- **session_ptr** Pointer to TLS Session instance.
- **func_ptr** Pointer to callback function.

### Return Values

- **NX_SUCCESS** (0x00) Successful assignment of callback.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer for the callback function or TLS session.

### Allowed From

Threads

### Example

```C
/* Simple callback notifying the user that a renegotiation is starting. */
ULONG tls_renegotiation_callback(NX_SECURE_TLS_SESSION *session)
{
    printf("Renegotiation initiated by remote host\n");

    return(NX_SUCCESS);
}

/* Establish a TLS session with a remote host (TLS Client) */
status =  nx_secure_tls_session_create(&tls_session,
                                       &nx_crypto_tls_ciphers,
                                       crypto_metadata,
                                       sizeof(crypto_metadata));


/* Set callback for renegotiation notification. */
status = nx_secure_tls_session_renegotiate_callback_set(&tls_session,
                                                tls_renegotiation_callback);
```

### See Also

- nx_secure_tls_session_create
- nx_secure_tls_session_start
- nx_secure_tls_session_receive
- nx_secure_tls_session_send
- nx_secure_tls_session_renegotiate

## nx_secure_tls_session_renegotiate

Initiate a session renegotiation handshake with the remote host

### Prototype

```C
UINT  nx_secure_tls_ session_renegotiate (
                  NX_SECURE_TLS_SESSION *tls_session,
                  UINT wait_option);
```

### Description

This service initiates a session *renegotiation* handshake with a connected remote TLS host. A renegotiation consists of a second TLS handshake within the context of a previously-established TLS session. Each of the new handshake messages is encrypted using the TLS session until new session keys are generated and ChangeCipherSpec messages are exchanged, at which time the new keys are used to encrypt all messages.

A renegotiation can be initiated at any time once a TLS session is established. If a renegotiation is attempted during a TLS handshake or before a TLS session is established no action will be taken.

> **Note:** *An entire TLS handshake will be performed when this service is invoked so the time to completion and returned status will vary depending on the current TLS settings and session parameters.*

NetX Duo Secure TLS implements the Secure Renegotiation Indication Extension from RFC 5746 to ensure that renegotiation handshakes are not subject to man-in-the-middle attacks.

### Parameters

- **session_ptr** Pointer to TLS Session instance.
- **wait_option** Indicates how long the service should wait for a packet from the remote host before returning. This is passed to all NetX Duo services within TLS.

### Return Values

- **NX_SUCCESS** (0x00) Successful renegotiation.
- **NX_NO_PACKET** (0x01) No data received.
- **NX_NOT_CONNECTED** (0x38) The underlying TCP socket is no longer connected.
- **NX_SECURE_TLS_HASH_MAC_VERIFY_FAILURE** (0x108) A received message failed an authentication hash check.
- **NX_SECURE_TLS_UNKNOWN_TLS_VERSION** (0x10F) A received message contained an unknown protocol version in its header.
- **NX_SECURE_TLS_ALERT_RECEIVED** (0x114) Received a TLS alert from the remote host.
- **NX_SECURE_TLS_RENEGOTIATION_SESSION_INACTIVE** (0x134) The local or remote TLS session is inactive, making renegotiation impossible.
- **NX_SECURE_TLS_RENEGOTIATION_FAILURE** (0x13A) The remote host did not provide either the SCSV or Secure Renegotiation Extension and thus renegotiation cannot be performed.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.
- **NX_SECURE_TLS_SESSION_UNINITIALIZED** (0x101) The supplied TLS session was not initialized.
- **NX_SECURE_TLS_ALLOCATE_PACKET_FAILED** (0x111) Underlying packet allocation failed.

### Allowed From

Threads

### Example

```C
/* Establish a TLS session with a remote host (TLS Client) */
status =  nx_secure_tls_session_create(&tls_session,
                                       &nx_crypto_tls_ciphers,
                                       crypto_metadata,
                                       sizeof(crypto_metadata));


/* Setup a client socket connection.  */
status = nx_tcp_client_socket_connect(&tcp_socket, server_ipv4_address,
REMOTE_SERVER_PORT, NX_WAIT_FOREVER);

/* Start the TLS session. */
status = nx_secure_tls_session_start(&tls_session, &tcp_socket, NX_WAIT_FOREVER);

/* Send some data in the first TLS session. (Check "status" for errors…)*/
status = nx_secure_tls_packet_allocate(&tls_session, &pool_0, &send_packet,
                                       NX_WAIT_FOREVER);
status = nx_packet_data_append(send_packet, "Hello there!\r\n", 14, &pool_0,
                               NX_WAIT_FOREVER);
status = nx_secure_tls_session_send(&tls_session, send_packet,
                                    NX_IP_PERIODIC_RATE);

/* Now renegotiate the session. */
status  = nx_secure_tls_session_renegotiate(&tls_session, NX_WAIT_FOREVER);

/* If status == NX_SUCCESS, new TLS session keys have been generated. */

/* Send some data in the new TLS session. This will be encrypted using the new
   keys. */
status = nx_secure_tls_packet_allocate(&tls_session, &pool_0, &send_packet,
                                       NX_WAIT_FOREVER);
status = nx_packet_data_append(send_packet, "Another message…\r\n", 18, &pool_0,
                               NX_WAIT_FOREVER);
status = nx_secure_tls_session_send(&tls_session, send_packet,
                                    NX_IP_PERIODIC_RATE);
```

### See Also

- nx_secure_tls_session_create
- nx_secure_tls_session_start
- nx_secure_tls_session_receive
- nx_secure_tls_session_send
- nx_secure_tls_session_renegotiation_callback_set

## nx_secure_tls_session_reset

Clear out and reset a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_session_reset (NX_SECURE_TLS_SESSION *session_ptr);
```

### Description

This service clears out a TLS session and resets the state as if the session had just been created so an existing TLS session object can be re-used for a new session.

### Parameters

- **session_ptr** Pointer to a TLS Session instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful initialization of the TLS session.
- **NX_INVALID_PARAMETERS** (0x4D) Invalid TLS session pointer.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.

### Allowed From

Threads

### Example

```C
/* Reset a TLS session.  */
status =  nx_secure_tls_session_reset(&tls_session);


/* If status is NX_SUCCESS the session was successfully reset and may be reused.*/
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_session_start
- nx_secure_tls_session_delete
- nx_secure_tls_session_send
- nx_secure_tls_session_receive
- nx_secure_tls_session_create

## nx_secure_tls_session_send

Send data through a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_session_send(NX_SECURE_TLS_SESSION *session_ptr,
                                    NX_PACKET *packet_ptr,
                                    ULONG wait_option);
```

### Description

This service sends data in the supplied NX_PACKET, using the specified active TLS session, and handling the encryption of that data before sending it to the remote host. If the receiver's last advertised window size is less than this request, the service optionally suspends based on the wait options specified.

> **Important:** *Unless an error is returned, the application should not release the packet after this call. Doing so will cause unpredictable results because the network driver will release the packet after transmission.*

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **packet_ptr** Pointer to a TLS packet containing data to be sent.
- **wait_option** Defines how the service behaves if the request is greater than the window size of the receiver.

### Return Values

- **NX_SUCCESS** (0x00) Successful initialization of the TLS session.
- **NX_NO_PACKET** (0x01) No data received.
- **NX_NOT_CONNECTED** (0x38) The underlying TCP socket is no longer connected.
- **NX_SECURE_TLS_TCP_SEND_FAILED** (0x109) The underlying TCP socket send failed.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.
- **NX_SECURE_TLS_SESSION_UNINITIALIZED** (0x101) The supplied TLS session was not initialized.

### Allowed From

Threads

### Example

```C
/* Send a packet using an active TLS session previously created and started with
nx_secure_tls_session_start. Wait until a packet is sent, blocking otherwise.   */
status =  nx_secure_tls_session_send(&tls_session, &packet_ptr, NX_WAIT_FOREVER);


/* If status is NX_SUCCESS the packet has been sent. */
```

### See Also

- nx_tcp_socket_receive
- nx_secure_x509_certificate_initialize
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_packet_allocate
- nx_secure_tls_session_start
- nx_secure_tls_session_delete
- nx_secure_tls_session_receive
- nx_secure_tls_session_end
- nx_secure_tls_session_create

## nx_secure_tls_session_server_callback_set

Set up a callback for TLS to invoke at the beginning of a TLS Server handshake

### Prototype

```C
UINT  nx_secure_tls_ session_server_callback_set (
                 NX_SECURE_TLS_SESSION *tls_session,
                 ULONG (*func_ptr)(NX_SECURE_TLS_SESSION *tls_session,
                                   NX_SECURE_TLS_HELLO_EXTENSION
                                   *extensions, UINT num_extensions));
```

### Description

This service assigns a function pointer to a TLS session that TLS will invoke when a TLS Server handshake has received a ClientHello message. The callback function allows an application to process any TLS extensions from the received ClientHello message that require input or decision making.

The callback is executed with the invoking TLS session control block and an array of NX_SECURE_TLS_HELLO_EXTENSION objects. The array of extension objects is intended to be passed into a helper function that will find and parse a specific extension. For an example helper function that parses TLS extensions provided in hello messages, see *nx_secure_tls_session_sni_extension_parse*.

The server callback may also be used to select the active identity certificate using *nx_secure_tls_active_certificate_set* for the TLS Server. This will most often occur in response to a Server Name Indication (SNI) extension which allows a TLS Client to indicate which server it is attempting to contact. See the references for *nx_secure_tls_session_sni_extension_parse* and *nx_secure_tls_active_certificate_set* for more information.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **func_ptr** Pointer to the TLS Server callback function.

### Return Values

- **NX_SUCCESS** (0x00) Successful allocation of Function pointer.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.

### Allowed From

Threads

### Example

```C
/* Application-supplied function to map server DNS name to a specific certificate
   ID. The certificate ID is supplied by the application when the certificate is
   added with nx_secure_tls_server_certificate_add. */
UINT application_find_id_by_dns_name(NX_SECURE_X509_DNS_NAME *dns_name)
{
    if(strncmp(dns_name->nx_secure_x509_dns_name, "server_name",
               dns_name->nx_secure_x509_dns_name_length) == 0)
    {
        /* DNS name matches one we know, return it's ID. */
        return(0x12);
    }

    /* Unknown DNS name, return 0 to indicate no matching ID found. */
    return(0);
}

/* Callback routine used to process ClientHello extensions and perform other
   runtime actions at the beginning of a TLS handshake (such as selecting an
   identify certificate to provide to the client). */
ULONG tls_server_callback(NX_SECURE_TLS_SESSION *session,
                          NX_SECURE_TLS_HELLO_EXTENSION *extensions, UINT
                          num_extensions)
{
UINT status;
NX_SECURE_X509_DNS_NAME dns_name;
UINT cert_id;
NX_SECURE_X509_CERT *certificate;


    /* Find and parse a Server Name Indication (SNI) extension. */
    status = nx_secure_tls_session_sni_extension_parse(session, extensions,
                                                       num_extensions, &dns_name);

    if(status != NX_SUCCESS && status != NX_SECURE_TLS_EXTENSION_NOT_FOUND)
    {
        /* Parsed an invalid extension, return the error. */
        return(status);
    }

    if(status == NX_SECURE_TLS_EXTENSION_NOT_FOUND)
    {
            /* SNI extension not found, just return success to use the default
               certificate. */
            return(NX_SUCCESS);
    }

    /* Find the application-supplied numeric identifier for the specified DNS
       name provided by the remote client. */
    cert_id = application_find_id_by_dns_name(&dns_name);

    /* If cert_id is 0, just use the default identity certificate added with
       nx_secure_tls_local_certificate_add. */
    if(cert_id != 0)
    {
        /* Application found a matching name, find the certificate in our
           store. */
        status = nx_secure_tls_server_certificate_find(tls_session, &certificate,
                                                       cert_id);

        if(status != NX_SUCCESS)
        {
            /* Didn't find a valid certificate with the supplied ID. Return an
               error so TLS can shut down the handshake. */
            return(NX_SECURE_TLS_CERTIFICATE_NOT_FOUND);
        }

        /* Set the active identity certificate – the certificate should have been
           added to the local store at application start with
           nx_secure_tls_local_certificate_add. */
        nx_secure_tls_active_certificate_set(session, certificate);
    }

    return(NX_SUCCESS);
}

/* TLS setup routine. */
UINT tls_setup(NX_SECURE_TLS_SESSION *tls_session)
{
        /* Add callback to TLS session.  */
        status =  nx_secure_tls_session_server_callback_set(tls_session,
                                                            server_callback);

        /* If status is NX_SUCCESS the callback was added and will be invoked
           immediately after a ClientHello message is received. */

        return(status);
}
```

### See Also

- nx_secure_tls_session_create
- nx_secure_tls_session_client_callback_set
- nx_secure_tls_active_certificate_set
- nx_secure_tls_session_sni_extension_parse
- nx_secure_tls_server_certificate_add
- nx_secure_tls_server_certificate_find

## nx_secure_tls_session_sni_extension_parse

Parse a Server Name Indication (SNI) extension received from a TLS Client

### Prototype

```C
UINT  nx_secure_tls_session_sni_extension_parse(
                                    NX_SECURE_TLS_SESSION *session_ptr,
                                    NX_SECURE_TLS_HELLO_EXTENSION
                                    *extensions,
                                    UINT num_extensions,
                                    NX_SECURE_X509_DNS_NAME *dns_name);
```

### Description

This service is intended to be called from within a TLS Server session callback, added to a TLS session using nx_secure_tls_session_server_callback_set. The callback is invoked following the reception of a ClientHello message from a remote TLS client and is supplied an array of available extensions (and the number of extensions in the array). That array and its length can be passed directly to this routine to determine if there is an SNI extension present – if not, NX_SECURE_TLS_EXTENSION_NOT_FOUND is returned indicating simply that the client opted not to provide the SNI extension (this is not an error).

If the SNI extension is found, the X.509 DNS name supplied by the TLS client is returned in the dns_name structure. Currently, the SNI extension only supplies a single DNS name entry, which may be used by the TLS server to determine which identity certificate to send to the remote client.**

The NX_SECURE_X509_DNS_NAME structure simply contains the DNS name as a UCHAR string in the field *nx_secure_x509_dns_name* and the length of the name string in *nx_secure_x509_dns_name_length*. The macro NX_SECURE_X509_DNS_NAME_MAX controls the size of the nx_secure_x509_dns_name buffer.

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **extensions** Pointer to an array of TLS Hello extensions (from session callback).
- **num_extensions** Number of extensions in array (from session callback).
- **dns_name** Return DNS name supplied in the SNI extension.

### Return Values

- **NX_SUCCESS** (0x00) Successful parsing of the extension.
- **NX_PTR_ERROR** (0x07) Invalid extensions array or TLS session pointer.
- **NX_SECURE_TLS_EXTENSION_NOT_FOUND** (0x136) SNI extension not found.
- **NX_SECURE_TLS_SNI_EXTENSION_INVALID** (0x137) SNI extension format was invalid.

### Allowed From

Threads

### Example

### See Also

- nx_secure_tls_session_server_callback_set
- nx_secure_tls_session_client_callback_set
- nx_secure_tls_server_callback_set
- nx_secure_tls_active_certificate_set

## nx_secure_tls_session_sni_extension_set

Set a Server Name Indication (SNI) extension DNS name to send to a remote Server

### Prototype

```C
UINT  nx_secure_tls_session_sni_extension_set(
                                    NX_SECURE_TLS_SESSION *session_ptr,
                                    NX_SECURE_X509_DNS_NAME *dns_name);
```

### Description

This service allows a TLS Client application to provide a preferred server DNS name to a remote TLS server using the Server Name Indication (SNI) TLS extension. The SNI extension allows the server to select the proper identity certificate and parameters based on the client's indicated server preference. The SNI extension currently only supports a single DNS name to be sent, hence the singular name parameter. The dns_name parameter must be initialized with *nx_secure_x509_dns_name_initialize* and will contain the client's preferred server. To unset the extension name, simply call this service with a "dns_name" parameter value of NX_NULL.

The NX_SECURE_X509_DNS_NAME structure simply contains the DNS name as a UCHAR string in the field  *nx_secure_x509_dns_name* and the length of the name string in *nx_secure_x509_dns_name_length*. The macro  NX_SECURE_X509_DNS_NAME_MAX controls the size of the nx_secure_x509_dns_name buffer.

> **Note:** *This routine must be called before nx_secure_tls_session_start is invoked or the ClientHello will not contain the SNI extension.*

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **dns_name** DNS name supplied by the application.

### Return Values

- **NX_SUCCESS** (0x00) Successful addition of DNS server name.
- **NX_PTR_ERROR** (0x07) Invalid DNS name or TLS session pointer.

### Allowed From

Threads

### Example

```C
#define TLS_SNI_SERVER_NAME "www.example.com"

NX_SECURE_X509_DNS_NAME dns_name;
NX_SECURE_TLS_SESSION client_tls_session;

/* Application where TLS session is started. */
void main()
{
    /* Create a TLS session for our socket. Ciphers and metadata defined
       elsewhere. See nx_secure_tls_session_create reference for more
       information. */
    status =  nx_secure_tls_session_create(&client_tls_session,
                                           &nx_crypto_tls_ciphers,
                                           server_crypto_metadata,
                                           sizeof(server_crypto_metadata));


    /* Initialize the DNS server name we want to send in the SNI extension. */
    nx_secure_x509_dns_name_initialize(&dns_name, TLS_SNI_SERVER_NAME,
                                    strlen(TLS_SNI_SERVER_NAME));

    /* The SNI server name needs to be set prior to starting the TLS session. */
    nx_secure_tls_session_sni_extension_set(&tls_session, &dns_name);

   /* Start TLS session as normal. */
}
```

### See Also

- nx_secure_tls_session_start
- nx_secure_x509_dns_name_initialize

## nx_secure_tls_session_start

Start a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_session_start(NX_SECURE_TLS_SESSION *session_ptr,
                                  NX_TCP_SOCKET *tcp_socket_ptr,
                                  ULONG wait_option);
```

### Description

This service starts a TLS session using the supplied TLS session control block and a connected TCP socket. The TCP connection must already be complete following a successful call to either nx_tcp_client_socket_connect or nx_tcp_server_socket_accept.

This service will determine the type of TLS session (Client or Server) from the TCP socket.

The wait option defines how the service behaves while the TLS handshake is in progress.

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **tcp_socket_ptr** Pointer to a connected TCP socket.
- **wait_option** Defines how the service behaves while the TLS handshake is in progress.

### Return Values

- **NX_SUCCESS** (0x00) Successful initialization of the TLS session.
- **NX_NOT_CONNECTED** (0x38) The underlying TCP socket is no longer connected.
- **NX_SECURE_TLS_UNRECOGNIZED_MESSAGE_TYPE** (0x102) A received TLS message type is incorrect.
- **NX_SECURE_TLS_UNSUPPORTED_CIPHER** (0x106) A cipher provided by the remote host is not supported.
- **NX_SECURE_TLS_HANDSHAKE_FAILURE** (0x107) Message processing during the TLS handshake has failed.
- **NX_SECURE_TLS_HASH_MAC_VERIFY_FAILURE** (0x108) An incoming message failed a hash MAC check.
- **NX_SECURE_TLS_TCP_SEND_FAILED** (0x109) An underlying TCP socket send failed.
- **NX_SECURE_TLS_INCORRECT_MESSAGE_LENGTH** (0x10A) An incoming message had an invalid length field.
- **NX_SECURE_TLS_BAD_CIPHERSPEC** (0x10B) An incoming ChangeCipherSpec message was incorrect.
- **NX_SECURE_TLS_INVALID_SERVER_CERT** (0x10C) An incoming TLS certificate is unusable for identifying the remote TLS server.
- **NX_SECURE_TLS_UNSUPPORTED_PUBLIC_CIPHER** (0x10D) The public-key cipher provided by the remote host is unsupported.
- **NX_SECURE_TLS_NO_SUPPORTED_CIPHERS** (0x10E) The remote host has indicated no ciphersuites that are supported by the NetX Duo Secure TLS stack.
- **NX_SECURE_TLS_UNKNOWN_TLS_VERSION** (0x10F) A received TLS message had an unknown TLS version in its header.
- **NX_SECURE_TLS_UNSUPPORTED_TLS_VERSION** (0x110) A received TLS message had a known but unsupported TLS version in its header.
- **NX_SECURE_TLS_ALLOCATE_PACKET_FAILED** (0x111) An internal TLS packet allocation failed.
- **NX_SECURE_TLS_INVALID_CERTIFICATE** (0x112) The remote host provided an invalid certificate.
- **NX_SECURE_TLS_ALERT_RECEIVED** (0x114) The remote host sent an alert indicating an error and ending the TLS session.
- **NX_SECURE_TLS_MISSING_CRYPTO_ROUTINE** (0x13B) An entry in the ciphersuite table had a NULL function pointer.
- **NX_SECURE_TLS_INAPPROPRIATE_FALLBACK** (0x146) A remote TLS ClientHello included the fallback SCSV and attempted a version fallback.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.

### Allowed From

Threads

### Example

```C
NX_TCP_SOCKET tcp_socket;
NX_SECURE_TLS_SESSION tls_session;
NX_SECURE_X509_CERT certificate;

/* Initialize the TLS session structure. */
nx_secure_tls_session_create(&tls_session,
                              &nx_crypto_tls_ciphers,
                              crypto_metadata,
                              sizeof(crypto_metadata));

/* Setup the TLS packet reassembly buffer. */
status = nx_secure_tls_session_packet_buffer_set(&tls_session, tls_packet_buffer,
                                                 sizeof(tls_packet_buffer));

/* Initialize a certificate for the TLS server. */
status =  nx_secure_x509_certificate_initialize(&certificate, certificate_data, 500,
NX_NULL, 0, private_key, 64);

/* If status is NX_SUCCESS, certificate is initialized. */

/* Add the certificate a local certificate to identify this TLS server. */
status = nx_secure_tls_add_local_certificate(&tls_session, &certificate);

/* If status is NX_SUCCESS, certificate was added successfully. */

/* Create and start a TCP socket as a server. */
/* NOTE: This assumes an IP instance called "ip_0" has already been created. */

/* Create a TCP server socket on the previously created IP instance, with normal
   delivery, IP fragmentation enabled, default time to live, a 8192-byte receive
   window, no urgent callback routine, and the "client_disconnect" routine to
   handle disconnection initiated from the other end of the connection.  */
status =  nx_tcp_socket_create(&ip_0, &tcp_socket, "TLS Server Socket", NX_IP_NORMAL,
                               NX_FRAGMENT_OKAY, NX_IP_TIME_TO_LIVE, 8192, NX_NULL,
                               NX_NULL);

/* If status is NX_SUCCESS, the TCP socket was created successfully. */

/* Start up a TCP server socket by listening on port 443 with a listen queue of
   size 5. */
status =  nx_tcp_server_socket_listen(&ip_0, 443, &tcp_socket, 5, NX_NULL);

/* Application main loop. */
while(1)
{
        /* Accept incoming requests on the TCP socket. */
        status =  nx_tcp_server_socket_accept(&tcp_socket, NX_WAIT_FOREVER);

        /* At this point, the TCP socket should be established (but not the TLS
        session yet). */

        /* Start the TLS session on our active TCP socket. */
        status = nx_secure_tls_session_start(&tls_session, &tcp_socket,
                                             NX_WAIT_FOREVER);

        /* At this point, if status is NX_SUCCESS, the TLS session has been
           established.  Application may now send/receive data through this
           channel. */

        /* Send and receive data using the TLS session. */
        /* Allocate TLS packets using nx_secure_tls_packet_allocate, write data with
           nx_packet_data_append, read data with nx_packet_data_extract_offset. */

        /* Send TLS data. */
        nx_secure_tls_session_send(&tls_session, &send_packet, NX_WAIT_FOREVER);

        nx_secure_tls_session_receive(&tls_session, &receive_packet_ptr,
        NX_WAIT_FOREVER);


        /* Once all application data is sent/received, end the TLS session. */
        status = nx_secure_tls_session_end(&tls_session);

        /* If status is NX_SUCCESS, the TLS session has been closed properly. */

        /* Now disconnect the TCP server socket from the client.  */
        nx_tcp_socket_disconnect(&tcp_socket, 200);

        /* Unaccept the TCP server socket.  Note that unaccept is called even if
           disconnect or accept fails.  */
        nx_tcp_server_socket_unaccept(&tcp_socket);

        /* Setup server socket for listening with this socket again.  */
        nx_tcp_server_socket_relisten(&ip_0, 443, &tcp_socket);
}

/* When the server application is shut down, clean up the TLS session. */
nx_secure_tls_session_delete(&tls_session);

/* Finally, clean up the TCP socket as well. */
nx_tcp_socket_delete(&tcp_socket);
```

### See Also

- nx_tcp_socket_receive
- nx_secure_x509_certificate_initialize
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_session_send
- nx_secure_tls_session_delete
- nx_secure_tls_session_receive
- nx_secure_tls_packet_allocate
- nx_secure_tls_session_end
- nx_secure_tls_session_create
- nx_tcp_socket_accept
- nx_tcp_socket_listen
- nx_tcp_socket_disconnect
- nx_tcp_socket_unaccept
- nx_tcp_socket_relisten
- nx_tcp_socket_delete
- nx_packet_allocate
- nx_packet_data_append
- nx_packet_data_extract_offset

## nx_secure_tls_session_time_function_set

Assign a timestamp function to a NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_time_function_set(
                              NX_SECURE_TLS_SESSION *session_ptr,
                              ULONG (*time_func_ptr)(void));
```

### Description

This function sets up a function pointer that TLS will invoke when it needs to get the current time, which is used in various TLS handshake messages and for verification of certificates.

The function is expected to return the current GMT in UNIX 32-bit format (seconds since the midnight starting Jan 1, 1970, UTC, ignoring leap seconds), as per the ClientHello requirements in the TLS RFC 5246.

If no timestamp function is assigned, a value of 0 for the timestamp in the TLS handshake will be used and certificate expiration checking will not work.

### Parameters

- **session_ptr** Pointer to a TLS Session instance.
- **time_func_ptr** Pointer to a function that returns the current time (GMT) in UNIX 32-bit format.

### Return Values

- **NX_SUCCESS** (0x00) Successful initialization of the TLS session.
- **NX_INVALID_PARAMETERS** (0x4D) Invalid TLS session pointer.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.

### Allowed From

Threads

### Example

```C
/* This function returns a 32-bit UNIX-style representation of the current GMT
   time. */
ULONG get_gmt_time(void)
{
ULONG time_value;

   /* Platform-specific time calculation goes here… */

   return(time_value);
}

/* Reset a TLS session.  */
status =  nx_secure_tls_timestamp_function_set(&tls_session, get_gmt_time);


/* If status is NX_SUCCESS the function was successfully added.*/
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_session_start
- nx_secure_tls_session_delete
- nx_secure_tls_session_sendnx_secure_tls_session_receive
- nx_secure_tls_session_create

## nx_secure_tls_trusted_certificate_add

Add trusted certificate to NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_trusted_certificate_add(NX_SECURE_TLS_SESSION
                            *session_ptr, NX_SECURE_X509_CERT *certificate_ptr);
```

### Description

This service adds an initialized NX_SECURE_X509_CERT structure instance to a TLS session. This certificate is used by the TLS stack to verify certificates supplied by the remote host during the TLS handshake.

Trusted certificates are required for TLS Client mode.

Trusted certificates are only required for TLS Server mode if client certificate authentication is enabled.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **certificate_ptr** Pointer to an initialized TLS Certificate instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful addition of certificate.
- **NX_INVALID_PARAMETERS** (0x4D) Tried to add an invalid certificate.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.

### Allowed From

Threads

### Example

```C
/* Initialize certificate structure. */
status = nx_secure_x509_certificate_initialize(&certificate, certificate_data,
                                               certificate_buffer,
                                               sizeof(certificate_buffer), 500,
                                               private_key, 64);

/* Add certificate to TLS session.  */
status =  nx_secure_tls_trusted_certificate_add(&tls_session, &certificate);


/* If status is NX_SUCCESS the certificate was successfully added.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_session_create
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_trusted_certificate_remove

## nx_secure_tls_trusted_certificate_remove

Remove trusted certificate from NetX Duo Secure TLS Session

### Prototype

```C
UINT  nx_secure_tls_trusted_certificate_remove(
                                    NX_SECURE_TLS_SESSION *session_ptr,
                                    UCHAR *common_name,
                                    UINT common_name_length);
```

### Description

This service removes a trusted certificate from a TLS session, keyed on the Common Name field in the certificate.

### Parameters

- **session_ptr** Pointer to a previously created TLS Session instance.
- **common_name** The Common Name value of the certificate to be removed.
- **common_name_length** The length of the Common Name string.

### Return Values

- **NX_SUCCESS** (0x00) Successful addition of certificate.
- **NX_PTR_ERROR** (0x07) Invalid TLS session pointer.
- **NX_SECURE_TLS_CERTIFICATE_NOT_FOUND** (0x119) Certificate was not found.

### Allowed From

Threads

### Example

```C
/* Remove certificate from TLS session.  */
status =  nx_secure_tls_trusted_certificate_remove(&tls_session,
                                                "www.example.com", 15);


/* If status is NX_SUCCESS the certificate was successfully removed.  */
```

### See Also

- nx_secure_x509_certificate_initialize
- nx_secure_tls_session_create
- nx_secure_tls_remote_certificate_allocate
- nx_secure_tls_trusted_certificate_add

## nx_secure_x509_certificate_initialize

Initialize X.509 Certificate for NetX Duo Secure TLS

### Prototype

```C
UINT  nx_secure_x509_certificate_initialize(
                  NX_SECURE_X509_CERT *certificate_ptr,
                  const UCHAR *certificate_data,
                  USHORT certificate_data_length,
                  UCHAR *raw_data_buffer,
                  USHORT buffer_size,
                  const UCHAR *private_key_data,
                  USHORT private_key_data_length,
                  UINT private_key_type);
```

### Description

This service initializes an NX_SECURE_X509_CERT structure from a binary-encoded X.509 digital certificate for use in a TLS session.

The certificate data **must** be a valid X.509 digital certificate in DER-encoded binary format. The data can some from any source (e.g. file system, compiled constant buffer, etc.) as long as a UCHAR pointer to that data is provided.

The *raw_data_buffer* parameter and its size are optional parameters that specify a dedicated buffer into which the certificate data is copied before parsing. If raw_data_buffer is passed as NX_NULL then the NX_SECURE_X509_CERT structure will point directly into the certificate_data array (buffer_size is ignored in this case). If raw_data_buffer is passed as NX_NULL, ***do not*** modify the data pointed to by the certificate_data pointer or certificate processing will likely fail!

The private key parameter is for local identity certificates – the private key is used by servers to decrypt the incoming key data from a client (encrypted using the server's public key) and by clients to verify their identity to a server when the server requests a client certificate. Adding a private key with this API will automatically mark the associated certificate as being an identity certificate for a TLS application. When initializing certificates for other purposes (e.g. the trusted store), the *private_key_data* parameter should be passed as NULL, the *private_key_data_length* as 0, and the *private_key_type* should be passed as NX_SECURE_X509_KEY_TYPE_NONE.

The *private_key_type* parameter indicates the formatting of the private key. For example, if the private key is a DER-encoded PKCS#1-format RSA private key, the private_key_type should be passed as NX_SECURE_X509_KEY_TYPE_RSA_PKCS1_DER, a type known to NetX Duo Secure which will be parsed immediately and saved for later use.

The private_key_type also supports user-defined key types for platforms and applications that have specific key formats or other needs. For example, a hardware-based encryption engine may use a specific format not understood by the NetX Duo Secure software, or a private key may be encrypted or represented by a cryptographic token as might be the case with a Trusted Platform Module (TPM) or PKCS#11 cryptographic hardware. When a user-defined key type is used, the key data is passed verbatim to the appropriate cryptographic routine - for example, an RSA private key would be passed, without any parsing or processing, directly to the RSA cryptographic routine provided to TLS in the ciphersuite table. The user-defined key type is also passed to the cryptographic routine (in the case of RSA, this is the "op" parameter).

The range of user-defined keys covers the top half of a 32-bit unsigned integer, from 0x0001 0000-0xFFFF FFFF. Values less than 0x0001 0000 are reserved for NetX Duo Secure use.

User-defined key types are an advanced feature that requires custom cryptographic routines to handle the raw private key data. Please contact your Express Logic representative if you have need of this feature.

### Parameters

- **certificate_ptr** Pointer to an uninitialized X.509 Certificate instance.
- **certificate_data** Pointer to DER-encoded X.509 binary data.
- **raw_data_buffer** Pointer to optional dedicated certificate data buffer.
- **buffer_size** Size of optional dedicated certificate data buffer.
- **certificate_data_length** Length of certificate binary data in
bytes.
- **private_key_data** Pointer to optional private key data.
- **private_key_data_length** Length of private key data.
- **private_key_type** Key type identifier.

### Return Values

- **NX_SUCCESS** (0x00) Successful addition of certificate.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.
- **NX_SECURE_TLS_INVALID_CERTIFICATE** (0x112) Certificate data did not contain a DER-encoded X.509 certificate.
- **NX_SECURE_TLS_UNSUPPORTED_PUBLIC_CIPHER** (0x10D) Certificate did not have a public-key cipher that is supported by NetX Duo Secure.
- **NX_SECURE_X509_INVALID_CERTIFICATE_SEQUENCE** (0x186) Private key or certificate did not contain a valid ASN.1 sequence.
- **NX_SECURE_PKCS1_INVALID_PRIVATE_KEY** (0x18A) The provided private key was not a valid PKCS#1 RSA key.
- **NX_SECURE_X509_INVALID_PRIVATE_KEY_TYPE** (0x19D) The private key type provided was not user-defined and did not match any known type.

### Allowed From

Threads

### Example

```C
/* Initialize certificate structure. The certificate structure will point directly
   into the certificate_data array since we are passing the raw_data_buffer as
   NX_NULL. This certificate has a private key so it will be used to identify this
   device. */
status =  nx_secure_x509_certificate_initialize(&certificate, certificate_data,
500, NX_NULL, 0, private_key, 64, NX_SECURE_X509_KEY_TYPE_RSA_PKCS1_DER);


/* If status is NX_SUCCESS the certificate was successfully initialized.  */
```

### See Also

- nx_secure_local_certificate_add
- nx_secure_tls_session_create
- nx_secure_tls_remote_certificate_allocate
- Importing X.509 Certificates into NetX Duo Secure in Chapter 3.

## nx_secure_x509_common_name_dns_check

Check DNS name against X.509 Certificate

### Prototype

```C
UINT  nx_secure_x509_common_name_dns_check(
                        NX_SECURE_X509_CERT *certificate,
                        const UCHAR *dns_tld, UINT dns_tld_length);
```

### Description

This service checks a certificate's Common Name against a Top- Domain name (TLD) provided by the caller for the purposes of DNS validation of a remote host. This utility function is intended to be called from within a certificate validation callback routine provided by the application. The TLD name should be the top part of the URL used to access the remote host (the "."-separated string before the first slash). If the Common Name contains a wildcard (such as *.example.com), the wildcard will match any with the same suffix. Note that only the first wildcard ("*") encountered (reading right-to-left) will be considered for wildcard matching – for example, abc.*.example.com will match *any* name ending in ".example.com".

If the Common Name does not match the provided string, the "subjectAltName" extension is parsed (if it exists in the certificate) and any DNSName entries are also compared. If none of those entries match, an error is returned.

It is important to understand the format of the common name (and subjectAltName entries) in expected certificates. For example, some certificates may use a raw IP address or a wild card. The DNS TLD string must be formatted such that it will match the expected values in received certificates.

### Parameters

- **certificate_ptr** Pointer to an X.509 Certificate instance.
- **dns_tld** Top-Level Domain name to compare against.
- **dns_tld_length** Length of TLD string.

### Return Values

- **NX_SUCCESS** (0x00) Successful comparison with Common Name or subjectAltName.
- **NX_SECURE_X509_CERTIFICATE_DNS_MISMATCH** (0x195) No matching name found.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.

### Allowed From

Threads

### Example

```C
/* Callback routine used to perform additional checks on a certificate. */
ULONG certificate_callback(NX_SECURE_TLS_SESSION *session, NX_SECURE_X509_CERT
*certificate)
{
ULONG status;
UCHAR *tld = "www.example.com";

    /* Check our DNS TLD against the certificate provided by the
       remote TLS host. */
    status = nx_secure_x509_common_name_dns_check(certificate, tld, strlen(tld));

        if(status != NX_SUCCESS)
        {
            /* TLD did not match any names in the certificate. */
            return(status);
        }

        /* DNS validation and any other checks were successful. */
        return(NX_SUCCESS);
}

…

/* Add callback to TLS session in TLS setup routine.  */
status =  nx_secure_tls_session_certificate_callback_set(&tls_session,
                                                         certificate_callback);
```

### See Also

- nx_secure_tls_session_create
- nx_secure_tls_session_certificate_callback_set
- nx_secure_x509_crl_revocation_check

## nx_secure_x509_crl_revocation_check

Check X.509 Certificate against a supplied Certificate Revocation List (CRL)

### Prototype

```C
UINT  nx_secure_x509_crl_revocation_check(const UCHAR *crl_data,
                              UINT crl_length,
                              NX_SECURE_X509_CERTIFICATE_STORE *store,
                              NX_SECURE_X509_CERT *certificate);
```

### Description

This service takes a DER-encoded Certificate Revocation List and searches for a specific certificate in that list. The issuer of the CRL is validated against a supplied certificate store, the CRL issuer is validated to be the same as the one for the certificate being checked, and the serial number of the certificate in question is used to search the revoked certificates list. If the issuers match, the signature checks out, and the certificate is **not** present in the list, the call is successful. All other cases cause an error to be returned.

### Parameters

- **crl_data** Pointer to a DER-encoded CRL.
- **crl_length** Length in bytes of CRL data.
- **store** Pointer to an X.509 Certificate store.
- **certificate_ptr** Pointer to an X.509 Certificate instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful validation that the certificate was not revoked.
- **NX_SECURE_TLS_CERTIFICATE_NOT_FOUND** (0x119) CRL issuer certificate not found.
- **NX_SECURE_TLS_ISSUER_CERTIFICATE_NOT_FOUND** 0x11B) Certificate issuer certificate not found.
- **NX_SECURE_X509_ASN1_LENGTH_TOO_LONG** (0x182) The CRL ASN.1 contained an invalid length field.
- **NX_SECURE_X509_UNEXPECTED_ASN1_TAG(0x189)** The CRL contained invalid ASN.1.
- **NX_SECURE_X509_CHAIN_VERIFY_FAILURE** (0x18C) A certificate chain verification failed.
- **NX_SECURE_X509_CRL_ISSUER_MISMATCH** (0x197) CRL and certificate issuers did not match.
- **NX_SECURE_X509_CRL_SIGNATURE_CHECK_FAILED** 0x198) The CRL signature was invalid.
- **NX_SECURE_X509_CRL_CERTIFICATE_REVOKED** (0x199) The certificate being checked was found in the CRL and is therefore revoked.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.

### Allowed From

Threads

### Example

```C
/* CRL obtained for the expected certificate issuer through some means (downloaded
   from server manually, obtained from CRL endpoint, etc…) */
const UCHAR *crl_data;
UINT crl_length = 300;

/* Callback routine used to perform additional checks on a certificate. */
ULONG certificate_callback(NX_SECURE_TLS_SESSION *session, NX_SECURE_X509_CERT
*certificate)
{
ULONG status;
NX_SECURE_X509_CERTIFICATE_STORE *store;

    /* Obtain a certificate store to check against. In the certificate callback,
       it usually makes the most sense to use the store associated with the TLS
       session. */
    store = &session -> nx_secure_tls_credentials.nx_secure_tls_certificate_store;

    /* Check our certificate against the CRL and TLS certificate store. */
    status = nx_secure_x509_crl_revocation_check(crl, crl_length, store,
                                                 certificate);

        if(status != NX_SUCCESS)
        {
            if(status == NX_SECURE_X509_CRL_CERTIFICATE_REVOKED)
            {
                /* Certificate was revoked. */
               return(status);
            }
            else
            {
               /* CRL was invalid or some other issue. In this case the certificate
                  may still be valid since the CRL itself was a problem. At this
                  point it is up to the application to decide whether to continue
                  with the TLS handshake. For this example, assume certificate is
                  valid (faulty CRL is a possible Denial-of-Service attack).*/
               status = NX_SUCCESS;
        }

    /* Other certificate checking can go here. */

    /* Return status of certificate checks. */
    return(status);
}

…

/* Add callback to TLS session in TLS setup routine.  */
status =  nx_secure_tls_session_certificate_callback_set(&tls_session,
                                                         certificate_callback);
```

### See Also

- nx_secure_tls_session_create
- nx_secure_tls_session_certificate_callback_set
- nx_secure_x509_common_name_dns_check

## nx_secure_x509_dns_name_initialize

Initialize an X.509 DNS name structure

### Prototype

```C
UINT  nx_secure_x509_dns_name_initialize(
                        NX_SECURE_X509_DNS_NAME *dns_name,
                        const UCHAR *name_string, UINT length);
```

### Description

This service initializes an X.509 DNS name for use with certain API services requiring a specific name format. For example, the *nx_secure_tls_sni_extension_parse* service expects an NX_SECURE_X509_DNS_NAME object in order to match the name provided by a remote host in the Server Name Indication extension during the TLS handshake. A DNS name is simply a character string with a length – the maximum allowed length of a DNS name (and the size of the internal buffer in NX_SECURE_X509_DNS_NAME) is controlled by the macro NX_SECURE_X509_DNS_NAME_MAX (default 100 bytes).

### Parameters

- **dns_name** DNS name structure to initialize.
- **name_string** DNS name string data.
- **length** Length of name string.

### Return Values

- **NX_SUCCESS** (0x00) Successful initialization.
- **NX_SECURE_X509_NAME_STRING_TOO_LONG** (0x19E) The given name string exceeded NX_SECURE_X509_DNS_NAME_MAX.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.

### Allowed From

Threads

### Example

```C
NX_SECURE_X509_DNS_NAME dns_name;

/* Initialize DNS name. */
status = nx_secure_x509_dns_name_initialize(&dns_name, "www.example.com",
                                            strlen("www.example.com");

/* Use initialized DNS name to send the Server Name Indication extension to the
   remote TLS server. */
status = nx_secure_tls_session_sni_extension_set(&tls_session, &dns_name);
```

### See Also

- nx_secure_tls_session_create
- nx_secure_tls_session_sni_extension_parse
- nx_secure_tls_session_sni_extension_set

## nx_secure_x509_extended_key_usage_extension_parse

Find and parse an X.509 extended key usage extension in an X.509 certificate

### Prototype

```C
UINT  nx_secure_x509_extended_key_usage_extension_parse(
                        NX_SECURE_X509_CERT *certificate,
                        UINT key_usage);
```

### Description

This service is intended to be called from within a certificate verification callback (see *nx_secure_tls_session_certificate_callback_set)*. It will search for a specific extended key usage OID within an X.509 certificate and return whether the OID is present. The key_usage parameter is an integer mapping of the OIDs which is used internally by NetX Duo Secure X.509 and TLS to avoid passing the variable-length OID strings as parameters.

The relevant OIDs for the extended key usage extension are given in the table below. A typical TLS Client implementation wishing to check extended key usage in a received TLS server certificate would check for the existence of the OID NX_SECURE_TLS_X509_TYPE_PKIX_KP_SERVER_AUTH – if the extension is present but that OID is not, then the certificate would be considered invalid for identifying the host as a TLS server and the certificate verification callback should return an error. If the extension itself is missing, then it is up to the application whether or not to proceed with the TLS handshake.

In the certificate verification callback, the error return code NX_SECURE_X509_KEY_USAGE_ERROR is reserved for application use. If there is an error in checking key usage, this value may be returned from the callback to indicate the reason for failure.

| NetX Duo Secure Identifier                                | OID Value         | Description                                      |
| --------------------------------------------------------- | --------------------- | ---------------------------------------------------- |
| NX_SECURE_TLS_X509_TYPE_PKIX_KP_SERVER_AUTH   | 1.3.6.1.5.5.7.3.1 | Certificate can be used to identify a TLS server |
| NX_SECURE_TLS_X509_TYPE_PKIX_KP_CLIENT_AUTH   | 1.3.6.1.5.5.7.3.2 | Certificate can be used to identify a TLS client |
| NX_SECURE_TLS_X509_TYPE_PKIX_KP_CODE_SIGNING  | 1.3.6.1.5.5.7.3.3 | Certificate can be used to sign code             |
| NX_SECURE_TLS_X509_TYPE_PKIX_KP_EMAIL_PROTECT | 1.3.6.1.5.5.7.3.4 | Certificate can be used to sign emails           |
| NX_SECURE_TLS_X509_TYPE_PKIX_KP_TIME_STAMPING | 1.3.6.1.5.5.7.3.8 | Certificate can be used to sign timestamps       |
| NX_SECURE_TLS_X509_TYPE_PKIX_KP_OCSP_SIGNING  | 1.3.6.1.5.5.7.3.9 | Certificate can be used to sign OCSP responses   |

OIDs and mappings for X.509 Extended Key Usage Extension

### Parameters

- **certificate** Pointer to certificate being verified.
- **key_usage** OID integer mapping from table above.

### Return Values

- **NX_SUCCESS** (0x00) Specified key usage OID found.
- **NX_SECURE_X509_MULTIBYTE_TAG_UNSUPPORTED** (0x181) ASN.1 multi-byte tag encountered (unsupported certificate).
- **NX_SECURE_X509_ASN1_LENGTH_TOO_LONG** (0x182) Invalid ASN.1 field encountered (invalid certificate).
- **NX_SECURE_X509_INVALID_TAG_CLASS** (0x190) Invalid ASN.1 tag class encountered (invalid certificate).
- **NX_SECURE_X509_INVALID_EXTENSION_SEQUENCE** (0x192) Invalid extension encountered (invalid certificate).
- **NX_SECURE_X509_EXTENSION_NOT_FOUND** (0x19B) The Extended Key Usage extension was not found in the provided certificate.
- **NX_PTR_ERROR** (0x07) Invalid certificate pointer.

### Allowed From

Threads

### Example

```C
ULONG certificate_verification_callback(NX_SECURE_TLS_SESSION *session, NX_SECURE_X509_CERT* certificate)
{
UINT status;

    /* Extended key usage - look for specific OIDs. */
    status = nx_secure_x509_extended_key_usage_extension_parse(certificate,
                        NX_SECURE_TLS_X509_TYPE_PKIX_KP_SERVER_AUTH);

    if(status != NX_SUCCESS)
    {
        if(NX_SECURE_X509_EXT_KEY_USAGE_NOT_FOUND)
        {
            printf("Extended key usage extension not found or specified key usage OID not
                    provided in extension.\n");
            /* The certificate was valid but the specified OID was not found. The
               application can decide whether to continue or abort the TLS handshake. */
            return(NX_SECURE_X509_KEY_USAGE_ERROR);
        }
        else
        {
            /* The extension or certificate was invalid. */
            return(status);
        }
    }

    /* The specified OID was found, return success! */
    return(NX_SUCCESS);
}
```

### See Also

- nx_secure_tls_session_certificate_callback_set
- nx_secure_x509_key_usage_extension_parse
- nx_secure_x509_crl_revocation_check
- nx_secure_x509_common_name_dns_check
- nx_secure_x509_extension_find

## nx_secure_x509_extension_find

Find and return an X.509 extension in an X.509 certificate

### Prototype

```C
UINT  nx_secure_x509_extension_find(
                        NX_SECURE_X509_CERT *certificate,
                        NX_SECURE_X509_EXTENSION *extension,
                        USHORT extension_id);
```

### Description

This service is intended to be called from within a certificate verification callback (see *nx_secure_tls_session_certificate_callback_set)* and is an advanced X.509 service.

The function will search for a specific extension within an X.509 certificate based on an OID and return whether the OID is present, along with a structure containing references to the relevant raw extension data. The extension_id parameter is an integer mapping of the OIDs which is used internally by NetX Duo Secure X.509 and TLS to avoid passing the variable-length OID strings as parameters.

The helper functions provided for specific extensions (such as *nx_secure_x509_key_usage_extension_parse*) call nx_secure_x509_extension_find internally to obtain the extension data.

The relevant OIDs for known X.509 extensions are given in the table below.

The NX_SECURE_X509_EXTENSION structure contains pointers into the X.509 certificate that allow helper functions such as *nx_secure_x509_key_usage_extension_parse* to quickly decode the raw extension DER-encoded ASN.1 data.

For information on specific extensions, see RFC 5280 (X.509 specification) or the reference for the appropriate helper functions if available.

The current version of NetX Duo Secure X.509 has limited support for X.509 extensions. More helper functions will be added in the future.

> **Important:** *This service is an advanced feature for users familiar with X.509 extensions and DER-encoded ASN.1. It is provided to enable those users to access extensions for which NetX Duo Secure X.509 does not currently provide helper functions. For those extensions without helper functions, you will have to parse the raw DER-encoded ASN.1 yourself.*

| NetX Duo Secure Identifier                              | OID Value | Description                                                                    | Helper function? |
| ------------------------------------------------------- | ------------- | ---------------------------------------------------------------------------------- | -------------------- |
| NX_SECURE_TLS_X509_TYPE_DIRECTORY_ATTRIBUTES  | 2.5.29.9  | Directory Attributes – basic information attributes about certificate subject  | No               |
| NX_SECURE_TLS_X509_TYPE_SUBJECT_KEY_ID       | 2.5.29.14 | Used to identify a specific public key                                         | No               |
| NX_SECURE_TLS_X509_TYPE_KEY_USAGE             | 2.5.29.15 | Provides information on valid uses for the certificate public key              | Yes              |
| NX_SECURE_TLS_X509_TYPE_SUBJECT_ALT_NAME     | 2.5.29.17 | Provides alternative DNS names to identify the certificate                     | Yes<sup>3</sup>        |
| NX_SECURE_TLS_X509_TYPE_ISSUER_ALT_NAME      | 2.5.29.18 | Provides alternative DNS names to identify the certificate's issuer            | No               |
| NX_SECURE_TLS_X509_TYPE_BASIC_CONSTRAINTS     | 2.5.29.19 | Provides basic certificate usage constraint information                        | No               |
| NX_SECURE_TLS_X509_TYPE_NAME_CONSTRAINTS      | 2.5.29.30 | Used to constrain certificate names to specific domains                        | No               |
| NX_SECURE_TLS_X509_TYPE_CRL_DISTRIBUTION      | 2.5.29.31 | Provides URIs for CRL distribution                                             | No               |
| NX_SECURE_TLS_X509_TYPE_CERTIFICATE_POLICIES  | 2.5.29.32 | List of certificate policies for large PKI systems                             | No               |
| NX_SECURE_TLS_X509_TYPE_CERT_POLICY_MAPPINGS | 2.5.29.33 | List of CA certificate policies                                                | No               |
| NX_SECURE_TLS_X509_TYPE_AUTHORITY_KEY_ID     | 2.5.29.35 | Used to identify a specific public key associated with a certificate signature | No               |
| NX_SECURE_TLS_X509_TYPE_POLICY_CONSTRAINTS    | 2.5.29.36 | CA policy constraints                                                          | No               |
| NX_SECURE_TLS_X509_TYPE_EXTENDED_KEY_USAGE   | 2.5.29.37 | Additional OID-based key usage information                                     | Yes              |
| NX_SECURE_TLS_X509_TYPE_FRESHEST_CRL          | 2.5.29.46 | Provides information for obtaining delta CRLs                                  | No               |
| NX_SECURE_TLS_X509_TYPE_INHIBIT_ANYPOLICY     | 2.5.29.54 | CA certificate field indicating that AnyPolicy cannot be used                  | No               |

OIDs and mappings for X.509 Extensions

3. The SubjectAltName extension is parsed as part of the DNS name check in the service nx_secure_x509_common_name_dns_check.

### Parameters

- **certificate** Pointer to certificate being verified.
- **extension** Return structure containing extension data pointer and length.
- **extension_id** OID integer mapping from table above.

### Return Values

- **NX_SUCCESS** (0x00) Specified extension OID found and data returned.
- **NX_SECURE_X509_MULTIBYTE_TAG_UNSUPPORTED** (0x181) ASN.1 multi-byte tag encountered (unsupported certificate).
- **NX_SECURE_X509_ASN1_LENGTH_TOO_LONG** (0x182) Invalid ASN.1 field encountered (invalid certificate).
- **NX_SECURE_X509_INVALID_TAG_CLASS** (0x190) Invalid ASN.1 tag class encountered (invalid certificate).
- **NX_SECURE_X509_INVALID_EXTENSION_SEQUENCE** (0x192) Invalid extension encountered
- **NX_SECURE_X509_EXTENSION_NOT_FOUND** (0x19B) The given extension OID was not found in the provided certificate.
- **NX_PTR_ERROR** (0x07) Invalid certificate or extension pointer.

### Allowed From

Threads

### Example

```C
/* Function to parse a Basic Constraints X.509 extension. */
UINT _basic_constraints_extension_parse(NX_SECURE_X509_CERT *certificate)
{
const UCHAR             *current_buffer;
ULONG                    length;
UINT                     status;
NX_SECURE_X509_EXTENSION extension_data;

    /* Find the Basic Constraints extension in the certificate. */
    status = _nx_secure_x509_extension_find(certificate, &extension_data,
                              NX_SECURE_TLS_X509_TYPE_BASIC_CONSTRAINTS);

    /* See if extension present - it might be OK if not present! */
    if (status != NX_SUCCESS)
    {
        return(status);
    }

    /* The extension_data structure now points to the raw extension ASN.1
      (DER-encoded). */
    current_buffer = extension_data.nx_secure_x509_extension_data;
    length = extension_data.nx_secure_x509_extension_data_length;

   /* Extension ASN.1 parsing… */

   return(NX_SUCCESS);
}
```

### See Also

- nx_secure_tls_session_certificate_callback_set
- nx_secure_x509_key_usage_extension_parse
- nx_secure_x509_crl_revocation_check
- nx_secure_x509_common_name_dns_check
- nx_secure_x509_extended_key_usage_extension_parse

## nx_secure_x509_key_usage_extension_parse

Find and parse an X.509 Key Usage extension in an X.509 certificate

### Prototype

```C
UINT  nx_secure_x509_key_usage_extension_parse(
                        NX_SECURE_X509_CERT *certificate,
                        USHORT *bitfield);
```

### Description

This service is intended to be called from within a certificate verification callback (see *nx_secure_tls_session_certificate_callback_set)*. It will search for the Key Usage extension and if found, will return the Key Usage bitfield in the "bitfield" parameter.

The bits, as defined by the X.509 specification (RFC 5280) are given in the table below. A bitwise AND with the appropriate bitmask (and checking for non-zero) will give the value of each bit.

Note that the DER-encoding of the bitfield eliminates extra zeroes so the actual position of the bits in the raw certificate data will likely be different from their positions in the decoded bitfield. The supplied bitmasks are only intended to be used on the decoded bitfield returned by *nx_secure_x509_key_usage_extension_parse* and not with the raw DER-encoded certificate data.

In the certificate verification callback, the error return code NX_SECURE_X509_KEY_USAGE_ERROR is reserved for application use. If there is an error in checking key usage, this value may be returned from the callback to indicate the reason for failure.

| NetX Duo Secure Identifier                            | Bit position | Description                                                                                                                                                  |
| ----------------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NX_SECURE_X509_KEY_USAGE_DIGITAL_SIGNATURE  | 0            | Certificate can be used for digital signatures                                                                                                               |
| NX_SECURE_X509_KEY_USAGE_NON_REPUDIATION    | 1            | Certificate can be used to verify digital signatures other than those for certificates and CRLs                                                              |
| NX_SECURE_X509_KEY_USAGE_KEY_ENCIPHERMENT   | 2            | Certificate can be used to encrypt symmetric keys (key transport)                                                                                            |
| NX_SECURE_X509_KEY_USAGE_DATA_ENCIPHERMENT  | 3            | Certificate can be used to directly encrypt raw user data (uncommon)                                                                                         |
| NX_SECURE_X509_KEY_USAGE_KEY_AGREEMENT      | 4            | Certificate can be used for key agreement (as with Diffie-Hellman)                                                                                           |
| NX_SECURE_X509_KEY_USAGE_KEY_CERT_SIGN     | 5            | Certificate can be used to sign and verify other certificates (the certificate is a CA or ICA certificate).                                                  |
| NX_SECURE_X509_KEY_USAGE_CRL_SIGN           | 6            | Certificate public key is used to verify signatures on CRLs                                                                                                  |
| NX_SECURE_X509_KEY_USAGE_ENCIPHER_ONLY      | 7            | Used with Key Agreement bit (bit 4) – when set, certificate key can only be used to encrypt during key agreement. Undefined if Key Agreement bit is not set. |
| NX_SECURE_X509_KEY_USAGE_DECIPHER_ONLY      | 8            | Used with Key Agreement bit (bit 4) – when set, certificate key can only be used to decrypt during key agreement. Undefined if Key Agreement bit is not set. |

Bitmasks and values for X.509 Key Usage Extension

### Parameters

- **certificate** Pointer to certificate being verified.
- **bitfield** Return the entire bitfield from the extension.

### Return Values

- **NX_SUCCESS** (0x00) Key usage extension found and bitfield returned.
- **NX_SECURE_X509_MULTIBYTE_TAG_UNSUPPORTED** (0x181) ASN.1 multi-byte tag encountered (unsupported certificate).
- **NX_SECURE_X509_ASN1_LENGTH_TOO_LONG** (0x182) Invalid ASN.1 field encountered (invalid certificate).
- **NX_SECURE_X509_INVALID_TAG_CLASS** (0x190) Invalid ASN.1 tag class encountered (invalid certificate).
- **NX_SECURE_X509_INVALID_EXTENSION_SEQUENCE** (0x192) Invalid extension encountered (invalid certificate).
- **NX_SECURE_X509_EXTENSION_NOT_FOUND** (0x19B)The Key Usage extension was not found in the provided certificate.
- **NX_PTR_ERROR** (0x07) Invalid certificate or bitfield pointer.

### Allowed From

Threads

### Example

```C
ULONG certificate_verification_callback(NX_SECURE_TLS_SESSION *session,
  NX_SECURE_X509_CERT* certificate)
{
UINT status;
USHORT key_usage_bitfield;

    /* Key usage – extract key usage bitfield. */
    status = nx_secure_x509_key_usage_extension_parse(certificate, &key_usage_bitfield);

   /* Check certificate for a few specific key usage bits. */
   if((key_usage_bitfield & NX_SECURE_X509_KEY_USAGE_DIGITAL_SIGNATURE) == 0 ||
      (key_usage_bitfield & NX_SECURE_X509_KEY_USAGE_NON_REPUDIATION)   == 0 ||
      (key_usage_bitfield & NX_SECURE_X509_KEY_USAGE_KEY_ENCIPHERMENT)  == 0)
    {
        printf("Expected key usage bitfield bits not set!\n");
        return(NX_SECURE_X509_KEY_USAGE_ERROR);
    }

    /* The specified bits were set, return success! */
    return(NX_SUCCESS);
}
```

### See Also

- nx_secure_tls_session_certificate_callback_set
- nx_secure_x509_extended_key_usage_extension_parse
- nx_secure_x509_crl_revocation_check
- nx_secure_x509_common_name_dns_check
- nx_secure_x509_extension_find
