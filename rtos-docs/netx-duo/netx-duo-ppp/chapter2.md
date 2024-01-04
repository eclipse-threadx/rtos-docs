---
title: Chapter 2 - Installation and use of NetX Duo Point-to-Point Protocol (PPP)
description: This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo Point-to-Point Protocol (PPP) component.
---

# Chapter 2 - Installation and use of NetX Duo Point-to-Point Protocol (PPP)

This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo Point-to-Point Protocol (PPP) component.

## Product Distribution

The NetX Duo Point-to-Point Protocol (PPP) package is available at <https://github.com/azure-rtos/netxduo>. The package includes the following files:

- **nx_ppp.h**: Header file for PPP for NetX Duo
- **nx_ppp.c**: C Source file for PPP for NetX Duo
- **nx_ppp.pdf**: PDF description of PPP for NetX Duo
- **demo_netx_ppp.c**: NetX Duo PPP demonstration

## PPP Installation

In order to use PPP for NetX Duo, the entire distribution mentioned previously should be copied to the same directory where NetX Duo is installed. For example, if NetX Duo is installed in the directory "*\threadx\arm7\green*" then the *nx_ppp.h* and *nx_ppp.c* files should be copied into this directory.

## Using PPP

Using PPP for NetX Duo is easy. Basically, the application code must include *nx_ppp.h* after it includes *tx_api.h* and *nx_api.h*, in order to use ThreadX and NetX Duo, respectively. Once *nx_ppp.h* is included, the application code is then able to make the PPP function calls specified later in this guide. The application must also include *nx_ppp.c* in the build process. This file must be compiled in the same manner as other application files and its object form must be linked along with the files of the application. This is all that is required to use NetX Duo PPP.

## Using Modems

If a modem is required for connection to the internet, some special considerations are required in order to use the NetX Duo PPP product. Basically, using a modem introduces additional initialization logic and logic for loss of communication. In addition, most of the additional modem logic is done outside the context of NetX Duo PPP. The basic flow of using the NetX Duo PPP with a modem goes something like this:

1. Initialize Modem

1. Dial Internet Service Provider (ISP)

1. Wait for Connection

1. Wait for UserID Prompt

1. Start NetX Duo PPP [PPP in operation]

1. Loss of Communication

1. Stop NetX Duo PPP (or restart via nx_ppp_restart)

### Initialize Modem

Using the application's low-level serial output routine, the modem is initialized via a series of ASCII character commands (see modem's documentation for more details).

### Dial Internet Service Provider

Using the application's low-level serial output routine, the modem is instructed to dial the ISP. For example, the following is typical of an ASCII string used to dial an ISP at the number 123-4567:

"ATDT123456\r"

### Wait for Connection

At this point, the application waits to receive indication from the modem that a connection has been established. This is accomplished by looking for characters from the application's low-level serial input routine. Typically, modems return an ASCII string "CONNECT" when a connection has been established.

### Wait for User ID Prompt

Once the connection has been established, the application must now wait for an initial login request from the ISP. This typically takes the form of an ASCII string like "Login?"

### Start NetX Duo PPP

At this point, the NetX Duo PPP can be started. This is accomplished by calling the *nx_ppp_create* service followed by the *nx_ip_create* service. Additional services to enable PAP and to setup the PPP IP addresses might also be required. Please review the following sections of this guide for more information.

### Loss of Communication

Once PPP is started, any non-PPP information is passed to the "invalid packet handling" routine the application specified to the *nx_ppp_create* service. Typically, modems send an ASCII string such as "NO CARRIER" when communication is lost with the ISP. When the application receives a non-PPP packet with such information, it should proceed to either stop the NetX Duo PPP instance or to restart the PPP state machine via the *nx_ppp_restart* API.

### Stop NetX Duo PPP

Stopping the NetX Duo PPP is fairly straightforward. Basically, all created sockets must be unbound and deleted. Next, delete the IP instance via the *nx_ip_delete* service. Once the IP instance is deleted, the *nx_ppp_delete* service should be called to finish the process of stopping PPP. At this point, the application is now able to attempt to reestablish communication with the ISP.

## Small Example System

An example that illustrates how easy it is to use NetX Duo PPP is described below. In this example, the PPP include file *nx_ppp.h* is brought in at line 3. Next, PPP is created in *"tx_application_define"* at line 56. The PPP control block "*my_ppp*" was defined as a global variable at line 9 previously. 

> **Note:** PPP should be created prior to creating the IP instance. After successful creation of PPP and IP, the thread "*my_thread*" waits for the PPP link to come alive at line 98. At line 104, both PPP and NetX Duo are fully operational.

The one item not shown in this example is the application's serial byte receive ISR. It will need to call *nx_ppp_byte_receive* with "*my_ppp*" and the byte received as input parameters.

```c
0001 #include   "tx_api.h"
0002 #include   "nx_api.h"
0003 #include   "nx_ppp.h"
0004
#define     DEMO_STACK_SIZE         4096
TX_THREAD               my_thread;
NX_PACKET_POOL          my_pool;
NX_IP                   my_ip;
NX_PPP                  my_ppp;

/* Define function prototypes. */

void    my_thread_entry(ULONG thread_input);
void    my_serial_driver_byte_output(UCHAR byte);
void    my_invalid_packet_handler(NX_PACKET *packet_ptr);
 
/* Define main entry point. */
intmain()
{

    /* Enter the ThreadX kernel. */
    tx_kernel_enter();
 }


/* Define what the initial system looks like. */

void    tx_application_define(void *first_unused_memory)
{

CHAR    *pointer;
UINT    status;

/* Setup the working pointer. */
pointer =  (CHAR *) first_unused_memory;

/* Create "my_thread". */
    tx_thread_create(&my_thread, "my thread", my_thread_entry, 0,  
                  pointer, DEMO_STACK_SIZE, 
                  2, 2, TX_NO_TIME_SLICE, TX_AUTO_START);
    pointer =  pointer + DEMO_STACK_SIZE;

    /* Initialize the NetX system. */
    nx_system_initialize();

    /* Create a packet pool. */
    status =  nx_packet_pool_create(&my_pool, "NetX Main Packet Pool", 
                                    1024, pointer, 64000);
    pointer = pointer + 64000;

    /* Check for pool creation error. */
    if (status)
        error_counter++;

    /* Create a PPP instance. */
    status = nx_ppp_create(&my_ppp, "My PPP", &my_ip, pointer, 1024, 2, 
                           &my_pool, my_invalid_packet_handler, my_serial_driver_byte_output);
    pointer =  pointer + 1024;
    /* Check for PPP creation pool. */
    if (status)
        error_counter++;

    /* Create an IP instance with the PPP driver. */
    status = nx_ip_create(&my_ip,"My NetX IP Instance", 
                           IP_ADDRESS(216,2,3,1), 0xFFFFFF00, &my_pool, 
                           nx_ppp_driver, pointer, DEMO_STACK_SIZE, 1);
    pointer =  pointer + DEMO_STACK_SIZE;

    /* Check for IP create errors. */
    if (status)
        error_counter++;

    /* Enable ICMP for my IP Instance. */
    status =  nx_icmp_enable(&my_ip);

    /* Check for ICMP enable errors. */
    if (status)
        error_counter++;

    /* Enable UDP. */
    status =  nx_udp_enable(&my_ip);
    if (status)
        error_counter++;
}

/* Define my thread. */
void    my_thread_entry(ULONG thread_input)
{

UINT        status;
ULONG       ip_status;
NX_PACKET   *my_packet;

/* Wait for the PPP link in my_ip to become enabled. */
    status =  nx_ip_status_check(&my_ip,NX_IP_LINK_ENABLED,&ip_status,3000);

    /* Check for IP status error. */
    if (status) 
        return;

    /* Link is fully up and operational. All NetX activities 
    are now available. */

}
```
## Configuration Options

There are several configuration options for building PPP for NetX Duo. The following list describes each in detail:

- **NX_DISABLE_ERROR_CHECKING**: Defined, this option removes the basic PPP error checking. It is typically used after the application has been debugged.
- **NX_PPP_PPPOE_ENABLE**: If defined, PPP can transmit packet over Ethernet
- **NX_PPP_BASE_TIMEOUT**: This defines the period rate (in timer ticks) that the PPP thread task is woken to check for PPP events. The default value is 1*NX_IP_PERIODIC_RATE (100 ticks).
- **NX_PPP_DISABLE_INFO**: If defined, internal PPP information gathering is disabled.
- **NX_PPP_DEBUG_LOG_ENABLE**: If defined, internal PPP debug log is enabled.
- **NX_PPP_DEBUG_LOG_PRINT_ENABLE**:If defined, internal PPP debug log *printf* to *stdio* is enabled. This is only valid if the debug log is also enabled.
- **NX_PPP_DEBUG_LOG_SIZE**: Size of debug log (number of entries in the debug log). On reaching the last entry, the debug capture wraps to the first entry and overwrites any data previously captured. The default value is 50.
- **NX_PPP_DEBUG_FRAME_SIZE**: Maximum amount of data captured from a received packet payload and saved to debug output. The default value is 50.
- **NX_PPP_DISABLE_CHAP**: If defined, internal PPP CHAP logic is removed, including the MD5 digest logic.
- **NX_PPP_DISABLE_PAP**: If defined, internal PPP PAP logic is removed.
- **NX_PPP_DNS_OPTION_DISABLE**: If defined, the primary DNS Server Option is disabled in the IPCP response. By default this option is not defined.
- **NX_PPP_DNS_ADDRESS_MAX_RETRIES**: This specifies how many times the PPP host will request a DNS Server address from the peer in the IPCP state. This has no effect if NX_PPP_DNS_OPTION_DISABLE is defined. The default value is 2.
- **NX_PPP_HASHED_VALUE_SIZE**: Specifies the size of "hashed value" strings used in CHAP authentication. The default value is set to 16 bytes, but can be redefined prior to inclusion of *nx_ppp.h.*
- **NX_PPP_MAX_LCP_PROTOCOL_RETRIES**: This defines the max number of retries if the PPP times out before sending another LCP configure request message. When this number is reached the PPP handshake is aborted and the link status is down. The default value is 20.
- **NX_PPP_MAX_PAP_PROTOCOL_RETRIES**: This defines the max number of retries if the PPP times out before sending another PAP authentication request message. When this number is reached the PPP handshake is aborted and the link status is down. The default value is 20.
- **NX_PPP_MAX_CHAP_PROTOCOL_RETRIES**: This defines the max number of retries if the PPP times out before sending another CHAP challenge message. When this number is reached the PPP handshake is aborted and the link status is down. The default value is 20.
- **NX_PPP_MAX_IPCP_PROTOCOL_RETRIES**: This defines the max number of retries if the PPP times out before sending another IPCP configure request message. When this number is reached the PPP handshake is aborted and the link status is down. The default value is 20.
- **NX_PPP_MRU**: Specifies the Maximum Receive Unit (MRU) for PPP. By default, this value is 1,500 bytes (the minimum value). This define can be set by the application prior to inclusion of *nx_ppp.h*.
- **NX_PPP_MINIMUM_MRU**: Specifies the minimum MRU received in an LCP configure request message. By default, this value is 1,500 bytes (the minimum value). This define can be set by the application prior to inclusion of *nx_ppp.h*.
- **NX_PPP_NAME_SIZE**: Specifies the size of "name" strings used in authentication. The default value is set to 32bytes, but can be redefined prior to inclusion of *nx_ppp.h.
- **NX_PPP_PASSWORD_SIZE**: Specifies the size of "password" strings used in authentication. The default value is set to 32bytes, but can be redefined prior to inclusion of *nx_ppp.h.*
- **NX_PPP_PROTOCOL_TIMEOUT**: This defines the wait option (in seconds) for the PPP task to receive a response to a PPP protocol request message. The default value is 4 seconds.
- **NX_PPP_RECEIVE_TIMEOUTS**: This defines the number of times the PPP thread task times out waiting to receive the next character in a PPP message stream. Thereafter, PPP releases the packet and begins waiting to receive the next PPP message. The default value is 4.
- **NX_PPP_SERIAL_BUFFER_SIZE**: Specifies the size of the receive character serial buffer. By default, this value is 3,000 bytes. This define can be set by the application prior to inclusion of *nx_ppp.h*.
- **NX_PPP_TIMEOUT**: This defines the wait option (in timer ticks) for allocating packets to transmit data as well as buffer PPP serial data into packets to send to the IP layer. The default value is 4*NX_IP_PERIODIC_RATE (400 ticks).
- **NX_PPP_THREAD_TIME_SLICE**: Time-slice option for PPP threads. By default, this value is TX_NO_TIME_SLICE. This define can be set by the application prior to inclusion of *nx_ppp.h*.
- **NX_PPP_VALUE_SIZE**: Specifies the size of "value" strings used in CHAP authentication. The default value is set to 32bytes, but can be redefined prior to inclusion of nx_ppp.h.