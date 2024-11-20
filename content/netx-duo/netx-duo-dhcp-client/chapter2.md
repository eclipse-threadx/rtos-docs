---
title: Chapter 2 - Installation and use of NetX Duo DHCP Client
description: This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo DHCP Client component.
---


This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo DHCP Client component.

## Product Distribution

NetX Duo can be obtained from our public source code repository at <https://github.com/eclipse-threadx/netxduo>. The package includes the following files:

- **nxd_dhcp_client.h**: Header file for NetX Duo DHCP
- **nxd_dhcp_client.c**: C Source file for DHCP NetX Duo
- **nxd_dhcp_client.pdf**: User Guide for NetX Duo DHCP
- **demo_netxduo_dhcp.c**: NetX Duo DHCP Client demonstration
- **demo_netxduo_multihome_dhcp_client.c**: NetX Duo DHCP Client demonstration of DHCP on multiple interfaces

## DHCP Installation

To use NetX Duo DHCP Client, the entire distribution mentioned previously should be copied to the same directory where NetX Duo is installed. For example, if NetX Duo is installed in the directory "*\threadx\arm7\green*" then the *nxd_dhcp_client.h* and *nxd_dhcp_client.c* files should be copied into this directory.

## Using DHCP

Using DHCP for NetX Duo is easy. Basically, the application code must include *nxd_dhcp_client.h* after it includes *tx_api.h* and *nx_api.h*, in order to use ThreadX and NetX Duo, respectively. Once *nxd_dhcp_client.h* is included, the application code is then able to make the DHCP function calls specified later in this guide. The application must also include *nxd_dhcp_client.c* in the build process. This file must be compiled in the same manner as other application files and its object form must be linked along with the files of the application. This is all that is required to use NetX Duo DHCP.

Note that since DHCP utilizes NetX Duo UDP services, UDP must be enabled with the *nx_udp_enable* call prior to using DHCP.

To obtain a previously assigned IP address, the DHCP Client can initiate the DHCP process with the Request message and Option 50 "Requested IP Address" to the DHCP Server. The DHCP Server will respond with either an ACK message if it grants the IP address to the Client or a NACK if it refuses. In the latter case, the DHCP Client restarts the DHCP process at the Init state with a Discover message and no requested IP address. The host application first creates the DHCP Client, then calls the *nx_dhcp_request_client_ip* API service to set the requested IP address before starting the DHCP process with *nx_dhcp_start*. An example DHCP application is provided elsewhere in this document for more details.

## In the Bound State

While the DHCP Client is in the bound state, the DHCP Client thread processes the Client state once per interval (as specified by NX_DHCP_TIME_INTERVAL) and decrements the time remaining on the IP lease assigned to the Client. When the renewal time has elapsed the DHCP Client state is updated to the RENEW state where the Client will request a renewal from the DHCP Server.

## Sending DHCP Messages To The Server

The DHCP Client has API services that allow the host application to send a message to the DHCP Server. Note these services are NOT intended for the host application to manually run the DHCP Client protocol.

- *nx_dhcp_release*: this sends a RELEASE message to the Server when the host application is either leaving the network or needs relinquish its IP address.
- *nx_dhcp_decline*: this sends a DECLINE message to the Server if the host application determines independently of the DHCP Client that its IP address is already in use.
- *nx_dhcp_forcerenew*: this sends a FORCERENEW message to the Server
- *nx_dhcp_send_request*: This takes as an argument a DHCP message type, as specified in *nxd_dhcp_client.h*, and sends the message to the Server. This is intended primarily for sending the DHCP INFORM message.

See [Description of DHCP Services](chapter3) for more information about these services 

## Starting and Stopping the DHCP Client

To stop the DHCP Client, regardless if it has achieved a bound state, the host application calls *nx_dhcp_stop*.

To restart a DHCP Client, the host application must first stop the DHCP Client using the *nx_dhcp_stop* service described above. Then the host can call *nx_dhcp_start* to resume the DHCP Client. If the host application wishes to clear a previous DHCP Client profile, for example, one obtained from a previous DHCP Server on another network, the host application should call *nx_dhcp_reinitialize* to perform this task internally before calling *nx_dhcp_start*.

A typical sequence might be:

```c
nx_dhcp_stop(&my_dhcp);
nx_dhcp_reinitialize(&my_dhcp);
nx_dhcp_start(&my_dhcp);
```

For DHCP applications running on only a single DHCP interface, stopping the DHCP Client also inactivates the DHCP CLIENT timer. Thus it is no longer keeping track of the time remaining on the IP lease. Stopping DHCP Client on a particular interface will not inactivate the DHCP Client timer but will stop timer updates to the time remaining on the IP lease on that interface

Therefore, stopping the DHCP Client is not advised unless the host application requires rebooting or switching networks.

## Using the DHCP Client with Auto IP

The NetX Duo DHCP Client works concurrently with the Auto IP protocol in applications where DHCP and Auto IP guarantee an address where a DHCP Server is not guaranteed to be available or responding. However, If the host is unable to detect a Server or get an IP address assigned, it can switch to the Auto IP protocol for a local IP address. However before doing so, it is advisable to stop the DHCP Client temporarily while Auto IP goes through the "probe" and "defense" stages. Once an Auto IP address is assigned to the host, the DHCP Client can be restarted and if a DHCP Server does become available, the host IP address can accept the IP address offered by the DHCP Server while the application is running.

The NetX Duo Auto IP has an address change notification for the host to monitor its activities in the event of an IP address change.

## Packet Chaining

For more efficient use of packet pool and memory resources, the DHCP Client can handle incoming chained packets (datagrams exceeding the driver MTU) from the Ethernet driver. If the driver has this capability, the application can set the packet pool for receiving packets to below the mandatory NX_DHCP_PACKET_PAYLOAD bytes. NX_DHCP_PACKET_PAYLOAD should accommodate the physical network (typically Ethernet) frame, plus 548 bytes of DHCP message data, and IP and UDP.

Note that the application can optimize the packet payload and number of packets in the packet pool that is part of the DHCP Client, and which is used for sending DHCP messages out. It can optimize the size based on expected usage and size of the DHCP Client messages.

## Small Example System

An example of how to use NetX Duo is shown below. The application thread entry function "*my_thread_entry*" is created at line 101. After successful creation, DHCP processing is initiated with the *nx_dhcp_start* call at line 108. At this point, the DHCP Client thread task separately attempts to contact a DHCP server. During this process, the application code waits for a valid IP address to be registered with the IP instance using the *nx_ip_status_check* service (or *nx_ip_interface_status_check* for a secondary interface) on line 95. This is more commonly done in a loop with a shorter wait option.

After line 127, DHCP has received a valid IP address and the application can then proceed, utilizing NetX Duo TCP/IP services as desired.

```c
#include   "tx_api.h"
#include   "nx_api.h"
#include   "nxd_dhcp_client.h"

#define     DEMO_STACK_SIZE         4096
TX_THREAD               my_thread;
NX_PACKET_POOL          my_pool;
NX_IP                   my_ip;
NX_DHCP                 my_dhcp;

/* Define function prototypes.  */

void    my_thread_entry(ULONG thread_input);
void    my_netx_driver(struct NX_IP_DRIVER_STRUCT *driver_req);

/* Define main entry point.  */
intmain()
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

    /* Create "my_thread".  */
 	  tx_thread_create(&my_thread, "my thread", my_thread_entry, 0,  
		                pointer, DEMO_STACK_SIZE, 2, 2, 
                        TX_NO_TIME_SLICE, TX_AUTO_START);
    pointer =  pointer + DEMO_STACK_SIZE;

    /* Initialize the NetX Duo system.  */
    nx_system_initialize();

    /* Create a packet pool.  */
    status =  nx_packet_pool_create(&my_pool, "NetX Main Packet Pool", 
                                     1024, pointer, 64000);
    pointer = pointer + 64000;

    /* Check for pool creation error.  */
    if (status)
        error_counter++;

    /* Create an IP instance without an IP address. */
    status = nx_ip_create(&my_ip, "My NetX IP Instance", IP_ADDRESS(0,0,0,0), 
                          0xFFFFFF00, &my_pool, my_netx_driver, pointer, 
                          DEMO_STACK_SIZE, 1);
    pointer =  pointer + DEMO_STACK_SIZE;

    /* Check for IP create errors.  */
    if (status)
        error_counter++;

    /* Enable ARP and supply ARP cache memory for my IP Instance.  */
    status =  nx_arp_enable(&my_ip, (void *) pointer, 1024);
    pointer = pointer + 1024;

    /* Check for ARP enable errors.  */
    if (status)
        error_counter++;

    /* Enable UDP.  */
    status =  nx_udp_enable(&my_ip);
    if (status)
        error_counter++;
 }


 /* Define my thread.  */

 void    my_thread_entry(ULONG thread_input)
 {

 UINT        status;
 ULONG       actual_status;
 NX_PACKET   *my_packet;

    /* Wait for the link to come up.  */
    do
    {
    /* Get the link status.  */
        status =  nx_ip_status_check(&my_ip, NX_IP_LINK_ENABLED, 
                                     &actual_status, 100);
    } while (status != NX_SUCCESS);

    /* Create a DHCP instance.  */
    status =  nx_dhcp_create(&my_dhcp, &my_ip, "My DHCP");

    /* Check for DHCP create error.  */
    if (status)
        error_counter++;

    /* Start DHCP.  */
    nx_dhcp_start(&my_dhcp);

    /* Check for DHCP start error.  */
    if (status)
        error_counter++;

    /* Wait for IP address to be resolved through DHCP.  */
    nx_ip_status_check(&my_ip, NX_IP_ADDRESS_RESOLVED, 
                       (ULONG *) &status, 100000);

    /* Check to see if we have a valid IP address.  */
    if (status)
    {
      error_counter++;
      return;
    }
    else
    {

  /* Yes, a valid IP address is now on lease…  All NetX Duo
        services are available.
    }
 }

```
## Multi-Server Environments

On networks where there is more than one DHCP Server, the DHCP Client accepts the first received DHCP Server Offer message, advances to the Request state, and ignores any other received offers.

## ARP Probes

The DHCP Client can be configured to send one or more ARP probes after IP address assignment from the DHCP Server to verify the IP address is not already in use. The ARP probe step is recommended by RFC 2131 and is particularly important in environments with more than one DHCP Server. If the host application enables the NX_DHCP_CLIENT_SEND_ARP_PROBE option (see **Configuration Options** in Chapter Two for additional ARP probe options), the DHCP Client will send a 'self addressed' ARP probe and wait for the specified time for a response. If none is received, the DHCP Client advances to the Bound state. If a response is received, the DHCP Client assumes the address is already in use. It automatically sends a DECLINE message to the Server, and reinitializes the Client to restart the DHCP probes again from the INIT state. This restarts the DHCP state machine and the Client sends another DISCOVER message to the Server.

## BOOTP Protocol

The DHCP Client also supports the BOOTP protocol as well the DHCP protocol. To enable this option and use BOOTP instead of DHCP, the host application must set the NX_DHCP_BOOTP_ENABLE configuration option. The host application can still request specific IP addresses in the BOOTP protocol. However, the DHCP Client does not support loading the host operating system as BOOTP is sometimes used to do.

## DHCP on a Secondary Interface

The NetX Duo DHCP Client can run on secondary interfaces rather than the default primary interface.

To run NetX Duo DHCP Client on a secondary network interface, the host application must set the interface index of the DHCP Client to the secondary interface using the *nx_dhcp_set_interface_index* API service. The interface must already be attached to the primary network interface using the *nx_ip_interface_attach* service. See the NetX Duo User Guide for more details on attaching secondary interfaces.

Below is an example system (Figure 1.2) in which the host application connects to the DHCP server on its secondary interface. On line 65, the secondary interface is attached to the IP task with a null IP address. On line 104, after the DHCP Client instance is created, the DHCP Client interface index is set to 1 (e.g. the offset from the primary interface which itself is index 0) by calling *nx_dhcp_set_interface_index*. Then the DHCP Client is ready to be started in line 108.

```c
#include   "tx_api.h"
#include   "nx_api.h"
#include   "nxd_dhcp_client.h"

#define     DEMO_STACK_SIZE         4096
TX_THREAD               my_thread;
NX_PACKET_POOL          my_pool;
NX_IP                   my_ip;
NX_DHCP                 my_dhcp;

/* Define function prototypes.  */

void    my_thread_entry(ULONG thread_input);
void    my_netx_driver(struct NX_IP_DRIVER_STRUCT *driver_req);

/* Define main entry point.  */

intmain()
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

    /* Create "my_thread".  */
    tx_thread_create(&my_thread, "my thread", my_thread_entry, 0,  
                      pointer, DEMO_STACK_SIZE, 
                      2, 2, TX_NO_TIME_SLICE, TX_AUTO_START);
    pointer =  pointer + DEMO_STACK_SIZE;

    /* Initialize the NetX Duo system.  */
    nx_system_initialize();

  /* Create a packet pool.  */
    status =  nx_packet_pool_create(&my_pool, "NetX Main Packet Pool", 
                                     1024, pointer, 64000);
    pointer = pointer + 64000;

    /* Check for pool creation error.  */
    if (status)
        error_counter++;

    /* Create an IP instance without an IP address. */
    status = nx_ip_create(&my_ip, "My NetX IP Instance", IP_ADDRESS(0,0,0,0), 
                           0xFFFFFF00, &my_pool, my_netx_driver, pointer, STACK_SIZE, 1);
    pointer =  pointer + DEMO_STACK_SIZE;

    /* Check for IP create errors.  */
    if (status)
        error_counter++;

    status =  _nx_ip_interface_attach(&ip_0, "port_2", IP_ADDRESS(0, 0, 0,0), 
                                       0xFFFFFF00UL, my_netx_driver);

    /* Enable ARP and supply ARP cache memory for my IP Instance.  */
    status =  nx_arp_enable(&my_ip, (void *) pointer, 1024);
    pointer = pointer + 1024;

    /* Check for ARP enable errors.  */
    if (status)
        error_counter++;

    /* Enable UDP.  */
    status =  nx_udp_enable(&my_ip);
    if (status)
        error_counter++;
}


void    my_thread_entry(ULONG thread_input)
{

UINT        status;
ULONG       status;
NX_PACKET   *my_packet;

    /* Wait for the link to come up.  */
    do
    {
      /* Get the link status.  */
        status =  nx_ip_status_check(&my_ip,NX_IP_LINK_ENABLED,& status,100);
    } while (status != NX_SUCCESS);

    /* Create a DHCP instance.  */
    status =  nx_dhcp_create(&my_dhcp, &my_ip, "My DHCP");

    /* Check for DHCP create error.  */
    if (status)
        error_counter++;

    /* Set the DHCP client interface to the secondary interface. */
    status = nx_dhcp_set_interface_index(&my_dhcp, 1);


    /* Start DHCP.  */
    nx_dhcp_start(&my_dhcp);

    /* Check for DHCP start error.  */
    if (status)
        error_counter++;

    /* Wait for IP address to be resolved through DHCP.  */
    nx_ip_status_check(&my_ip, NX_IP_ADDRESS_RESOLVED, 
                       (ULONG *) &status, 100000);

    /* Check to see if we have a valid IP address.  */
    if (status)
    {
        error_counter++;
        return;
    }
    else
    {
    /* Yes, a valid IP address is now on lease…  All NetX Duo
        services are available.*/
    }
}
```

## DHCP Client on Multiple Interfaces Simultaneously

To run DHCP Client on multiple interfaces, NX_MAX_PHYSICAL_INTERFACES in *nx_api.h* must be set to the number of physical interfaces connected to the device. By default, this value is 1 (e.g. the primary interface). To register an additional interface to the IP instance use the *nx_ip_interface_attach* service. See the NetX Duo User Guide for more details on attaching secondary interfaces.

The next step is to set the NX_DHCP_CLIENT_MAX_RECORDS in *nxd_dhcp_client.h* to the maximum number of interfaces expected to run DHCP simultaneously. Note that NX_DHCP_CLIENT_MAX_RECORDS does not have to equal NX_MAX_PHYSICAL_INTERFACES. For example, NX_MAX_PHYSICAL_INTERFACES can be 3 and NX_DHCP_CLIENT_MAX_RECORDS equal to 2. In this configuration, only two interfaces (and they can be any two of the three physical interfaces at any time) of the three physical interfaces can run DHCP at any one time. DHCP Client Records do not have a one to one mapping to network interfaces e.g. Client Record 1 does not automatically correlate to physical interface index 1.

NX_DHCP_CLIENT_MAX_RECORDS can also be set to greater than NX_MAX_PHYSICAL_INTERFACES but this would create unused client records and be an inefficient use of memory.

Before it can start DHCP on any interface, the application must enable those interfaces by calling *nx_dhcp_interface_enable*. Note that the exception is the primary interface which is automatically enabled in the *nx_dhcp_create* call (and which can be disabled using the *nx_dhcp_interface_disable* service discussed below).

At any time, an interface can be disabled for DHCP or DHCP can be stopped on that interface independently of other interfaces running DHCP.

As mentioned above, to enable a specific interface for DHCP, use the *nx_dhcp_interface_enable* service and specify the physical interface index in the input argument. Up to NX_DHCP_CLIENT_MAX_RECORDS interfaces can be enabled with the only limitation that the interface index input argument be less than NX_MAX_PHYSICAL_INTERFACES.

To start DHCP on a specific interface, use the *nx_dhcp_interface_start* service. To start DHCP on all enabled interfaces, use the *nx_dhcp_start* service. (Interfaces that have already started DHCP will not be affected by *nx_dhcp_start*.)

To stop DHCP on an interface, use the *nx_dhcp_interface_stop* service. DHCP must already have started on that interface or an error status is returned. To stop DHCP on all enabled interfaces, use the *nx_dhcp_stop* service. DHCP can be stopped independently of other interfaces at any time.

Most of the existing DHCP Client services have an 'interface' equivalent e.g. *nx_dhcp_interface_release* is the interface specific equivalent of *nx_dhcp_release.* If DHCP Client is configured for a single interface, they perform the same action.

Note that non-interface specific DHCP Client services typically apply to all interfaces but not all. In the latter case, the non-interface specific service applies to the first DHCP enabled interface found in searching the DHCP Client list of interface records. See **Description of Services** in Chapter Three for how a non-interface specific service performs when multiple interfaces are enabled for DHCP.

In the example sequence below, the IP instance has two network interfaces and first runs DHCP on the secondary interface. At some time later, it starts DHCP on the primary interface. Then it releases the IP address on the primary interface and restarts DHCP on the primary interface:

```c
nx_dhcp_create(&my_dhcp_client); /* By default this enables primary interface for DHCP. */

nx_dhcp_interface_enable(&my_dhcp_client, 1); /* Secondary interface is enabled. */

nx_dhcp_interface_start(&my_dhcp_client, 1); /* DHCP is started on secondary interface. */

/* Some time later… */

nx_dhcp_interface_start(&my_dhcp_client, 0); /* DHCP is started on primary interface. */

nx_dhcp_interface_release(&my_dhcp_client, 0); /* Some time later… */

nx_dhcp_interface_start(&my_dhcp_client, 0); /* DHCP is restarted on primary interface. */
```

For a complete list of interface specific services see **Description of Services** in Chapter Three.

## Configuration Options

User configurable DHCP options in *nxd_dhcp_client.h* allow the host application to fine tune DHCP Client for its particular requirements. The following is a list of these parameters:  
  
- **NX_DHCP_ENABLE_BOOTP**: Defined, this option enables the BOOTP protocol instead of DHCP. By default this option is disabled.
- **NX_DHCP_CLIENT_RESTORE_STATE**: If defined, this enables the DHCP Client to save its current DHCP Client license 'state' including time remaining on the lease, and restore this state between DHCP Client application reboots. The default value is disabled.
- **NX_DHCP_CLIENT_USER_CREATE_PACKET_POOL**: If set, the DHCP Client will not create its own packet pool. The host application must use the *nx_dhcp_packet_pool_set* service to set the DHCP Client packet pool. The default value is disabled.
- **NX_DHCP_CLIENT_SEND_ARP_PROBE**: Defined, this enables the DHCP Client to send an ARP probe after IP address assignment to verify the assigned DHCP address is not owned by another host. By default, this option is disabled.
- **NX_DHCP_ARP_PROBE_WAIT**: Defines the length of time the DHCP Client waits for a response after sending an ARP probe. The default value is one second (1 * NX_IP_PERIODIC_RATE)
- **NX_DHCP_ARP_PROBE_MIN**: Defines the minimum variation in the interval between sending ARP probes. The value is defaulted to 1 second.
- **NX_DHCP_ARP_PROBE_MAX**: Defines the maximum variation in the interval between sending ARP probes. The value is defaulted to 2 seconds.
- **NX_DHCP_ARP_PROBE_NUM**: Defines the number of ARP probes sent for determining if the IP address assigned by the DHCP server is already in use. The value is defaulted to 3 probes.
- **NX_DHCP_RESTART_WAIT**: Defines the length of time the DHCP Client waits to restart DHCP if the IP address assigned to the DHCP Client is already in use. The value is defaulted to 10 seconds.
- **NX_DHCP_CLIENT_MAX_RECORDS**: Specifies the maximum number of interface records to save to the DHCP Client instance. A DHCP Client interface record is a record of the DHCP Client running on a specific interface. The default value is set as physical interfaces count(NX_MAX_PHYSICAL_INTERFACES).
- **NX_DHCP_CLIENT_SEND_MAX_DHCP_MESSAGE_OPTION**: Defined, this enables the DHCP Client to send maximum DHCP message size option. By default, this option is disabled.
- **NX_DHCP_CLIENT_ENABLE_HOST_NAME_CHECK**: Defined, this enables the DHCP Client to check the input host name in the nx_dhcp_create call for invalid characters or length. By default, this option is disabled.
- **NX_DHCP_THREAD_PRIORITY**: Priority of the DHCP thread. By default, this value specifies that the DHCP thread runs at priority 3.
- **NX_DHCP_THREAD_STACK_SIZE**: Size of the DHCP thread stack. By default, the size is 2048 bytes.
- **NX_DHCP_TIME_INTERVAL**: Interval in seconds when the DHCP Client timer expiration function executes. This function updates all the timeouts in the DHCP process e.g. if messages should be retransmitted or DHCP Client state changed. By default, this value is 1 second.
- **NX_DHCP_OPTIONS_BUFFER_SIZE**: Size of DHCP options buffer. By default, this value is 312 bytes.
- **NX_DHCP_PACKET_PAYLOAD**: Specifies the size in bytes of the DHCP Client packet payload. The default value is NX_DHCP_MINIMUM_IP_DATAGRAM + physical header size. The physical header size in a wireline network is usually the Ethernet frame size.
- **NX_DHCP_PACKET_POOL_SIZE**: Specifies the size of the DHCP Client packet pool. The default value is (5 *NX_DHCP_PACKET_PAYLOAD) which will provide four packets plus room for internal packet pool overhead.
- **NX_DHCP_MIN_RETRANS_TIMEOUT**: Specifies the minimum wait option for receiving a DHCP Server reply to client message before retransmitting the message. The default value is the RFC 2131 recommended 4 seconds.
- **NX_DHCP_MAX_RETRANS_TIMEOUT**: Specifies the maximum wait option for receiving a DHCP Server reply to client message before retransmitting the message. The default value is 64 seconds.
- **NX_DHCP_MIN_RENEW_TIMEOUT**: Specifies minimum wait option for receiving a DHCP Server message and sending a renewal request after the DHCP Client is bound to an IP address. The default value is 60 seconds. However, the DHCP Client uses the Renew and Rebind expiration times from the DHCP server message before defaulting to the minimum renew timeout.
- **NX_DHCP_TYPE_OF_SERVICE**: Type of service required for the DHCP UDP requests. By default, this value is defined as NX_IP_NORMAL to indicate normal IP packet service.
- **NX_DHCP_FRAGMENT_OPTION**: Fragment enable for DHCP UDP requests. By default, this value is NX_DONT_FRAGMENT to disable DHCP UDP fragmenting.
- **NX_DHCP_TIME_TO_LIVE**: Specifies the number of routers this packet can pass before it is discarded. The default value is set to 0x80.
- **NX_DHCP_QUEUE_DEPTH**: Specifies the number of maximum depth of receive queue. The default value is set to 4.