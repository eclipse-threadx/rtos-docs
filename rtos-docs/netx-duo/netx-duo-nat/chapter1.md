---
title: Chapter 1 - An introduction to Network Address Translation
description: IP Network Address Translation (NAT) was originally developed to solve the problem of a limited number of Internet IPv4 addresses.
---

# Chapter 1 - An introduction to Network Address Translation

## The Need for Network Address Translation

IP Network Address Translation (NAT) was originally developed to solve the problem of a limited number of Internet IPv4 addresses. The need for NAT arises when multiple devices need to access the Internet but only one IPv4 Internet address is assigned by the Internet Service Provider (ISP).

There are other benefits of using NAT as well. Network topology outside the local domain can change in many ways. Customers may change providers, company backbones may be reorganized, or providers may merge or split. Whenever the external topology changes, address assignments for hosts within the local domain must also change to reflect these external changes. Changes of this type can be hidden from users within the domain by centralizing changes to a single address translation router. NAT enables access for local hosts to the public Internet and protects them from direct access from the outside. Organizations with a network setup predominantly for internal use, with a need for occasional external access are good candidates for this scheme.

## Basic NAT and Network Address Port Translation

A NAT-enabled router is installed between the public network and the private network. The role of the NAT-enabled router is to translate between the internal private IPv4 addresses and the assigned public IPv4 address, so all the devices on the private network are able to share the same public IPv4 address.

In the basic implementation of NAT, the NAT router 'owns' one or more globally registered IP addresses different from its own IP address. These global addresses are available to assign to hosts on its private network either statically or dynamically. NAPT, or Network Address Port Translation, is a variation of basic NAT, where network address translation is extended to include a 'transport' identifier. Most typically this is the port number for TCP and UDP packets, and the Query ID for ICMP packets.

Connections across the NAT boundary are typically initiated by hosts on the private network sending outbound packets to an external host. These hosts are usually assigned *dynamic* (temporary) IP addresses for this purpose. However, it is also possible to have connections initiated in the opposite direction if the private network has 'servers' e.g. HTTP or FTP servers that will accept Client requests from the external network. NAT will typically assign these local hosts a *static* (permanent) IP address:port.

## How Network Address Translation Works

A typical network setup with a NAT-enabled router is illustrated in Figure 1.

![A typical network setup with a NAT-enabled router](media/image2.png)

**Figure 1 - A typical network setup with a NAT-enabled router**

A NAT-enabled router typically has two network interfaces. One interface is connected to the public Internet; the other is connected to the private network. A typical router in this setup is responsible for routing IP datagrams between the private network and the public network based on destination IP address. A NAT-enabled router performs address translation before routing an IPv4 datagram between the public and the private interface. A translation is established for each TCP or UDP session, based the internal source address, source port number, and external destination address and destination port number. For ICMP echo request and response datagram, the ICMP query ID is used instead of the port number.

To illustrate a typical implementation of Network Address Translation, let us consider a network setup in Figure 2.

![A typical implementation of Network Address Translation](media/image3.png)

**Figure 2 - A typical implementation of Network Address Translation**

In this scenario, the NAT router connects the private network to the left, and the public network to the right. Let's assume on the public network side, the NAT router interface IP address is 202.151.25.14; on the private network interface, the NAT router uses the IP address 192.168.1.254. A node on the private network initiates a TCP connection with a web server on the Internet.

Figure 3 shows a high-level view of the Network Address Translation process.

![A high-level view of the Network Address Translation process](media/image4.png)

**Figure 3 - A high-level view of the Network Address Translation process**

1. Client transmits a TCP SYN message to the web server. The sender address is 192.168.1.15, port number 6732; the destination address is 128.15.54.3, port number 80.
1. The packet from the Client is received on the private network interface by the NAT router. The outbound traffic rule applies to the packet: the sender's (Client's) address is translated to the NAT router's public IP address 202.15.25.14, and sender (Client) source port number is translated to the TCP port number 2015 on the public interface.
1. The packet is then transmitted over the Internet and ultimately reaches its destination host 128.15.54.3. Notice that on the receiving side, based on the IP layer source address and TCP layer port number, the packet appears to have originated from 202.151.24.14, port number 2015.
   Figure 4 shows the NAT process on the return path.

   ![NAT process on the return path](media/image5.png)

   **Figure 4 - NAT process on the return path**
1. In this scenario, the Internet host 128.15.54.3 sends a response packet with the NAT router's Internet address as its destination.
1. The packet reaches the NAT router. Since this is an in-bound packet, the in-bound translation rules apply: the destination address is changed back to the original sender's (Client's) IP address: 192.168.1.15, destination port number 6732.
1. The packet is then forwarded to the Client through the interface that is connected to the internal network.

In this manner the internet network address and port number of the sender is not exposed to other hosts on the public Internet.

## NetX Duo NAT Features

When the NAT instance is created using *nx_nat_create* call, the NAT translation table is created.

```C
UINT nx_nat_create(NX_NAT_DEVICE *nat_ptr, NX_IP *ip_ptr,
    UINT global_interface_index,
    VOID *dynamic_cache_memory,
    UINT dynamic_cache_size);
```

To keep track of the network address translations for all active connections between local and external networks, the NetX Duo NAT-enabled router maintains a translation table with information about each private host connection which includes source and destination IP address and port number.

The location of this translation table ("cache") is set with the dynamic_cache_memory pointer. This area must be a 4 byte aligned buffer space. The size of the table (or number of entries) is determined by dividing the cache size dynamic_cache_size by the size of a NAT table entry. The table must be large enough for the minimal number of entries specified by **NX_NAT_MIN_ENTRY_COUNT which is defined in *nx_nat.h*. The default value is 3.**

The timeout for all dynamic entries in the NetX Duo NAT translation table are initialized to NX_NAT_ENTRY_RESPONSE_TIMEOUT which is defined in *nx_nat.h*. The default value is 4 minutes (or 240 system ticks for a 100 mHz processor) as recommended by RFC 2663. Each time NetX Duo NAT receives or sends a packet matching a dynamic entry in the table it resets that entry's time out to NX_NAT_ENTRY_RESPONSE_TIMEOUT. When searching the table, NetX Duo NAT will also check the table for expired entries and delete them.

To create inbound entries as static in the table e.g. for servers on the local network, NetX Duo NAT provides the *nx_nat_inbound_entry_create* service. If a table entry defines the local host connection as static, it never expires.

```C
UINT nx_nat_inbound_entry_create(NX_NAT_DEVICE *nat_ptr,
    NX_NAT_TRANSLATION_ENTRY *entry_ptr,
    ULONG local_ip_address, USHORT external_port,
    USHORT local_port, UCHAR protocol);
```

This service is described in more detail in [Chapter 4 - Description of Services](chapter4.md)

During runtime, if the translation table is full and no more entries can be added, NetX Duo NAT will notify the NAT application with a cache full callback if one is registered with the NAT instance. This is done using the *nx_nat_cache_notify_set* service:

```C
UINT nx_nat_cache_notify_set(NX_NAT_DEVICE *nat_ptr,
    VOID (*cache_full_notify_cb)(NX_NAT_DEVICE *nat_ptr));
```

See [Chapter 4 - Description of Services](chapter4.md) for more details about this service.

## NAT Packet Processing in NetX Duo

NetX Duo NAT is intended for use on an IPv4 router. For NAT to work, NetX Duo must be configured for forwarding packets to the NAT server. See Chapter 2 on NetX Duo NAT installation for how to do so. The NAT server then indicates if it will 'consume' (attempt to forward) the packet to a host on either of its networks. If it will not consume the packet, the packet is 'returned' to NetX Duo to process the packet as it normally would.

When the NAT server receives a packet to forward from NetX Duo, it determines if the packet is inbound or outbound.

For outbound packets, the NAT server checks the packet IP header source address and port. If the translation table does not contain an entry for a packet previously sent by this host for the same destination, NAT will create a new entry which will contain a unique global source IP address:port for the connection, and modify the packet headers with this new IP address:port before sending it onto the external network.

For inbound packets, the NAT server looks for a previous entry in its translation table with an external IP address: port matching the packet destination IP address: port. If no match is found, it will discard the packet unless the destination address: port is the external address for server on the local network. If it does find a match, it will replace the packet header's external destination IP address: port with the private IP address: port and send the packet onto the local network to the intended private host.

NetX Duo NAT uses a range of TCP, UDP and ICMP translation ports for creating unique local address: port connections for local hosts connecting with outside hosts. The following user configurable options, defined in *nx_nat.h,* define the range for each protocol:

```C
NX_NAT_START_TCP_PORT

NX_NAT_END_TCP_PORT

NX_NAT_START_UDP_PORT

NX_NAT_END_UDP_PORT

NX_NAT_START_ICMP_QUERY_ID

NX_NAT_END_ICMP_QUERY_ID
```

## NAT Requirements and Constraints

NetX Duo NAT requires NetX Duo 5.8 or later. The NAT application requires creation of a single IP instance and an interface to the internal and external physical network.

Constraints:

- NetX Duo NAT supports TCP, UDP and ICMP. IGMP is not supported.
- NetX Duo NAT does not support IPv6 addressing.
- NetX Duo NAT does not include DNS or DHCP services, although NetX Duo NAT can integrate those services with its NAT operations.

## RFCs Supported by NetX Duo NAT

NetX Duo NAT implementation is based on information presented in the following RFCs:

- RFC 2663: IP Network Address Translator (NAT) Terminology and Considerations
- RFC 3022: Traditional IP Network Address Translator (Traditional NAT)
- RFC 4787: Network Address Translation (NAT) Behavioral Requirements for Unicast UDP
