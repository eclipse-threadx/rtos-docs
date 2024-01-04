---
title: Chapter 1 - Introduction to NetX Duo AutoIP
description: The NetX Duo AutoIP Protocol is a protocol designed for dynamically configuring IPv4 addresses on a local network. In order to function properly, the NetX Duo AutoIP package requires that a NetX Duo IP instance has already been created.
---

# Chapter 1 - Introduction to NetX Duo AutoIP

The AutoIP Protocol is a protocol designed for dynamically configuring IPv4 addresses on a local network. AutoIP is a simple protocol that utilizes ARP capabilities to perform its automatic IP address assignment function. AutoIP allocates addresses in the range of 169.254.1.0 through 169.254.254.255.

## AutoIP Requirements

In order to function properly, the NetX Duo AutoIP package requires that a NetX Duo IP instance has already been created. In addition, ARP must be enabled on that same IP instance. The NetX Duo AutoIP package has no further requirements.

## AutoIP Constraints

The NetX Duo AutoIP protocol implements the requirements of the RFC3927 standard. However, there are the following constraints:

1. If NetX Duo DHCP is used, the DHCP thread must be created with a higher priority than both the NetX Duo IP instance thread and the AutoIP thread.

1. NetX Duo AutoIP does not provide a mechanism for old IP addresses to continue being used.

1. When the IP address changes, the application is responsible for tearing down any existing TCP connections and re-establishing them on the new IP address.

## AutoIP Protocol Implementation

The NetX Duo AutoIP protocol first selects a random address within the AutoIP IPv4 address range of 169.254.1.0 through 169.254.254.255. Alternatively, the application may force a starting IP address by providing it to the ***nx_auto_ip_start*** function. This is useful in situations where an AutoIP address was successfully used in a prior run.

Once an AutoIP address is selected, NetX Duo AutoIP sends out a series of ARP probes for the selected address. An ARP probe consists of an ARP request message with the sender address set to 0.0.0.0 and the target address set to the desired AutoIP address. A series of these ARP probes are sent (the actual number is determined by the define NX_AUTO_IP_PROBE_NUM). If another network node responds to this probe or sends an identical probe for the same address, a new AutoIP address is randomly selected within the AutoIP IPv4 address range and the probe processing repeats.

If NX_AUTO_IP_PROBE_NUM probes are sent without any responses, NetX Duo AutoIP issues a series of ARP announcements for the selected address. An ARP announcement consists of an ARP request message with both the sender and target address in the ARP message set to the selected AutoIP address. A series of ARP announcement messages are sent, corresponding to the define NX_AUTO_IP_ANNOUNCE_NUM. If another network node responds to an announce message or sends an identical announcement for the same address, a new AutoIP address is randomly selected within the AutoIP IPv4 address range and the probe processing starts over.

When the probe and announcement completes without any detected conflicts, the selected AutoIP address is considered valid and the associated IP instance is setup with this address.

## AutoIP Address Change

As mentioned before, NetX Duo AutoIP changes the IP instance address after successful probe and announcement processing. Monitoring for this case is not terribly important. However, it is possible to have the AutoIP address change in the future. Potential causes include future AutoIP address conflicts as well as DHCP address resolution. In order to process these potential situations properly, the application should use the following NetX Duo API to alert it of any and all IP address changes:

```c
nx_ip_address_change_notify(NX_IP *ip_ptr,
            VOID (*ip_address_change_notify)(NX_IP *,VOID*),
            VOID *additional_info);
```

The processing in the supplied *ip_address_change_notify* function must either restart the NetX Duo AutoIP processor or disable it if DHCP has subsequently resolved the IP address. Please refer to the *Small Example System* section for sample processing.

## AutoIP RFCs

NetX Duo AutoIP is compliant with RFC3927 and related RFCs.
