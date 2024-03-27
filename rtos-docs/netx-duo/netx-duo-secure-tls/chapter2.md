---
title: Chapter 2 - Installation and use of NetX Duo Secure
description: This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo Secure component.
---

# Chapter 2 - Installation and use of NetX Duo Secure

This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo Secure component.

## Product Version Number

User may verify the product version number by finding the following symbols in nx_secure_tls.h:

***NETX_SECURE_MAJOR_VERSION***

***NETX_SECURE_MINOR_VERSION***

Service Pack releases has the following symbol defined to indicate the service pack number:

***NETX_SECURE_SERVICE_PACK_VERSION***

## Product Distribution

NetX Duo Secure is available at [https://github.com/eclipse-threadx/netxduo](https://github.com/eclipse-threadx/netxduo). The package includes source files, include files, and a PDF file that contains this document, as follows:

- **nx_secure_tls_api.h** Public API header file for NetX Duo Secure TLS
- **nx_secure_tls_user.h** User defines header file for NetX Duo Secure TLS
- **nx_secure_tls_port.h** Platform-specific definitions for NetX Duo Secure
- **nx_secure_tls.h** Header file for NetX Duo Secure TLS
- **nx_secure_tls&#42;.c/h** C/H Source files for NetX Duo Secure TLS
- **nx_crypto&#42;.c/h** C/H Source files for NetX Duo Secure Cryptography
- **nx_secure_x509&#42;.c/h** C/H Source files for X.509 digital certificates.
- **demo_netx_secure_tls.c** C Source file for NetX Duo Secure TLS Demo
- **NetX_Secure_User_Guide.pdf** PDF description of NetX Duo Secure product

> **Note:** The nx_crypto* files are provided for different hardware platforms in a subdirectory of the NetX Duo Secure parent directory.

## NetX Duo Secure Installation

In order to use NetX Duo Secure, the entire distribution mentioned previously should be copied to the same directory level where NetX Duo is installed. For example, if NetX Duo is installed in the directory "*\threadx\arm7\NetXDuo*" then the *nx_secure**.* directories should be copied into "*\threadx\arm7\NetXDuoSecure*".

## Using NetX Duo Secure

Using NetX Duo Secure TLS is straightforward. Basically, the application code must include *nx_secure_tls_api.h* after it includes *tx_api.h* and *nx_api.h*, in order to use ThreadX and NetX Duo. Once *nx_secure_tls_api.h* is included, the application code is then able to make the NetX Duo Secure function calls specified later in this guide. The application must also import the *nx_secure**.* files into a NetXSecure library, and the platform-specific *nx_crypto**.* files into a NetXCrypto library.

## Small Example System (TLS Client)

An example of how easy it is to use NetX Duo Secure is described below. This demonstrates a simple TLS Client, using software crypto modules (not hardware specific) for encryption. It is designed to work with the OpenSSL reverse-echo server (openssl s_server -rev).

Note that in order to run this example, you will need an X.509 CA certificate to authenticate your target server's certificate. For the
OpenSSL example, a simple 2-level PKI (root CA certificate-> >server
certificate) will suffice. You will need to fill in the "trusted_ca_data" array with a DER-encoded binary version of your CA certificate and update the "trusted_ca_length" variable to reflect your certificate's actual length.

You will also need the network driver for your hardware (replace "nx_driver_xx" parameter in nx_ip_create call).

```C
#include "tx_api.h"
#include "nx_api.h"
#include "nx_secure_tls_api.h"
#include "nx_secure_x509.h"

/* Define the size of our application stack. */
#define     DEMO_STACK_SIZE         	4096

/* Define the remote server IP address using NetX IP_ADDRESS macro. */
#define     REMOTE_SERVER_IP_ADDRESS  	IP_ADDRESS(192, 168, 1, 1)

/* Define the IP address for this device. */
#define     DEVICE_IP_ADDRESS         	IP_ADDRESS(192, 168, 1, 2)

/* Define the remote server port. 443 is the HTTPS default. */
#define     REMOTE_SERVER_PORT       	443

/* Define the ThreadX and NetX object control blocks...  */

NX_PACKET_POOL          pool_0;
NX_IP                   ip_0;
NX_TCP_SOCKET tcp_socket;
NX_SECURE_TLS_SESSION tls_session;
NX_SECURE_X509_CERT certificate;

/* Define an HTTP request to be sent to the HTTPS web server not defined here but
  represented by the ellipsis. */
UCHAR http_request[] = { "GET /example.html HTTP/1.1"  };

/* Define the IP thread's stack area.  */
ULONG ip_thread_stack[3 * 1024 / sizeof(ULONG)];

/* Define packet pool for the demonstration.  */
#define NX_PACKET_POOL_SIZE ((1536 + sizeof(NX_PACKET)) * 32)

ULONG packet_pool_area[NX_PACKET_POOL_SIZE/sizeof(ULONG) + 64 / sizeof(ULONG)];

/* Define the ARP cache area.  */
ULONG arp_space_area[512 / sizeof(ULONG)];

/* Define the TLS Client thread.  */
ULONG             tls_client_thread_stack[6 * 1024 / sizeof(ULONG)];
TX_THREAD         tls_client_thread;
void              client_thread_entry(ULONG thread_input);

/* Define the TLS packet reassembly buffer. */
UCHAR tls_packet_buffer[18000];

/* Define the metadata area for TLS cryptography. The actual size needed can be
   Ascertained by calling nx_secure_tls_metadata_size_calculate.
*/
UCHAR tls_crypto_metadata[18000];

/* Pointer to the TLS ciphersuite table that is included in the platform-specific
   cryptography subdirectory. The table maps the cryptographic routines for the
   platform to function pointers usable by the TLS library.
*/
extern const NX_SECURE_TLS_CRYPTO nx_crypto_tls_ciphers_ecc;
extern const USHORT nx_crypto_ecc_supported_groups[];
extern const NX_CRYPTO_METHOD *nx_crypto_ecc_curves[];
extern const UINT nx_crypto_ecc_supported_groups_size;

/* Binary data for the TLS Client X.509 trusted root CA certificate, ASN.1 DER-
   encoded. A trusted certificate must be provided for TLS Client applications
   (unless X.509 authentication is disabled) or TLS will treat all certificates as
   untrusted and the handshake will fail.
*/

/* DER-encoded binary certificate, not defined here but represented by the ellipsis,
   for the sake of brevity. */
const UCHAR trusted_ca_data[] = { … };
const UINT trusted_ca_length = 0x574;

/* Define the application – initialize drivers and TCP/IP setup.
   NOTE: the variable "status" should be checked after every API call. Most error
         checking has been omitted for clarity. */
void    tx_application_define(void *first_unused_memory)
{
UINT  status;

   /* Initialize the NetX system.  */
   nx_system_initialize();

   /* Create a packet pool. Check status for errors. */
   status =  nx_packet_pool_create(&pool_0, "NetX Main Packet Pool", 1536,
                                   (ULONG*)(((int)packet_pool_area + 64) & ~63) ,
                                   NX_PACKET_POOL_SIZE);

   /* Create an IP instance for the specific target. Check status for errors. This
      call is not completely defined. Please see other demo files for proper usage
      of the nx_ip_create call. */
   status = nx_ip_create(&ip_0, "NetX IP Instance 0",
                         DEVICE_IP_ADDRESS ,
                         0xFFFFFF00UL,
                         &pool_0, nx_driver_xx,
                         (UCHAR*)ip_thread_stack,
                         sizeof(ip_thread_stack),
                         1);

   /* Enable ARP and supply ARP cache memory for IP Instance 0. Check status for
 	   errors. */
   status =  nx_arp_enable(&ip_0, (void *)arp_space_area, sizeof(arp_space_area));

   /* Enable TCP traffic. Check status for errors. */
   status =  nx_tcp_enable(&ip_0);

   status =  nx_ip_fragment_enable(&ip_0);

   /* Initialize the NetX Duo Secure TLS system.  */
   nx_secure_tls_initialize();

	/* Create the TLS client thread to start handling incoming requests. */
   tx_thread_create(&tls_client_thread, "TLS Client thread", client_thread_entry, 0,
            	     tls_client_thread_stack, sizeof(tls_client_thread_stack),
            	     16, 16, 4, TX_AUTO_START);
   return;
}

/* Thread to handle the TLS Client instance. */
void client_thread_entry(ULONG thread_input)
{
UINT       status;
ULONG       actual_status;
NX_PACKET *send_packet;
NX_PACKET *receive_packet;
UCHAR receive_buffer[100];
ULONG bytes;
ULONG server_ipv4_address;

	/* We are not using the thread input parameter so suppress compiler warning. */
    NX_PARAMETER_NOT_USED(thread_input);

   /* Ensure the IP instance has been initialized.  */
   status =  nx_ip_status_check(&ip_0, NX_IP_INITIALIZE_DONE, &actual_status,
                                 NX_IP_PERIODIC_RATE);

   /* Create a TCP socket to use for our TLS session.  */
   status =  nx_tcp_socket_create(&ip_0, &tcp_socket, "TLS Client Socket",
                                  NX_IP_NORMAL, NX_FRAGMENT_OKAY,
                                  NX_IP_TIME_TO_LIVE, 8192, NX_NULL, NX_NULL);

   /* Create a TLS session for our socket. This sets up the TLS session object for
          later use */
   status =  nx_secure_tls_session_create(&tls_session,
                                          &nx_crypto_tls_ciphers_ecc,
                                          tls_crypto_metadata,
                                          sizeof(tls_crypto_metadata));

   /* Initialize ECC parameters for this session. */
   status = nx_secure_tls_ecc_initialize(&tls_session,
                                             nx_crypto_ecc_supported_groups,
                                             nx_crypto_ecc_supported_groups_size,
                                             nx_crypto_ecc_curves);

   /* Set the packet reassembly buffer for this TLS session. */
   status = nx_secure_tls_session_packet_buffer_set(&tls_session, tls_packet_buffer,
                                                    sizeof(tls_packet_buffer));

   /* Initialize an X.509 certificate with our CA root certificate data. */
   nx_secure_x509_certificate_initialize(&certificate, trusted_ca_data,
                                         trusted_ca_length, NX_NULL, 0, NX_NULL, 0,
                                         NX_SECURE_X509_KEY_TYPE_NONE);

   /* Add the initialized certificate as a trusted root certificate. */
   nx_secure_tls_trusted_certificate_add(&tls_session, &certificate);

   /* Setup this thread to open a connection on the TCP socket to a remote server.
      The IP address can be used directly or it can be obtained via DNS or other
      means.*/
   server_ipv4_address = REMOTE_SERVER_IP_ADDRESS;
   status = nx_tcp_client_socket_connect(&tcp_socket, server_ipv4_address,
                                         REMOTE_SERVER_PORT, NX_WAIT_FOREVER);

   /* Start the TLS Session using the connected TCP socket. This function will
      ascertain from the TCP socket state that this is a TLS Client session. */
   status = nx_secure_tls_session_start(&tls_session, &tcp_socket,
                                         NX_WAIT_FOREVER);

	/* Allocate a TLS packet to send an HTTP request over TLS (HTTPS). */
    status = nx_secure_tls_packet_allocate(&tls_session, &pool_0, &send_packet,
                                          NX_WAIT_FOREVER);

	/* Populate the packet with our HTTP request. */
    nx_packet_data_append(send_packet, http_request, sizeof(http_request), &pool_0,
                          NX_WAIT_FOREVER);


   /* Send the HTTP request over the TLS Session, turning it into HTTPS. */
   status = nx_secure_tls_session_send(&tls_session, send_packet, NX_WAIT_FOREVER);

   /* If the send fails, you must release the packet.  */
   if (status != NX_SUCCESS)
   {
         /* Release the packet since the packet was not sent.  */
         nx_packet_release(send_packet);
   }

   /* Receive the HTTP response and any data from the server. */
   status = nx_secure_tls_session_receive(&tls_session, &receive_packet,
   NX_WAIT_FOREVER);
   if (status == NX_SUCCESS)
   {
       /* Extract the data we received from the remote server. */
       status = nx_packet_data_extract_offset(receive_packet, 0, receive_buffer,
                                             100,  &bytes);
	    /* Display the response data. */
       receive_buffer[bytes] = 0;
       printf("Received data: %s\n", receive_buffer);

	    /* Release the packet when done with it. */
       nx_packet_release(receive_packet);
   }

   /* End the TLS session now that we have received our HTTPS/HTML response. */
   status = nx_secure_tls_session_end(&tls_session, NX_WAIT_FOREVER);

   /* Check for errors to make sure the session ended cleanly. */

   /* Disconnect the TCP socket. */
   status =  nx_tcp_socket_disconnect(&tcp_socket, NX_WAIT_FOREVER);

}
```

## Small Example System (TLS Web Server)

An example of how easy it is to use NetX Duo Secure is described below and demonstrates a simple TLS Web Server (HTTPS).

Note that in order to run this example, you will need an X.509 certificate to identify your server to TLS clients. For most web browsers a simple self-signed certificate should be sufficient. Your browser will complain about not being able to authenticate the server and in some cases may be unable to establish a TLS/HTTPS connection to your server. You will need to fill in the "certificate_data" array with a DER-encoded binary version of your server certificate and update the "certificate_length" variable to reflect your certificate's actual length. You also need to fill in the "private_key" array with a DER-encoded version of your certificate's private key, formatted using PKCS#1 for RSA key and RFC 5915 for ECC keys. Fill in the "private_key_length" variable with the actual length of your key data.

> **Important:** Some browsers (particularly some versions of the Chrome browser) may reject self-signed certificates. In this case you can create a 2-level PKI with a root CA certificate that is used to sign your server certificate. In this situation, the root CA certificate is installed as a trusted root certificate in your browser. !!! IMPORTANT – remove your root CA certificate from your browser when done and do not use it for any production applications !!!

You will also need the network driver for your hardware (replace "nx_driver_xx" parameter in nx_ip_create call).

```C
#include "tx_api.h"
#include "nx_api.h"
#include "nx_secure_tls_api.h"
#include "nx_secure_x509.h"

#define     DEMO_STACK_SIZE         4096

/* Define the IP address for this device. */
#define     DEVICE_IP_ADDRESS             IP_ADDRESS(192, 168, 1, 2)

/* Define the ThreadX and NetX object control blocks...  */

NX_PACKET_POOL          pool_0;
NX_IP                   ip_0;
NX_TCP_SOCKET tcp_socket;
NX_SECURE_TLS_SESSION tls_session;
NX_SECURE_X509_CERT certificate;

/* Define the IP thread's stack area.  */
ULONG ip_thread_stack[3 * 1024 / sizeof(ULONG)];

/* Define packet pool for the demonstration.  */
#define NX_PACKET_POOL_SIZE ((1536 + sizeof(NX_PACKET)) * 32)

ULONG packet_pool_area[NX_PACKET_POOL_SIZE/sizeof(ULONG) + 64 / sizeof(ULONG)];

/* Define the ARP cache area.  */
ULONG arp_space_area[512 / sizeof(ULONG)];


/* Define the TLS Server thread.  */
ULONG             tls_server_thread_stack[6 * 1024 / sizeof(ULONG)];
TX_THREAD         tls_server_thread;
void              server_thread_entry(ULONG thread_input);

/* Define the TLS packet reassembly buffer. */
UCHAR tls_packet_buffer[18000];

/* Define the metadata area for TLS cryptography. The actual size needed can be
   Ascertained by calling nx_secure_tls_metadata_size_calculate.
*/
UCHAR tls_crypto_metadata[18000];

/* Pointer to the TLS ciphersuite table that is included in the platform-specific
   cryptography subdirectory. The table maps the cryptographic routines for the
   platform to function pointers usable by the TLS library.
*/
extern const NX_SECURE_TLS_CRYPTO nx_crypto_tls_ciphers_ecc;
extern const USHORT nx_crypto_ecc_supported_groups[];
extern const NX_CRYPTO_METHOD *nx_crypto_ecc_curves[];
extern const UINT nx_crypto_ecc_supported_groups_size;

/* Binary data for the TLS Server X.509 certificate, ASN.1 DER-encoded. Note that the
   certificate data and private key data is represented by an ellipsis for the sake
   of brevity.
*/
const UCHAR certificate_data[] = { … }; /* DER-encoded binary certificate. */
const UINT certificate_length = 0x574;

/* Binary data for the TLS Server Private Key, from private key
   file generated at the time of the X.509 certificate creation. ASN.1 DER-encoded. */
const UCHAR private_key[] = { … }; /* DER-encoded private key file (PKCS#1 RSA or ECC) */
const UINT private_key_length = 0x40;

/* Define some HTML data (web page) with an HTTPS header to serve to connecting
   clients. */
UCHAR html_data[] = { "HTTP/1.1 200 OK\r\n" \
        "Date: Tue, 19 May 2020 23:59:59 GMT\r\n" \
        "Content-Type: text/html\r\n" \
        "Content-Length: 200\r\n\r\n" \
        "<html>\r\n"\
        "<body>\r\n"\
        "<b>Hello NetX Duo Secure User!</b>\r\n"\
        "This is a simple webpage\r\n"\
        "served up using NetX Duo Secure!\r\n"\
        "</body>\r\n"\
        "</html>\r\n" };

/* Define the application – initialize drivers and TCP/IP setup.  */
void    tx_application_define(void *first_unused_memory)
{
UINT  status;


    /* Initialize the NetX system.  */
    nx_system_initialize();

    /* Create a packet pool. Check status for errors. */
    status =  nx_packet_pool_create(&pool_0, "NetX Main Packet Pool", 1536,
                                    (ULONG*)(((int)packet_pool_area + 64) & ~63) ,
                                    NX_PACKET_POOL_SIZE);

    /* Create an IP instance for the specific target. Check status for errors. */
    status = nx_ip_create(&ip_0, "NetX IP Instance 0",
                          DEVICE_IP_ADDRESS,
                          0xFFFFFF00UL,
                          &pool_0, nx_driver_xx,
                          (UCHAR*)ip_thread_stack,
                          sizeof(ip_thread_stack),
                          1);

    /* Enable ARP and supply ARP cache memory for IP Instance 0. Check status for
         errors. */
    status =  nx_arp_enable(&ip_0, (void *)arp_space_area, sizeof(arp_space_area));

    /* Enable TCP traffic. Check status for errors. */
    status =  nx_tcp_enable(&ip_0);

    status =  nx_ip_fragment_enable(&ip_0);

    /* Initialize the NetX Duo Secure TLS system.  */
    nx_secure_tls_initialize();

    /* Create the TLS server thread to start handling incoming requests. */
    tx_thread_create(&tls_server_thread, "TLS Server thread", server_thread_entry, 0,
                   tls_server_thread_stack, sizeof(tls_server_thread_stack),
                   16, 16, 4, TX_AUTO_START);
    return;
}

/* Thread to handle the TLS Server instance. */
void server_thread_entry(ULONG thread_input)
{
UINT       status;
ULONG      actual_status;
NX_PACKET *send_packet;
NX_PACKET *receive_packet;
UCHAR receive_buffer[100];
ULONG bytes;

    NX_PARAMETER_NOT_USED(thread_input);

    /* Ensure the IP instance has been initialized.  */
    status =  nx_ip_status_check(&ip_0, NX_IP_INITIALIZE_DONE, &actual_status,
                                 NX_IP_PERIODIC_RATE);

    /* Create a TCP socket to use for our TLS session.  */
    status =  nx_tcp_socket_create(&ip_0, &tcp_socket, "TLS Server Socket",
                                   NX_IP_NORMAL, NX_FRAGMENT_OKAY,
                                   NX_IP_TIME_TO_LIVE, 8192, NX_NULL, NX_NULL);

    /* Create a TLS session for our socket.  */
    status =  nx_secure_tls_session_create(&tls_session,
                                        &nx_crypto_tls_ciphers_ecc,
                                        tls_crypto_metadata,
                                        sizeof(tls_crypto_metadata));

    status = nx_secure_tls_ecc_initialize(&tls_session,
                                          nx_crypto_ecc_supported_groups,
                                          nx_crypto_ecc_supported_groups_size,
                                          nx_crypto_ecc_curves);

     /* Set the packet reassembly buffer for this TLS session. */
     status = nx_secure_tls_session_packet_buffer_set(&tls_session, tls_packet_buffer,
                                                      sizeof(tls_packet_buffer));

    /* Initialize an X.509 certificate and private ECC key for our TLS Session. */
    nx_secure_x509_certificate_initialize(&certificate, certificate_data, NX_NULL, 0,
                                          certificate_length, private_key,
                                          private_key_length,
                                          NX_SECURE_X509_KEY_TYPE_EC_DER);

    /* Add the initialized certificate as a local identity certificate. */
    nx_secure_tls_local_certificate_add(&tls_session, &certificate);


    /* Setup this thread to listen on the TCP socket.
       Port 443 is standard for HTTPS. */
    status =  nx_tcp_server_socket_listen(&ip_0, 443, &tcp_socket, 5, NX_NULL);

    while(1)
     {
         /* Accept a client TCP socket connection.  */
         status =  nx_tcp_server_socket_accept(&tcp_socket, NX_WAIT_FOREVER);

         /* Start the TLS Session using the connected TCP socket. */
         status = nx_secure_tls_session_start(&tls_session, &tcp_socket,
                                              NX_WAIT_FOREVER);

         /* Receive the HTTPS request. */
         status = nx_secure_tls_session_receive(&tls_session, &receive_packet,
                                                NX_WAIT_FOREVER);

if (status == NX_SUCCESS)
      {
         /* Extract the HTTP request information from the HTTPS request. */
            status = nx_packet_data_extract_offset(receive_packet, 0, receive_buffer,
                                                  100, &bytes);
         /* Display the HTTP request data. */
            receive_buffer[bytes] = 0;
            printf("Received data: %s\n", receive_buffer);

         /* Release the packet when done with it */
            nx_packet_release(receive_packet);
      }

         /* Allocate a TLS packet to send HTML data back to client. */
         status = nx_secure_tls_packet_allocate(&tls_session, &pool_0, &send_packet,
                                                NX_WAIT_FOREVER);

         /* Populate the packet with our HTTP response and HTML web page data. */
         nx_packet_data_append(send_packet, html_data, sizeof(html_data), &pool_0,
                               NX_WAIT_FOREVER);

         /* Send the HTTP response over the TLS Session, turning it into HTTPS. */
         status = nx_secure_tls_session_send(&tls_session, send_packet,
                                                 NX_WAIT_FOREVER);

         /* If the send fails, you must release the packet.  */
         if (status != NX_SUCCESS)
         {
              /* Release the packet since it was not sent.  */
              nx_packet_release(send_packet);
         }

         /* End the TLS session now that we have sent our HTTPS/HTML response. */
         status = nx_secure_tls_session_end(&tls_session, NX_WAIT_FOREVER);

         /* Check for errors to make sure the session ended cleanly! */

         /* Disconnect the TCP socket so we can be ready for the next request. */
         status =  nx_tcp_socket_disconnect(&tcp_socket, NX_WAIT_FOREVER);

         /* Unaccept the server socket.  */
         status =  nx_tcp_server_socket_unaccept(&tcp_socket);

         /* Setup server socket for listening again.  */
         status =  nx_tcp_server_socket_relisten(&ip_0, 443, &tcp_socket);
     }
}
```

## A Note on TLS Session Error Recovery

The example systems described above show the basic outlines for a TLS Client and Server, respectively, but for clarity the error handling is omitted. However, part of the security TLS provides is dependent on the proper handling of error conditions. Generally, the most serious potential problems will be handled within the TLS stack itself, but it is important for the TLS application to properly respond to and recover from TLS errors that are not handled within the TLS implementation.

In order to illustrate the necessary logic for proper error handling, the following function demonstrates a typical collection of API services that can be used to properly handle TLS errors and cleanly reset the TLS state after an error condition is encountered. Other than the section where noted, the logic applies to both TLS Client and TLS Server instances.

Note that the most important API calls in the function are to the services *nx_secure_tls_session_end*, which cleanly closes the TLS session or handshake, and *nx_secure_tls_session_reset*, which clears the TLS session state so the tls_session control structure instance can be reused for a new TLS session. Also note that *nx_secure_tls_session_reset* does not clear the user-configured state such as certificates or assigned buffers, allowing the session to be reused without calling *nx_secure_tls_session_create* again. To completely clear all TLS session state, the service *nx_secure_tls_session_delete* may be used instead.

```C
/* Define a helper function to clean up a broken TLS session (to be called on any
   error from nx_secure_tls_session_start onwards). Note that the variables
   tls_session, tcp_socket, and ip_0 are global in the above examples. */
VOID tls_session_error_cleanup(VOID)
{
UINT status;
UINT alert_level, alert_value;

      /* If we got an error back from a TLS API call, there may be an alert from the
         remote host. Extract the alert level and value to print out. Note that the TLS
         API will return NX_SECURE_TLS_ALERT_RECEIVED (0x114) if an alert was received.
         For other error codes the alert value and level are not valid. */
      status = nx_secure_tls_session_alert_value_get(&tls_session, &alert_level,
                                                     &alert_value);
      if(status)
      {
         printf("Pointer error in getting alert value.\n");
      }
      else
      {
         printf("Alert received. Value: %d, Level: %d\n", alert_value, alert_level);
      }

      /* End the TLS session. This is required to properly shut down the TLS
         connection. */
      status = nx_secure_tls_session_end(&tls_session, NX_WAIT_FOREVER);

      /* If the session did not shut down cleanly, this is a possible security issue. */
      if (status)
      {
         printf("Error in TLS session end: %x\n", status);
      }

      /* Reset the TLS session to re-use the control structure for the next connection.
         This API service resets the TLS session state but does not remove user-
         configured options such as certificates, PSKs, buffers, and cipher routines. */
      nx_secure_tls_session_reset(&tls_session);

      /* Disconnect the TCP socket, closing the connection. */
      status =  nx_tcp_socket_disconnect(&tcp_socket, NX_WAIT_FOREVER);

      /* Check for error.  */
      if (status)
      {
           printf("Error in TCP socket close: %x\n", status);
      }

   /* The following code applies only to a TLS server instance. */
   #if NX_SECURE_TLS_SERVER
      /* Unaccept the server socket.  */
      status =  nx_tcp_server_socket_unaccept(&tcp_socket);

      /* Check for error.  */
      if (status)
      {
           printf("Error in TCP socket unaccept: %x\n", status);
      }

      /* Setup server socket for listening again.  */
      status =  nx_tcp_server_socket_relisten(&ip_0, DEVICE_SERVER_PORT, &tcp_socket);

      /* Check for error.  */
      if (status)
      {
           printf("Error in TCP socket relisten: %x\n", status);
      }
#endif
} /* End function. */
```

## Configuration Options

There are several configuration options for building NetX Duo Secure. Following is a list of all options, where each is described in detail:

| Define | Meaning |
|----------------------|------------------------------------------------|
| **NX_SECURE_DISABLE_ERROR_CHECKING**                | Defined, this option removes the basic NetX Duo Secure error checking. It is typically used after the application has been debugged. |
| **NX_CRYPTO_MAX_RSA_MODULUS_SIZE**                  | Defined, this option gives the maximum RSA modulus expected, in bits. The default value is 4096 for a 4096-bit modulus. Other values can be 3072, 2048, or 1024 (not recommended). |
| **NX_SECURE_ALLOW_SELF_SIGNED_CERTIFICATES**        | Defined, this option allows TLS to accept self-signed certificates from a remote host. By default, TLS will reject self-signed server certificates as a security precaution. If this macro is defined, self-signed certificates must still be added to the trusted store to be accepted. |
| **NX_SECURE_ENABLE_CLIENT_CERTIFICATE_VERIFY**      | Defined, this option enables the optional X.509 Client Certificate Verification for TLS Servers<sup>1</sup>.  |
| **NX_SECURE_ENABLE_PSK_CIPHERSUITES**               | Defined, this option enables Pre-Shared Key (PSK) functionality. It does not disable digital certificates. |
| **NX_SECURE_TLS_MAX_PSK_ID_SIZE**                   | Defined, this option gives the maximum size of PSK ID. |
| **NX_SECURE_TLS_MAX_PSK_KEYS**                      | Defined, this option gives the maximum number of PSK keys. |
| **NX_SECURE_TLS_MAX_PSK_SIZE**                      | Defined, this option gives the maximum size of PSK. |
| **NX_SECURE_TLS_CLIENT_DISABLED**                   | Defined, this option removes all TLS stack code related to TLS Client mode, reducing code and data usage. |
| **NX_SECURE_TLS_SERVER_DISABLED**                   | Defined, this option removes all TLS stack code related to TLS Server mode, reducing code and data usage.  |
| **NX_SECURE_DISABLE_ECC_CIPHERSUITE**               | Defined, this option removes all TLS logic for Elliptic Curve Cryptography (ECC) ciphersuites. These ciphersuites are optional in TLS 1.2 and earlier and disabling them can result in significant code and data size reduction.|
| **NX_SECURE_TLS_ENABLE_TLS_1_3**                    | Defined, this option enables TLSv1.3 mode. TLS 1.3 is the newest version of TLS and is disabled by default. |
| **NX_SECURE_TLS_ENABLE_TLS_1_0**                    | Defined, this option enables the legacy TLSv1.0 mode. TLSv1.0 is considered obsolete so it should only be enabled for backward-compatibility with older applications. |
| **NX_SECURE_TLS_ENABLE_TLS_1_1**                    | Defined, this option enables the legacy TLSv1.1 mode. TLSv1.1 is considered obsolete so it should only be enabled for backward-compatibility with older applications. |
| **NX_SECURE_TLS_DISABLE_PROTOCOL_VERSION_DOWNGRADE**| Defined, this option disables protocol version downgrade for TLS client. |
| **NX_SECURE_AEAD_CIPHER_CHECK**                     | Defined, it allows to detect user-implemented AEAD algorithms other than AES-CCM or AES-GCM. It can be defined like #define NX_SECURE_AEAD_CIPHER_CHECK(a) ((a) == NEW_ALGORITHM_ID). It works only when `NX_SECURE_ENABLE_AEAD_CIPHER` is defined. |
| **NX_SECURE_ENABLE_AEAD_CIPHER**                    | Defined, this option enables AEAD ciphersuites. |
| **NX_SECURE_TLS_MINIMUM_CERTIFICATE_SIZE**          | Defined, this option gives the minimum reasonable size for a TLS X509 certificate. This is used in checking for errors in allocating certificate space. The size is determined by assuming a 512-bit RSA key, MD5 hash, and a rough estimate of other data. It is theoretically possible for a real certificate to be smaller, but in that case, bypass the error checking by re-defining this macro. Approximately: 64(RSA) + 16(MD5) + 176(ASN.1 + text data, common name, etc). |
| **NX_SECURE_TLS_MINIMUM_MESSAGE_BUFFER_SIZE**       | Defined, this option gives the minimum size for the TLS message buffer. It is determined by a number of factors, but primarily the expected size of the TLS handshake Certificate message (sent by the TLS server) that may contain multiple certificates of 1-2KB each. The upper limit is determined by the length field in the TLS header (16 bit), and is 64KB. |
| **NX_SECURE_TLS_PREMASTER_SIZE**                    | Defined, this option gives the maximum sie of pre-master secret. |
| **NX_SECURE_TLS_SNI_EXTENSION_DISABLED**            | Defined, this option disables Server Name Indication (SNI) extension. |
| **NX_SECURE_TLS_USE_SCSV_CIPHPERSUITE**             | Defined, this option enables SCSV ciphersuite in ClientHello message. |
| **NX_SECURE_TLS_DISABLE_SECURE_RENEGOTIATION**      | Defined, this option disables secure session renegotiation extension (RFC 5746). |
| **NX_SECURE_TLS_REQUIRE_RENEGOTIATION_EXT**         | Defined, this option will terminate the connection immediately upon failure to receive the secure renegotiation extension during the initial handshake. |
| **NX_SECURE_TLS_DISABLE_CLIENT_INITIATED_RENEGOTIATION** | Defined, this option disables client-initiated renegotiation for TLS servers. In some instances, client-initiated renegotiation can become a possible denial-of-service vulnerability. |
| **NX_SECURE_HASH_METADATA_CLONE**                   | Defined, this option allows custom hash clone during handshake state. |
| **NX_SECURE_HASH_CLONE_CLEANUP**                    | Defined, this option allows custom hash cleanup during handshake state. |
| **NX_SECURE_KEY_CLEAR**                             | Defined, this option enables key related materials cleanup when they are not used anymore. |
| **NX_SECURE_MEMCMP**                                | Defined, this option maps memory compare function in TLS. |
| **NX_SECURE_MEMCPY**                                | Defined, this option maps memory copy function in TLS. |
| **NX_SECURE_MEMMOVE**                               | Defined, this option maps memory move function in TLS. |
| **NX_SECURE_MEMSET**                                | Defined, this option maps memory set function in TLS. |
| **NX_SECURE_MEMSET**                                | Defined, this option maps memory set function in TLS. |
| **NX_SECURE_POWER_ON_SELF_TEST_MODULE_INTEGRITY_CHECK** | Defined, this option enables module integrity self test. |
| **NX_SECURE_RNG_CHECK_COUNT**                       | Defined, this option gives the maximum number of random number check for duplication during integrity self test. |
| **NX_SECURE_X509_USE_EXTENDED_DISTINGUISHED_NAMES** | Defined, this option enables the optional X.509 Distinguished Name fields, at the expense of extra memory use for X.509 certificates. |
| **NX_SECURE_X509_DISABLE_CRL**                      | Defined, this option disables X509 Certificate Revocation List check. |
| **NX_SECURE_X509_STRICT_NAME_COMPARE**              | Defined, this option enables strict X509 comparisons for all fields. |

1. Note that this option only enables the code to be linked into the application. The feature will need to be enabled with the API service nx_secure_tls_session_client_verify_enable or configured using nx_secure_tls_session_x509_client_verify_configure in order to use Client Certificate Verification.
