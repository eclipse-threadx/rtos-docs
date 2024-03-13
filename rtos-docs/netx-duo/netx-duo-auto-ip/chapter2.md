---
title: Chapter 2 - Installation and use of NetX Duo AutoIP
description: This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo AutoIP component.
---

# Chapter 2 - Installation and use of NetX Duo AutoIP

This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo AutoIP component.

## Product Distribution

AutoIP can be obtained from our public source code repository at [https://github.com/eclipse-threadx/netxduo/](https://github.com/eclipse-threadx/netxduo/). The package includes three source files, one include files, and a PDF file that contains this document, as follows:

- **nx_auto_ip.h**: Header file for NetX Duo AutoIP
- **nx_auto_ip.c**: C Source file for NetX Duo AutoIP
- **demo_netx_auto_ip.c**: C Source file for NetX Duo AutoIP Demo
- **nx_auto_ip.pdf**: PDF description of NetX Duo AutoIP

## AutoIP Installation

In order to use NetX Duo AutoIP, the entire distribution mentioned previously should be copied to the same directory where NetX Duo is installed. For example, if NetX Duo is installed in the directory "*\threadx\arm7\green*" then the *nx_auto_ip.h*, *nx_auto_ip.c*, and *demo_netx_auto_ip.c* files should be copied into this directory.

## Using AutoIP

Using NetX Duo AutoIP is easy. Basically, the application code must include *nx_auto_ip.h* after it includes *tx_api.h* and *nx_api.h*, in order to use ThreadX and NetX Duo. Once *nx_auto_ip.h* is included, the application code is then able to make the AutoIP function calls specified later in this guide. The application must also include *nx_auto_ip.c* in the build process. These files must be compiled in the same manner as other application files and its object form must be linked along with the files of the application. This is all that is required to use NetX Duo AutoIP.

> **Note:** Since AutoIP utilizes NetX Duo ARP services, ARP must be enabled with the *nx_arp_enable* call prior to using AutoIP.

## Small Example System

An example of how easy it is to use NetX Duo AutoIP is described below. In this example, the AutoIP include file *nx_auto_ip.h* is brought in at line 002. Next, the NetX Duo AutoIP instance is created in "*tx_application_define*" at line 090. Note that the NetX Duo AutoIP control block "auto_ip_0" was defined previously as a global variable at line 015. After successful creation, an NetX Duo AutoIP is started at line 098. The IP address change callback function processing starts at line 105, which is used to handle subsequent conflicts or possible DHCP address resolution.

> **Note:** The example below assumes the host device is a single-homed device. For a multihomed device, the host application can use the NetX Duo AutoIP service *nx_auto_ip_interface_*set to specify a secondary network interface to probe for an IP address. See the [NetX Duo User Guide](../about-this-guide.md) for more details on setting up multihomed applications. Note further that the host application should use the NetX Duo API *nx_status_ip_interface_check* to verify AutoIP has obtained an IP address.

```c
#include "tx_api.h"
#include "nx_api.h"
#include "nx_auto_ip.h"

#define     DEMO_STACK_SIZE     4096

/* Define the ThreadX and NetX object control blocks... */

TX_THREAD              thread_0;
NX_PACKET_POOL         pool_0;
NX_IP                  ip_0;

/* Define the AUTO IP structures for the IP instance. */

NX_AUTO_IP             auto_ip_0;

/* Define the counters used in the demo application... */

ULONG                 thread_0_counter;
ULONG                 address_changes;
ULONG                 error_counter;

/* Define thread prototypes. */
void     thread_0_entry(ULONG thread_input);
void     ip_address_changed(NX_IP *ip_ptr, VOID *auto_ip_address);
void     _nx_ram_network_driver(struct NX_IP_DRIVER_STRUCT *driver_req);

/* Define main entry point. */

int main()
{

    /* Enter the ThreadX kernel. */
    tx_kernel_enter();
}

/* Define what the initial system looks like. */

void     tx_application_define(void *first_unused_memory)
{

CHAR     *pointer;
UINT     status;

    /* Setup the working pointer. */
    pointer = (CHAR *) first_unused_memory;

    /* Create the main thread. */
    tx_thread_create(&thread_0, "thread 0", thread_0_entry, 0,
                    pointer, DEMO_STACK_SIZE,
                    16, 16, 1, TX_AUTO_START);

    pointer = pointer + DEMO_STACK_SIZE;

    /* Initialize the NetX system. */
    nx_system_initialize();

    /* Create a packet pool. */
    status = nx_packet_pool_create(&pool_0, "NetX Main Packet Pool", 128,
                                    pointer, 4096);
    pointer = pointer + 4096;

    if (status)
        error_counter++;

    /* Create an IP instance. */
    status = nx_ip_create(&ip_0, "NetX IP Instance 0", IP_ADDRESS(0, 0, 0, 0),
                        0xFFFFFF00UL, &pool_0, _nx_ram_network_driver,
                        pointer, 4096, 1);
    pointer = pointer + 4096;

    if (status)
        error_counter++;

    /* Enable ARP and supply ARP cache memory for IP Instance 0. */
    status = nx_arp_enable(&ip_0, (void *) pointer, 1024);
    pointer = pointer + 1024;

    /* Check ARP enable status. */
    if (status)
        error_counter++;

    /* Create the AutoIP instance for IP Instance 0. */
    status = nx_auto_ip_create(&auto_ip_0, "AutoIP 0", &ip_0, pointer, 4096, 1);
    pointer = pointer + 4096;

    /* Check AutoIP create status. */
    if (status)
        error_counter++;

    /* Start AutoIP instances. */
    status = **nx_auto_ip_start**(&auto_ip_0, 0 /*IP_ADDRESS(169,254,254,255)*/);

    /* Check AutoIP start status. */
    if (status)
        error_counter++;

    /* Register an IP address change function for IP Instance 0. */
    status = nx_ip_address_change_notify(&ip_0, ip_address_changed,
                                        (void *) &auto_ip_0);

    /* Check IP address change notify status. */
    if (status)
        error_counter++;
}

/* Define the test thread. */

void     thread_0_entry(ULONG thread_input)
{

UINT     status;
ULONG    actual_status;

    /* Wait for IP address to be resolved. */
    do
    {

        /* Call IP status check routine. */
        status = nx_ip_status_check(&ip_0, NX_IP_ADDRESS_RESOLVED,
            &actual_status, 10000);

    } while (status != NX_SUCCESS);

    /* Since the IP address is resolved at this point, the application can now fully utilize NetX! */

    while(1)
    {

        /* Increment thread 0's counter. */
        thread_0_counter++;

        /* Sleep... */
        tx_thread_sleep(10);
    }
}

void          ip_address_changed(NX_IP *ip_ptr, VOID *auto_ip_address)
{

ULONG         ip_address;
ULONG         network_mask;
NX_AUTO_IP    *auto_ip_ptr;

    /* Setup pointer to auto IP instance. */
    auto_ip_ptr = (NX_AUTO_IP *) auto_ip_address;

    /* Pickup the current IP address. */
    nx_ip_address_get(ip_ptr, &ip_address, &network_mask);

    /* Determine if the IP address has changed back to zero. If so, make sure the AutoIP instance is started. */
    if (ip_address == 0)
    {

        /* Get the last AutoIP address for this node. */
        nx_auto_ip_get_address(auto_ip_ptr, &ip_address);

        /* Start this AutoIP instance. */
        nx_auto_ip_start(auto_ip_ptr, ip_address);
        }

    /* Determine if IP address has transitioned to a non local IP address. */
    else if ((ip_address & 0xFFFF0000UL) != IP_ADDRESS(169, 254, 0, 0))
    {

        /* Stop the AutoIP processing. */
        nx_auto_ip_stop(auto_ip_ptr);
    }

    /* Increment a counter. */
    address_changes++;
}
```

## Configuration Options

There are several configuration options for building NetX Duo AutoIP. Following is a list of all options, where each is described in detail:

- **NX_DISABLE_ERROR_CHECKING**: Defined, this option removes the basic AutoIP error checking. It is typically used after the application has been debugged.
- **NX_AUTO_IP_PROBE_WAIT**: The number of seconds to wait before sending first probe. By default, this value is defined as 1.
- **NX_AUTO_IP_PROBE_NUM**: The number of ARP probes to send. By default, this value is defined as 3.
- **NX_AUTO_IP_PROBE_MIN**: The minimum number of seconds to wait between sending probes. By default, this value is defined as 1.
- **NX_AUTO_IP_PROBE_MAX**: The maximum number of seconds to wait between sending probes. By default, this value is defined as 2.
- **NX_AUTO_IP_MAX_CONFLICTS**: The number of AutoIP conflicts before increasing processing delays. By default, this value is defined as 10.
- **NX_AUTO_IP_RATE_LIMIT_INTERVAL**: The number of seconds to extend the wait period when the total number of conflicts is exceeded. By default, this value is defined as 60.
- **NX_AUTO_IP_ANNOUNCE_WAIT**: The number of seconds to wait before sending announcement. By default, this value is defined as 2.
- **NX_AUTO_IP_ANNOUNCE_NUM**: The number of ARP announces to send. By default, this value is defined as 2.
- **NX_AUTO_IP_ANNOUNCE_INTERVAL**: The number of seconds to wait between sending announces. By default, this value is defined as 2.
- **NX_AUTO_IP_DEFEND_INTERVAL**: The number of seconds to wait between defense announces. By default, this value is defined as 10.
