---
title: Chapter 2 - Installation and use of NetX Duo POP3 Client
description: The NetX Duo POP3 Client includes one source file, one header file, and a demo file.
---

# Chapter 2 - Installation and Use of NetX Duo POP3 Client

NetX Duo POP3 Client includes one source file, one header file, and a demo file. There are two additional files for MD5 digest services. There is also a User Guide PDF file (this document).

- **nxd_pop3_client.c** C Source file for NetX Duo POP3 Client API
- **nxd_pop3_client.h** C Header file for NetX Duo POP3 Client API
- **demo_netxduo_pop3_client.c** Demo file for POP3 Client creation and session initiation
- **nx_md5.c** C Source file defining MD5 digest services
- **nx_md5.h** C Header file defining MD5 digest services
- **nxd_pop3_client.pdf** NetX Duo POP3 Client User Guide

To use NetX Duo POP3 Client, the entire distribution mentioned previously can be copied to the same directory where NetX Duo is installed. For example, if NetX Duo is installed in the directory "*\threadx\mcf5272\green*" then the *nx_md5.h*, *nx_md5.c,* *nxd_pop3_client.h, and nxd_pop3_client.c* files should be copied into this directory.

## Using NetX Duo POP3 Client

To use the NetX Duo POP3 Client service, the application must add *nxd_pop3_client.c* to its build project. The application code must include *nx_md5.h, and nxd_pop3_client.h* after *tx_api.h* and *nx_api.h*, in order to use ThreadX and NetX Duo.

These files must be compiled in the same manner as other application files and the object code must be linked along with the files of the application. This is all that is required to use the NetX Duo POP3 Client.

## Small Example of the NetX Duo POP3 Client

An example of how to use NetX Duo POP3 Client services is described in Figure 1 that appears below. This demo sets up the two callbacks for notification of mail download and session completion on lines 37 and 38. The POP3 Client packet pool is created on line 76. The IP thread task is created on line 88. Note that this packet pool is also used for the POP3 Client packet pool. TCP is enabled on the IP task in line 107.

The POP3 Client is created on line 133 inside the application thread entry function, *demo_thread_entry*. This is because the *nx_pop3_client_create* service also attempts to make a TCP connection with the POP3 server. If successful, the application queries the POP3 server for the number of items in its maildrop on line 149 using the *nx_pop3_client_mail_items_get* service.

If there are one or more items, the application iterates through the while loop for each mail item to download the mail message. The RETR request is made on line 149 in the *nx_pop3_client_mail_item_get* call. If successful, the application downloads packets using the *nx_pop3_client_mail_item_message_get* service on line 177 till it detects the last packet in the message has been received on line 196. Lastly, the application deletes the mail item, assuming a successful download has occurred on line 199 in *the nx_pop3_client_mail_item_delete* call. The RFC 1939 recommends that POP3 Clients instruct the Server to delete downloaded mail items to prevent mail accumulating in the Client's maildrop. The Server may automatically do so anyway.

Once all the mail items are downloaded, or if a POP3 Client service call fails, the application exits of the loop and deletes the POP3 Client on line 217 using the *nx_pop3_client_delete* service.

```C
/*
   demo_netxduo_pop3.c

   This is a small demo of POP3 Client on the NetX Duo TCP/IP stack.
   This demo relies on Thread, NetX Duo and POP3 Client API to conduct
   a POP3 mail session.
 */

#include  "tx_api.h"
#include  "nx_api.h"
#include  "nxd_pop3_client.h"

#define DEMO_STACK_SIZE             4096
#define CLIENT_ADDRESS              IP_ADDRESS(192,2,2,61)
#define SERVER_ADDRESS              IP_ADDRESS(192,2,2,89)
#define SERVER_PORT                 110

/* Replace the 'ram' driver with your own Ethernet driver. */
void    _nx_ram_network_driver(struct NX_IP_DRIVER_STRUCT *driver_req);

/* Set up the POP3 Client.  */

TX_THREAD           demo_client_thread;
NX_POP3_CLIENT      demo_client;
NX_PACKET_POOL      client_packet_pool;
NX_IP               client_ip;

#define PAYLOAD_SIZE 500

/* Set up Client thread entry point. */
void    demo_thread_entry(ULONG info);


  /* Shared secret is the same as password. */

#define LOCALHOST                               "recipient@domain.com"
#define LOCALHOST_PASSWORD                      "testpwd"

/* Define main entry point.  */
int main()
{
    /* Enter the ThreadX kernel.  */
    tx_kernel_enter();
}

/* Define what the initial system looks like.  */
void    tx_application_define(void *first_unused_memory)
{

    UINT     status;
    UCHAR    *free_memory_pointer;


    /* Setup the working pointer.  */
    free_memory_pointer =  first_unused_memory;

    /* Create a client thread.  */
    tx_thread_create(&demo_client_thread, "Client", demo_thread_entry, 0,
                     free_memory_pointer, DEMO_STACK_SIZE, 1, 1,
                     TX_NO_TIME_SLICE, TX_AUTO_START);

    free_memory_pointer =  free_memory_pointer + DEMO_STACK_SIZE;

    /* Initialize the NetX system.  */
    nx_system_initialize();

    /* The demo client username and password is the authentication
       data used when the server attempts to authentication the client. */

    /* Create Client packet pool. */
    status =  nx_packet_pool_create(&client_packet_pool,"POP3 Client Packet Pool",
                                    PAYLOAD_SIZE, free_memory_pointer,
                                    (PAYLOAD_SIZE * 10));
    if (status != NX_SUCCESS)
    {
        return;
    }

    /* Update pointer to unallocated (free) memory. */
    free_memory_pointer = free_memory_pointer + (PAYLOAD_SIZE * 10);


    /* Create IP instance for demo Client */
    status = nx_ip_create(&client_ip, "POP3 Client IP Instance",
                          CLIENT_ADDRESS, 0xFFFFFF00UL, &client_packet_pool,
                          _nx_ram_network_driver, free_memory_pointer,
                          2048, 1);

    if (status != NX_SUCCESS)
    {
        return;
    }

    /* Update pointer to unallocated (free) memory. */
    free_memory_pointer =  free_memory_pointer + 2048;

    /* Enable ARP and supply ARP cache memory. */
    nx_arp_enable(&client_ip, (void **) free_memory_pointer, 1024);

    /* Update pointer to unallocated (free) memory. */
    free_memory_pointer = free_memory_pointer + 1024;

    /* Enable TCP and ICMP for Client IP. */
    nx_tcp_enable(&client_ip);
    nx_icmp_enable(&client_ip);

    return;
}



/* Define the application thread entry function. */

void    demo_thread_entry(ULONG info)
{

    UINT        status;
    UINT        mail_item, number_mail_items;
    UINT        bytes_downloaded = 0;
    UINT        final_packet = NX_FALSE;
    ULONG       total_size, mail_item_size, bytes_retrieved;
    NX_PACKET   *packet_ptr;

    /* Let the IP instance get initialized with driver parameters. */
    tx_thread_sleep(40);


    /* Create a NetX POP3 Client instance with no byte or block memory pools.
       Note that it uses its password for its APOP shared secret. */
    status =  nx_pop3_client_create(&demo_client,
                                    NX_TRUE,
                                    &client_ip, &client_packet_pool, SERVER_ADDRESS,
                                    SERVER_PORT, LOCALHOST, LOCALHOST_PASSWORD);

    /* Check for error. */
    if (status != NX_SUCCESS)
    {

        status = nx_pop3_client_delete(&demo_client);

        /* Abort. */
        return;
    }

    /* Find out how many items are in our mailbox.  */
    status = nx_pop3_client_mail_items_get(&demo_client, &number_mail_items,
                                            &total_size);

    printf("Got %d mail items, total size%d \n", number_mail_items, total_size);

    /* If nothing in the mailbox, disconnect. */
    if (number_mail_items == 0)
    {

        nx_pop3_client_delete(&demo_client);

        return;
    }

    /* Download all mail items.  */
    mail_item = 1;

    while (mail_item <= number_mail_items)
    {


        /* This submits a RETR request and gets the mail message size. */
        status = nx_pop3_client_mail_item_get(&demo_client, mail_item,
                                               &mail_item_size);

        /* Loop to get all mail message packets until the mail item is completely
           downloaded. */
        while((final_packet ==  NX_FALSE) && (status == NX_SUCCESS))
        {

            status = nx_pop3_client_mail_item_message_get(&demo_client, &packet_ptr,
                                                        &bytes_retrieved,
                                                        &final_packet);

            if (status != NX_SUCCESS)
            {

                break;
            }

            if (bytes_retrieved != 0)
            {

                printf("Received %d bytes of data for item %d: %s\n",
                        packet_ptr -> nx_packet_length,
                        mail_item, packet_ptr -> nx_packet_prepend_ptr);
            }

            nx_packet_release(packet_ptr);

            /* Determine if this is the last data packet. */
            if (final_packet)
            {
                /* It is. Let the server know it can delete this mail item. */
                status = nx_pop3_client_mail_item_delete(&demo_client, mail_item);
            }

            /* Keep track of how much mail message data is left. */
            bytes_downloaded += bytes_retrieved;
        }

        /* Get the next mail item. */
        mail_item++;

        tx_thread_sleep(100);

    }

    /* Disconnect from the POP3 server. */
    status = nx_pop3_client_quit(&demo_client);

    /* Delete the POP3 Client.  This will not delete the Client packet pool. */
    status = nx_pop3_client_delete(&demo_client);

}
```

**Figure 1. Example of a NetX Duo POP3 Client application**

## POP3 Client Configuration Options

There are several configuration options with the NetX Duo POP3 Client. Following is a list of all options described in detail:

- **NX_POP3_CLIENT_PACKET_TIMEOUT** This defines the wait option in seconds for the POP3 Client to allocate a packet. The default value is 1 second.
- **NX_POP3_CLIENT_CONNECTION_TIMEOUT** This defines the wait option in seconds for the POP3 Client to connect with the POP3 Server. The default value is 30 seconds.
- **NX_POP3_CLIENT_DISCONNECT_TIMEOUT** This defines the wait option in seconds for the POP3 Client to disconnect from the POP3 Server. The default value is 2 seconds.
- **NX_POP3_TCP_SOCKET_SEND_WAIT** This option sets the wait option in seconds in *nx_tcp_socket_send* service calls. The default value is 2 seconds.
- **NX_POP3_SERVER_REPLY_TIMEOUT** This option sets the wait option in *nx_tcp_socket_receive* service calls for the Server reply to a Client request. The default value is 10 seconds.
- **NX_POP3_CLIENT_TCP_WINDOW_SIZE** This option sets the size of the Client TCP receive window. This should be set to the IP instance MTU size minus the IP and TCP header. The default value is 1460. This should be less if the application is sending POP3 packets over IPv6 (1440 bytes) to account for the larger IPv6 header.
- **NX_POP3_MAX_USERNAME** This option sets the size of the buffer of the POP3 Client user name. The default value is 40 bytes.
- **NX_POP3_MAX_PASSWORD** This option sets the size of the buffer of the POP3 Client password. The default value is 20 bytes.
