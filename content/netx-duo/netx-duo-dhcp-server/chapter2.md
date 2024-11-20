---
title: Chapter 2 - Installation and Use of the NetX Duo DHCP Server
description: This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo DHCP component.
---


This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo DHCP component.

## Product Distribution

The NetX Duo DHCP Server is available at [https://github.com/eclipse-threadx/netxduo](https://github.com/eclipse-threadx/netxduo). The package includes two source files and a PDF file that contains this document, as follows:

- **nxd_dhcp_server.h** Header file for NetX Duo DHCP Server
- **nxd_dhcp_server.c** C Source file for NetX Duo DHCP Server
- **nxd_dhcp_server.pdf** User Guide for NetX Duo DHCP Server
- **demo_netxduo_dhcp.c** NetX Duo DHCP Server demonstration

## DHCP Installation

In order to use NetX Duo DHCP Server, the entire distribution mentioned previously should be copied to the same directory where NetX Duo is installed. For example, if NetX Duo is installed in the directory "*\threadx\arm7\green*" then the *nxd_dhcp_server.h* and *nxd_dhcp_server.c* files should be copied into this directory.

## Using NetX Duo DHCP Server

Using NetX Duo DHCP Server is easy. Basically, the application code must include *nx_dhcp_server.h* after it includes *tx_api.h* and *nx_api.h*, in order to use ThreadX and NetX Duo, respectively. Once *nxd_dhcp_server.h* is included, the application code is then able to make the DHCP function calls specified later in this guide. The application must also include *nxd_dhcp_server.c* in the build process. This file must be compiled in the same manner as other application files and its object form must be linked along with the files of the application. For more details on using NetX Duo DHCP Server, see the following sections **Requirements of the NetX Duo DHCP** **Server** and **Constraints of the NetX Duo DHCP Server**.

Note that since DHCP utilizes NetX Duo UDP services, UDP must be enabled with the *nx_udp_enable* call prior to using DHCP.

## Requirements of the NetX Duo DHCP Server

The NetX Duo DHCP Server requires a UDP socket port assigned to the well known DHCP port 67. To create the DHCP Server, the application must create a packet pool with packet payload at least 548 bytes plus IP, UDP and Ethernet headers (which total 44 bytes with 4 byte alignment).

It is assumed that the Server and Client are both using Ethernet hardware address settings:

- Hardware type 1
- Hardware length 6
- Hops 0

### Multiple Client Sessions

The NetX Duo DHCP Server can handles multiple Client sessions by keeping a table of active DHCP clients and what 'state' the Client is in e.g. DHCP states INIT, BOOT, SELECTING, REQUESTING, RENEWING etc. If the session time out expires before receiving the next Client message, unless that Client is bound to an IP lease, the Server will clear the Client session data and return the assigned IP address back to the available pool. If the Server receives multiple DISCOVER messages from the same Client the Server resets the session time out and keeps the IP address reserved for the Client to accept in a subsequent REQUEST message.

The NetX Duo DHCP Server also accepts the single state Client DHCP request e.g. the Client only sends a REQUEST message. This assumes the Client has been previously assigned an IP lease from the DHCP server.

### Setting Interface Specific Network Parameters Server Responses

The application can set the router, subnet mask and DNS server parameters for each interface it handles DHCP Client requests, using the *nx_dhcp_set_interface_network_parameters* service. Otherwise these parameters are defaulted to the IP gateway on the Server's primary interface, its DHCP network subnet, and DHCP Server IP address, respectively.

The DHCP server includes these parameters in the option data of DHCP messages it sends to DHCP clients.

### Assigning IP addresses to the Client

If the Client DISCOVER message does not specify a requested IP address, the DHCP Server can use one from its own pool. If the Server has no available IP addresses it will send the Client a NACK message.

The NetX Duo DHCP Server will grant the requested IP address in the Client REQUEST message as long as the IP address is available and can be found in the Server IP address database. The application creates the Server's list of available IP addresses for assigning to DHCP Clients using the *nx_dhcp_create_server_ip_address_list* service. If the Server does not have the requested IP addresses or it is assigned to another host it will send the Client a NACK message.

When the DHCP Server receives a Client request, it identifies that Client uniquely using the Client MAC address in the Client MAC address field in the DHCP message. If the Client changes it's MAC address or is moved elsewhere onto another subnet it should send a RELEASE message to the Server to return the IP address back to the available pool, and request a new IP address in the INIT state.

See the **Small Example System** section for details. The number of IP addresses saved to the DHCP Server instance is limited to the size of the server address array in the DHCP Server control block, and defined by the configurable option NX_DHCP_IP_ADDRESS_MAX_LIST_SIZE.

### IP Address Lease Times

The DHCP Server will also accept the request Client lease time if that lease time is less than the Server default lease time which is defined in configurable option NX_DHCP_DEFAULT_LEASE_TIME. Renewal and rebind times assigned to the Client are 50% and 85% of the lease time, respectively, unless the lease time is infinity (0xFFFFFFFF), in which case renewal and rebind times are also set to infinity.

### DHCP Server Timeouts

The DHCP Server has a user configurable session timeout, NX_DHCP_CLIENT_SESSION_TIMEOUT, for waiting for the next DHCP Client message unless the session is completed. The time out is reset when the Server receives the next message from the Client regardless if is the same message previously sent.

### Internal error handling

The DHCP Server receives and processes DHCP Client packets in the *nx_dhcp_listen_for_messages* function. This function will discontinue processing the current DHCP Client packet if the packet is invalid, or the DHCP Server encounters an internal error. n*x_dhcp_listen_for_messages* returns an error status. The DHCP Server thread relinquishes control briefly of the ThreadX scheduler before calling this function to receive the next DHCP Client message. In the current release there is no logging support for error status returns from *nx_dhcp_listen_for_messages.*

### Option 55: Parameter Request List

The NetX Duo DHCP Server must be configured with a set of options to load to Parameter Request Option (55) list in the OFFER and DHCPACK messages it transmits back to the Client. These options should include network critical configuration data for the Client network and by default is defined to be router IP address, subnet mask, and DNS server. The option list is a space delimited list and defined in the user configurable NX_DHCP_DEFAULT_SERVER_OPTION_LIST. Note the number of options specified in this list must equal NX_DHCP_DEFAULT_OPTION_LIST_SIZE which is also user defined.

## Constraints of the NetX Duo DHCP Server

### DHCP Messages

The NetX Duo DHCP Server does not verify that an IP address has not been assigned elsewhere on the network before granting the IP address to the Client. If there are multiple DHCP servers, this can indeed be the case. *As per RFC 2131, it is the Client's responsibility to verify the IP address is unique on its network* (e.g. pinging the address). If it is not, the Server should receive a DECLINE message with the IP address to update its database from the Client.

The NetX Duo DHCP Server does not issue FORCE_RENEW messages. It is up to the DHCP Client to renew its IP address lease. However, the DHCP Server monitors the time remaining on all the assigned IP addresses in its database. When an IP address lease expires that IP address is returned to the pool of available IP addresses. Hence it is up to the Client to actively renew/rebind its IP address lease.

Session data is cleared as soon as the Client either is granted ("bound") to an IP lease (or an existing one is renewed). If a Client packet proves bogus, or the Client times out between responses, session data is cleared.

### Saving Data Between Reboots

The NetX Duo DHCP Server saves Client data including DHCP request parameters in a Client record table. This table is not stored in non-volatile memory, so if the DHCP Server host must reboot that information is not saved between reboots.

The NetX Duo DHCP Server saves IP address lease data in a IP address table. This table is not stored in non-volatile memory, so if the DHCP Server host must reboot that information is not saved between reboots.

### Relay Agents

The NetX Duo DHCP Server is configured with a zero IP address for the 'Relay agent' field because it does not support out of network DHCP requests.

## Small Example System

An example of how easy it is to use the NetX Duo DHCP Server is described below. In this example, the DHCP include file *nxd_dhcp_server.h* is brought in at line 5. DHCP Server thread stack size, IP thread stack size and test thread stack size are all defined in lines 7-13.

First, an optional test thread task for stopping, restarting and eventually deleting the DHCP server is created with the "*test_thread_entry*" function at line 57. A DHCP Server control block "*dhcp_server*" is defined as a global variable at line 20. Note that the server packet pool is created with packets having a payload at least as large as the standard DHCP message (548 bytes plus IP and UDP header bytes). After successfully creating an IP instance for the DHCP Server, the application creates the DHCP Server in line 96. Next, the application enables the Server IP instance to be UDP enabled. Before starting the DHCP Server, the available IP address list is created in line 137 using the **nx_dhcp_create_server_ip_address_list** service. The network configuration parameters are set in the following line 138 using the **nx_dhcp_set_interface_network_parameters** service, DHCP Server services become available when the application calls the *nx_dhcp_server_start* at line 141. The test thread task demonstrates the use of stopping and restarting the DHCP server.

```C
/* This is a small demo of NetX Duo DHCP Server for the high-performance NetX Duo TCP/IP stack.  */

#include   "tx_api.h"
#include   "nx_api.h"
#include   "nxd_dhcp_server.h"

#define     DEMO_TEST_STACK_SIZE         2048
#define     DEMO_SERVER_STACK_SIZE  2048
#define     SERVER_IP_ADDRESS_LIST  "192.168.2.10 192.168.2.11 192.168.2.12"
#define     PACKET_PAYLOAD          1000
#define     PACKET_POOL_SIZE        (PACKET_PAYLOAD * 10)
#define     SERVER_IP_THREAD_STACK    2048


/* Define the ThreadX and NetX Duo object control blocks...  */

TX_THREAD test_thread;
NX_PACKET_POOL server_pool;
NX_IP server_ip;
NX_DHCP_SERVER dhcp_server;


/* Define the counters used in the demo application...  */

ULONG state_changes;


/* Define thread prototypes.  */

void test_thread_entry(ULONG thread_input);
void nx_etherDriver_mcf5485(struct NX_IP_DRIVER_STRUCT *driver_req);


/* Define main entry point.  */

int main()
{
    /* Enter the ThreadX kernel.  */
    tx_kernel_enter();
}


/* Define what the initial system looks like.  */

void    tx_application_define(void *first_unused_memory)
{
    CHAR    *pointer;
    UINT    status;


    /* Setup the working pointer.  */
    pointer =  (CHAR *) first_unused_memory;

    /* Create the test thread.  */
    status = tx_thread_create(&test_thread, "test thread", test_thread_entry, 0,
            pointer, TEST_STACK_SIZE,  1, 1, TX_NO_TIME_SLICE, TX_DONT_START);

    if (status)
    {
        printf("Error with DHCP test thread create. Status 0x%x\r\n", status);
        return;
    }

    pointer =  pointer + DEMO_STACK_SIZE;

    /* Initialize the NetX Duosystem.  */
    nx_system_initialize();

    /* Create the DHCP Server packet pool.  */
    status =  nx_packet_pool_create(&server_pool, "NetX Main Packet Pool", PACKET_PAYLOAD,
        pointer, PACKET_POOL_SIZE);
    pointer = pointer + PACKET_POOL_SIZE;

    /* Check for pool creation error.  */
    if (status)
    {
        printf("Error with DHCP server packet pool create. Status 0x%x\r\n", status);
        return;
    }

    /* Create the DHCP Server IP instance.  */
    status = nx_ip_create(&server_ip, "NetX DHCP Server IP", NX_DHCP_SERVER_IP_ADDRESS,
        0xFFFFFF00UL,  &server_pool, nx_etherDriver_mcf5485, pointer,
        R_IP_THREAD_STACK, 1);

    pointer =  pointer + DEMO_IP_THREAD_STACK;

    /* Check for IP create errors.  */
    if (status)
    {
        printf("Error with DHCP server IP task create. Status 0x%x\r\n", status);
        return;
    }

    /* Create the DHCP Server instance.  */
    status =  nx_dhcp_server_create(&dhcp_server, &server_ip, pointer,
                                     DEMO_SERVER_STACK_SIZE,"DHCP Server", &server_pool);

    if (status)
    {
        printf("Error with DHCP server create. Status 0x%x\r\n", status);
        return;
    }

    pointer = pointer + DEMO_SERVER_STACK_SIZE;

    /* Enable ARP and supply ARP cache memory for IP Instance 0.  */
    status =  nx_arp_enable(&server_ip, (void *) pointer, 1024);
    pointer = pointer + 1024;

    /* Check for ARP enable errors.  */
    if (status)
    {
        printf("Error with ARP enable. Status 0x%x\r\n", status);
        return;
    }

    /* Enable UDP traffic.  */
    status =  nx_udp_enable(&server_ip);

    /* Check for UDP enable errors.  */
    if (status)
    {
        printf("Error with ICMP enable. Status 0x%x\r\n", status);
        return;
    }

    /* Enable ICMP to enable the ping utility.  */
    status =  nx_icmp_enable(&server_ip);

    /* Check for errors.  */
    if (status)
    {
        printf("Error with ICMP enable. Status 0x%x\r\n", status);
    }

   status = nx_dhcp_create_server_ip_address_list(&dhcp_server, iface_index,
                                 START_IP_ADDRESS_LIST, END_IP_ADDRESS_LIST, &addresses_added);

   status = nx_dhcp_set_interface_network_parameters(&dhcp_server, iface_index,
                               NX_DHCP_SUBNET_MASK, IP_ADDRESS(10,0,0,1),
                               IP_ADDRESS(10,0,0,1));

    /* Start the DHCP Server.  */
   status =  nx_dhcp_server_start(&dhcp_server);

    tx_thread_resume(&test_thread);
}

/* Define the test thread.  */
void    test_thread_entry(ULONG thread_input)
{
    UINT status;
    UINT keep_spinning;


    /* Just let the test thread be idle till we're ready to shut things down. */
    keep_spinning = 1;
    while(keep_spinning)
    {
        tx_thread_sleep(300);
    }

    printf("Stopping the server...\n");
    status = nx_dhcp_server_stop(&dhcp_server);
    if (status)
    {
        printf("Error with DHCP server stop. Status 0x%x\r\n", status);
        return;
    }

    tx_thread_sleep(500);

    printf("Starting the server...\n");
    status = nx_dhcp_server_start(&dhcp_server);
    if (status)
    {
        printf("Error with DHCP server start. Status 0x%x\r\n", status);
        return;
    }


    tx_thread_sleep(600);

    printf("Stopping the server for good...\n");
    status = nx_dhcp_server_stop(&dhcp_server);
    if (status)
    {
        printf("Error with DHCP server stop. Status 0x%x\r\n", status);
        return;
    }

    tx_thread_sleep(200);


    printf("Deleting the server...\n");
    status = nx_dhcp_server_delete(&dhcp_server);
    if (status)
    {
        printf("Error with DHCP server delete. Status 0x%x\r\n", status);
        return;
    }
}

```

## Configuration Options

There are several configuration options for building NetX Duo DHCP Server. The following list describes each in detail:

- **NX_DISABLE_ERROR_CHECKING**: This option removes the basic DHCP error checking. It it typically used after the application is debugged.
- **NX_DHCP_SERVER_THREAD_PRIORITY**: This option specifies the priority of the DHCP Server thread. By default, this value specifies that the DHCP thread runs at priority 2.
- **NX_DHCP_TYPE_OF_SERVICE**: This option specifies the type of service required for the DHCP UDP requests. By default, this value is defined as NX_IP_NORMAL to indicate normal IP packet service.
- **NX_DHCP_FRAGMENT_OPTION**: Fragment enable for DHCP UDP requests. By default, this value is set to NX_DONT_FRAGMENT to disable UDP fragmenting.
- **NX_DHCP_TIME_TO_LIVE**: Specifies the number of routers the packet can pass before it is discarded. The default value is 0x80.
- **NX_DHCP_QUEUE_DEPTH**: Specifies the number of packets that the DHCP Server socket keeps before flushing the queue. The default value is 5.
- **NX_DHCP_PACKET_ALLOCATE_TIMEOUT**: Specifies the timeout in timer ticks for the NetX Duo DHCP Server to wait for allocate a packet from its packet pool. The default value is set to NX_IP_PERIODIC_RATE.
- **NX_DHCP_SUBNET_MASK** This is the subnet mask the DHCP Client should be configured with. The default value is set to 0xFFFFFF00.
- **NX_DHCP_FAST_PERIODIC_TIME_INTERVAL**: This is timeout period in timer ticks for the DHCP Server fast timer to check on session time remaining and handle sessions that have timed out.
- **NX_DHCP_SLOW_PERIODIC_TIME_INTERVAL**: This is timeout period in timer ticks for the DHCP Server slow timer to check on IP address lease time remaining and handle lease that have timed out.
- **NX_DHCP_CLIENT_SESSION_TIMEOUT**: This is timeout period in timer ticks the DHCP Server will wait to receive the next DHCP Client message.
- **NX_DHCP_DEFAULT_LEASE_TIME**: This is IP Address lease time in seconds assigned to the DHCP Client, and the basis for computing the renewal and rebind times also assigned to the Client. The default value is set to 0xFFFFFFFF (infinity).
- **NX_DHCP_IP_ADDRESS_MAX_LIST_SIZE**: This is size of the DHCP Server array for holding available IP addresses for assigning to the Client. The default value is 20.
- **NX_DHCP_CLIENT_RECORD_TABLE_SIZE**: This is size of the DHCP Server array for holding Client records. The default value is 50.
- **NX_DHCP_CLIENT_OPTIONS_MAX**: This is size of the array in the DHCP Client instance for holding the all the requested options in the parameter request list in the current session. The default value is 12.
- **NX_DHCP_OPTIONAL_SERVER_OPTION_LIST**: This is the buffer holding the DHCP Server's default list of options to supply to the current DHCP Client in the parameter request list. The default is "1 3 6."
- **NX_DHCP_OPTIONAL_SERVER_OPTION_SIZE**: This is the size of the array to hold the DHCP Server's default list of options. The default value is 3.
- **NX_DHCP_SERVER_HOSTNAME_MAX**: This is size of the buffer for holding the Server host name. The default value is 32.
- **NX_DHCP_CLIENT_HOSTNAME_MAX**: This is size of the buffer for holding the Client host name in the current DHCP Server Client session. The default value is 32.
