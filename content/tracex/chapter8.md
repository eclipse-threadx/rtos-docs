---
title: Chapter 8 - NetX Duo trace events
description: This chapter contains a description of the NetX Duo events. 
---


This chapter contains a description of the NetX Duo events. 

## List of Events and Icons

The following is a list of NetX Duo events displayed by TraceX.

| Icon                                           | Meaning                 |
| -------------------------------- | ------------------------------------- |
| ![Internal A R P Request Receive icon](../media/user-guide/netx-events/image1.png)    | Internal ARP Request Receive |
| ![Internal A R P Request Send icon](../media/user-guide/netx-events/image2.png)    | Internal ARP Request Send |
| ![Internal A R P Response Receive icon](../media/user-guide/netx-events/image3.png)    | Internal ARP Response Receive |
| ![Internal A R P Response Send icon](../media/user-guide/netx-events/image4.png)    | Internal ARP Response Send |
| ![Internal I C M P Receive icon](../media/user-guide/netx-events/image5.png)    | Internal ICMP Receive |
| ![Internal I C M P Send icon](../media/user-guide/netx-events/image6.png)    | Internal ICMP Send |
| ![Internal NetX Duo I G M P Receive icon](../media/user-guide/netx-events/image7.png)    | Internal NetX Duo IGMP Receive |
| ![Internal I P Receive icon](../media/user-guide/netx-events/image8.png)    | Internal IP Receive |
| ![Internal I P Send icon](../media/user-guide/netx-events/image9.png)    | Internal IP Send |
| ![Internal T C P Data Receive icon](../media/user-guide/netx-events/image10.png)    | Internal TCP Data Receive |
| ![Internal T C P Data Send icon](../media/user-guide/netx-events/image11.png)    | Internal TCP Data Send |
| ![Internal T C P FIN Receive icon](../media/user-guide/netx-events/image12.png)    | Internal TCP FIN Receive |
| ![Internal T C P F I N Send icon](../media/user-guide/netx-events/image13.png)    | Internal TCP FIN Send |
| ![Internal T C P R S T Receive icon](../media/user-guide/netx-events/image14.png)    | Internal TCP RST Receive |
| ![Internal T C P R S T Send icon](../media/user-guide/netx-events/image15.png)    | Internal TCP RST Send |
| ![Internal T C P S Y N Receive icon](../media/user-guide/netx-events/image16.png)    | Internal TCP SYN Receive |
| ![Internal T C P S Y N Send icon](../media/user-guide/netx-events/image17.png)    | Internal TCP SYN Send |
| ![Internal U D P Receive icon](../media/user-guide/netx-events/image18.png)    | Internal UDP Receive |
| ![Internal U D P Send icon](../media/user-guide/netx-events/image19.png)    | Internal UDP Send |
| ![Internal R A R P Receive icon](../media/user-guide/netx-events/image20.png)    | Internal RARP Receive |
| ![Internal R A R P Send icon](../media/user-guide/netx-events/image21.png)    | Internal RARP Send |
| ![Internal T C P Retry icon](../media/user-guide/netx-events/image22.png)    | Internal TCP Retry |
| ![Internal T C P State Change icon](../media/user-guide/netx-events/image23.png)    | Internal TCP State Change |
| ![Internal I / O Driver Packet Send icon](../media/user-guide/netx-events/image24.png)    | Internal I/O Driver Packet Send |
| ![icon](../media/user-guide/netx-events/image25.png)    | Internal I/O Driver Initialize |
| ![Internal I / O Driver Initialize icon](../media/user-guide/netx-events/image26.png)    | Internal I/O Driver Link Enable |
| ![Internal I / O Driver Link Disable icon](../media/user-guide/netx-events/image27.png)    | Internal I/O Driver Link Disable |
| ![Internal I / O Driver Packet Broadcast icon](../media/user-guide/netx-events/image28.png)    | Internal I/O Driver Packet Broadcast |
| ![Internal I / O Driver ARP Send icon](../media/user-guide/netx-events/image29.png)    | Internal I/O Driver ARP Send |
| ![Internal I / O Driver ARP Response Send icon](../media/user-guide/netx-events/image30.png)    | Internal I/O Driver ARP Response Send |
| ![Internal I / O Driver RARP Send icon](../media/user-guide/netx-events/image31.png)    | Internal I/O Driver RARP Send |
| ![Internal I / O Driver Multicast Join icon](../media/user-guide/netx-events/image32.png)    | Internal I/O Driver Multicast Join |
| ![Internal I / O Driver Multicast Leave icon](../media/user-guide/netx-events/image33.png)    | Internal I/O Driver Multicast Leave |
| ![Internal I / O Driver Get Status icon](../media/user-guide/netx-events/image34.png)    | Internal I/O Driver Get Status |
| ![Internal I / O Driver Get Speed icon](../media/user-guide/netx-events/image35.png)    | Internal I/O Driver Get Speed |
| ![Internal I / O Driver Get Duplex Type icon](../media/user-guide/netx-events/image36.png)    | Internal I/O Driver Get Duplex Type |
| ![Internal I / O Driver Get Error Count icon](../media/user-guide/netx-events/image37.png)    | Internal I/O Driver Get Error Count |
| ![Internal I / O Driver Get RX Count icon](../media/user-guide/netx-events/image38.png)    | Internal I/O Driver Get RX Count |
| ![Internal I / O Driver Get TX Count icon](../media/user-guide/netx-events/image39.png)    | Internal I/O Driver Get TX Count |
| ![Internal I / O Driver Get Allocation Errors icon](../media/user-guide/netx-events/image40.png)    | Internal I/O Driver Get Allocation Errors |
| ![Internal I / O Driver Un-initialize icon](../media/user-guide/netx-events/image41.png)    | Internal I/O Driver Un-initialize |
| ![Internal I / O Driver Deferred Processing icon](../media/user-guide/netx-events/image42.png)    | Internal I/O Driver Deferred Processing |
| ![A R P Dynamic Entries Invalidate icon](../media/user-guide/netx-events/image43.png)    | **ARP Dynamic Entries Invalidate** (*nx_arp_dynamic_entries_invalidate*) |
| ![A R P Dynamic Entry Set icon](../media/user-guide/netx-events/image44.png)    | **ARP Dynamic Entry Set** (*nx_arp_dynamic_entry_set*) |
| ![A R P Enable icon](../media/user-guide/netx-events/image45.png)    | **ARP Enable** (*nx_arp_enable*) |
| ![A R P Gratuitous Send icon](../media/user-guide/netx-events/image46.png)    | **ARP Gratuitous Send** (*nx_arp_gratuitous_send*) |
| ![A R P Hardware Address Find icon](../media/user-guide/netx-events/image47.png)    | **ARP Hardware Address Find** (*nx_arp_hardware_address_find*) |
| ![A R P Information Get icon](../media/user-guide/netx-events/image48.png)    | **ARP Information Get** (*nx_arp_info_get*) |
| ![A R P IP Address Find icon](../media/user-guide/netx-events/image49.png)    | **ARP IP Address Find** (*nx_arp_ip_address_find*) |
| ![A R P Static Entry Create icon](../media/user-guide/netx-events/image50.png)    | **ARP Static Entry Create** (*nx_arp_static_entry_create*) |
| ![A R P Static Entries Delete icon](../media/user-guide/netx-events/image51.png)    | **ARP Static Entries Delete** (*nx_arp_static_entries_delete*) |
| ![A R P Static Entry Delete icon](../media/user-guide/netx-events/image52.png)    | **ARP Static Entry Delete** (*nx_arp_static_entry_delete*) |
| ![Duo Cache Entry Delete icon](../media/user-guide/netx-events/image53.png)    | **Duo Cache Entry Delete** (*nxd_nd_cache_entry_delete*) |
| ![Duo Cache Entry Set icon](../media/user-guide/netx-events/image54.png)    | **Duo Cache Entry Set** (*nxd_nd_cache_entry_set*) |
| ![Duo Cache Invalidate icon](../media/user-guide/netx-events/image55.png)    | **Duo Cache Invalidate** (*nxd_nd_cache_invalidate*) |
| ![Duo Cache I P Address Find icon](../media/user-guide/netx-events/image56.png)    | **Duo Cache IP Address Find** (*nxd_nd_cache_ip_address_find*) |
| ![Duo I C M P Enable icon](../media/user-guide/netx-events/image57.png)    | **Duo ICMP Enable** (*nxd_icmp_enable*) |
| ![Duo I C M P I P v 6 Ping icon](../media/user-guide/netx-events/image58.png)    | **Duo ICMP IPv6 Ping** (*nxd_icmp_ping*) |
| ![Duo I P Max Payload Size Find icon](../media/user-guide/netx-events/image59.png)    | **Duo IP Max Payload Size Find** (*nx_max_payload_size_find*) |
| ![Duo I P Raw Packet Send icon](../media/user-guide/netx-events/image60.png)    | **Duo IP Raw Packet Send** (*nxd_ip_raw_packet_send*) |
| ![Duo I P v 6 Default Router Add icon](../media/user-guide/netx-events/image61.png)    | **Duo IPv6 Default Router Add** (*nxd_ipv6_default_router_add*) |
| ![Duo I P v 6 Default Router Delete icon](../media/user-guide/netx-events/image62.png)    | **Duo IPv6 Default Router Delete** (*nxd_ipv6_default_router_delete)* |
| ![Duo I P v 6 Enable icon](../media/user-guide/netx-events/image63.png)    | **Duo IPv6 Enable** (*nxd_ipv6_enable)* |
| ![Duo I P v 6 Global Address Get icon](../media/user-guide/netx-events/image64.png)    | **Duo IPv6 Global Address Get** (*nxd_ipv6_global_address_get*) |
| ![Duo I P v 6 Global Address Set icon](../media/user-guide/netx-events/image65.png)    | **Duo IPv6 Global Address Set** (*nxd_ipv6_global_address_set*) |
| ![Duo I P v 6 Initiate Dad Process icon](../media/user-guide/netx-events/image66.png)    | **Duo IPv6 Initiate Dad Process** (*nxd_ipv6_initiate_dad_process*) |
| ![Duo I P v 6 Interface Address Get icon](../media/user-guide/netx-events/image67.png)    | **Duo IPv6 Interface Address Get** (*nxd_ipv6_interface_address_get*) |
| ![Duo I P v 6 Interface Address Set icon](../media/user-guide/netx-events/image68.png)    | **Duo IPv6 Interface Address Set** (*nxd_ipv6_interface_address_set*) |
| ![Duo I P v 6 Link Local Address Get icon](../media/user-guide/netx-events/image69.png)    | **Duo IPv6 Link Local Address Get** (*nxd_ipv6_linklocal_address_get*) |
| ![Duo I P v 6 Link Local Address Set icon](../media/user-guide/netx-events/image70.png)    | **Duo IPv6 Link Local Address Set** (*nxd_ipv6_linklocal_address_set*) |
| ![Duo I P v 6 Raw Packet Send icon](../media/user-guide/netx-events/image71.png)    | **Duo IPv6 Raw Packet Send** (*nxd_ipv6_raw_packet_send)* |
| ![Duo T C P Socket Peer Info Get icon](../media/user-guide/netx-events/image72.png)    | **Duo TCP Socket Peer Info Get** (*nxd_tcp_socket_peer_info_get*) |
| ![Duo T C P Socket Set Interface icon](../media/user-guide/netx-events/image73.png)    | **Duo TCP Socket Set Interface** (*nxd_tcp_socket_set_interface*) |
| ![Duo U D P Socket Send icon](../media/user-guide/netx-events/image74.png)    | **Duo UDP Socket Send** (*nxd_udp_socket_send*) |
| ![Duo U D P Socket Set Interface icon](../media/user-guide/netx-events/image75.png)    | **Duo UDP Socket Set Interface** (*nxd_udp_socket_set_interface*) |
| ![Duo U D P Source Extract icon](../media/user-guide/netx-events/image76.png)    | **Duo UDP Source Extract** (*nxd_udp_source_extract*) |
| ![I C M P Enable icon](../media/user-guide/netx-events/image77.png)    | **ICMP Enable** (*nx_icmp_enable*) |
| ![I C M P Information Get icon](../media/user-guide/netx-events/image78.png)    | **ICMP Information Get** (*nx_icmp_info_get*) |
| ![I C M P Ping icon](../media/user-guide/netx-events/image79.png)    | **ICMP Ping** (*nx_icmp_ping*) |
| ![I G M P Enable icon](../media/user-guide/netx-events/image80.png)    | **IGMP Enable** (*nx_igmp_enable*) |
| ![I G M P Information Get icon](../media/user-guide/netx-events/image81.png)    | **IGMP Information Get** (*nx_igmp_info_get*) |
| ![I G M P Loopback Disable icon](../media/user-guide/netx-events/image82.png)    | **IGMP Loopback Disable** (*nx_igmp_loopback_disable*) |
| ![I G M P Loopback Enable icon](../media/user-guide/netx-events/image83.png)    | **IGMP Loopback Enable** (*nx_igmp_loopback_enable*) |
| ![I G M P Multicast Join icon](../media/user-guide/netx-events/image84.png)    | **IGMP Multicast Join** (*nx_igmp_multicast_join*) |
| ![I G M P Multicast Leave icon](../media/user-guide/netx-events/image85.png)    | **IGMP Multicast Leave** (*nx_igmp_multicast_leave*) |
| ![I P Address Change Notify icon](../media/user-guide/netx-events/image86.png)    | **IP Address Change Notify** (*nx_ip_address_change_notify*) |
| ![I P Address Get icon](../media/user-guide/netx-events/image87.png)    | **IP Address Get** (*nx_ip_address_get*) |
| ![I P Address Set icon](../media/user-guide/netx-events/image88.png)    | **IP Address Set** (*nx_ip_address_set*) |
| ![I P Create icon](../media/user-guide/netx-events/image89.png)    | **IP Create** (*nx_ip_create*) |
| ![I P Delete icon](../media/user-guide/netx-events/image90.png)    | **IP Delete** (*nx_ip_delete*) |
| ![I P Driver Direct Command icon](../media/user-guide/netx-events/image91.png)    | **IP Driver Direct Command** (*nx_ip_driver_direct_command*) |
| ![I P Forwarding Disable icon](../media/user-guide/netx-events/image92.png)    | **IP Forwarding Disable** (*nx_ip_forwarding_disable*) |
| ![I P Forwarding Enable icon](../media/user-guide/netx-events/image93.png)    | **IP Forwarding Enable** (*nx_ip_forwarding_enable*) |
| ![I P Fragment Disable icon](../media/user-guide/netx-events/image94.png)    | **IP Fragment Disable** (*nx_ip_fragment_disable*) |
| ![I P Fragment Enable icon](../media/user-guide/netx-events/image95.png)    | **IP Fragment Enable** (*nx_ip_fragment_enable*)  |
| ![I P Gateway Address Set icon](../media/user-guide/netx-events/image96.png)    | **IP Gateway Address Set** (*nx_ip_gateway_address_set*) |
| ![I P Information Get icon](../media/user-guide/netx-events/image97.png)    | **IP Information Get** (*nx_ip_info_get*) |
| ![I P Interface Attach icon](../media/user-guide/netx-events/image98.png)    | **IP Interface Attach** (*nx_ip_interface_attach*) |
| ![I P Interface Info Get icon](../media/user-guide/netx-events/image99.png)    | **IP Interface Info Get** (*nx_ip_interface_info_get*) |
| ![I P Raw Packet Disable icon](../media/user-guide/netx-events/image100.png)    | **IP Raw Packet Disable** (*nx_ip_raw_packet_disable*) |
| ![I P Raw Packet Enable icon](../media/user-guide/netx-events/image101.png)    | **IP Raw Packet Enable** (*nx_ip_raw_packet_enable*) |
| ![I P Raw Packet Receive icon](../media/user-guide/netx-events/image102.png)    | **IP Raw Packet Receive** (*nx_ip_raw_packet_receive*) |
| ![I P Raw Packet Send icon](../media/user-guide/netx-events/image103.png)    | **IP Raw Packet Send** (*nx_ip_raw_packet_send*) |
| ![I P Static Route Add icon](../media/user-guide/netx-events/image104.png)    | **IP Static Route Add** (*nx_ip_static_route_add*) |
| ![I P Static Route Delete icon](../media/user-guide/netx-events/image105.png)    | **IP Static Route Delete** (*nx_ip_static_route_delete)* |
| ![I P Status Check icon](../media/user-guide/netx-events/image106.png)    | **IP Status Check** (*nx_ip_status_check*)|
| ![I P S E C Enable icon](../media/user-guide/netx-events/Image107.png)    | **IPSEC Enable** (*nx_ipsec_enable)* |
| ![Packet Allocate icon](../media/user-guide/netx-events/Image108.png)    | **Packet Allocate** (*nx_packet_allocate*) |
| ![Packet Copy icon](../media/user-guide/netx-events/Image109.png)    | **Packet Copy** (*nx_packet_copy*) |
| ![Packet Data Append icon](../media/user-guide/netx-events/Image110.png)    | **Packet Data Append** (*nx_packet_data_append*) |
| ![Packet Data Extract Offset icon](../media/user-guide/netx-events/Image111.png)    | **Packet Data Extract Offset** (*nx_packet_data_extract_offset*) |
| ![Packet Data Retrieve icon](../media/user-guide/netx-events/Image112.png)    | **Packet Data Retrieve** (*nx_packet_data_retrieve*) |
| ![Packet Length Get icon](../media/user-guide/netx-events/Image113.png)    | **Packet Length Get** (*nx_packet_length_get*) |
| ![Packet Pool Create icon](../media/user-guide/netx-events/Image114.png)    | **Packet Pool Create** (*nx_packet_pool_create*) |
| ![Packet Pool Delete icon](../media/user-guide/netx-events/Image115.png)    | **Packet Pool Delete** (*nx_packet_pool_delete*) |
| ![Packet Pool Information Get icon](../media/user-guide/netx-events/Image116.png)    | **Packet Pool Information Get** (*nx_packet_pool_info_get*) |
| ![Packet Release icon](../media/user-guide/netx-events/Image117.png)    | **Packet Release** (*nx_packet_release*) |
| ![Packet Transmit Release icon](../media/user-guide/netx-events/Image118.png)    | **Packet Transmit Release** (*nx_packet_transmit_release*) |
| ![R A R P Disable icon](../media/user-guide/netx-events/Image119.png)    | **RARP Disable** (*nx_rarp_disable*) |
| ![R A R P Enable icon](../media/user-guide/netx-events/Image120.png)    | **RARP Enable** (*nx_rarp_enable*) |
| ![R A R P Information Get icon](../media/user-guide/netx-events/Image121.png)    | **RARP Information Get** (*nx_rarp_info_get*) |
| ![System Initialize icon](../media/user-guide/netx-events/Image122.png)    | **System Initialize** (*nx_system_initialize*) |
| ![T C P Client Socket Bind icon](../media/user-guide/netx-events/Image123.png)    | **TCP Client Socket Bind** (*nx_tcp_client_socket_bind*) |
| ![T C P Client Socket Connect icon](../media/user-guide/netx-events/Image124.png)    | **TCP Client Socket Connect** (*nx_tcp_client_socket_connect*) |
| ![T C P Client Socket Port Get icon](../media/user-guide/netx-events/Image125.png)    | **TCP Client Socket Port Get** (*nx_tcp_client_socket_port_get*) |
| ![T C P Client Socket Unbind icon](../media/user-guide/netx-events/Image126.png)    | **TCP Client Socket Unbind** (*nx_tcp_client_socket_unbind*) |
| ![T C P Enable icon](../media/user-guide/netx-events/Image127.png)    | **TCP Enable** (*nx_tcp_enable*) |
| ![T C P Free Port Find icon](../media/user-guide/netx-events/Image128.png)    | **TCP Free Port Find** (*nx_tcp_free_port_find*) |
| ![T C P Information Get icon](../media/user-guide/netx-events/Image129.png)    | **TCP Information Get** (*nx_tcp_info_get*) |
| ![T C P Server Socket Accept icon](../media/user-guide/netx-events/Image130.png)    | **TCP Server Socket Accept** (*nx_tcp_server_socket_accept*) |
| ![T C P Server Socket Listen icon](../media/user-guide/netx-events/Image131.png)    | **TCP Server Socket Listen** (*nx_tcp_server_socket_listen*) |
| ![T C P Server Socket Relisten icon](../media/user-guide/netx-events/Image132.png)    | **TCP Server Socket Relisten** (*nx_tcp_server_socket_relisten*) |
| ![T C P Server Socket Unaccept icon](../media/user-guide/netx-events/Image133.png)    | **TCP Server Socket Unaccept** (*nx_tcp_server_socket_unaccept*) |
| ![T C P Server Socket Unlisten icon](../media/user-guide/netx-events/Image134.png)    | **TCP Server Socket Unlisten** (*nx_tcp_server_socket_unlisten*) |
| ![T C P Socket Bytes Available icon](../media/user-guide/netx-events/Image135.png)    | **TCP Socket Bytes Available** (*nx_tcp_socket_bytes_available*) |
| ![T C P Socket Create icon](../media/user-guide/netx-events/Image136.png)    | **TCP Socket Create** (*nx_tcp_socket_create*) |
| ![T C P Socket Delete icon](../media/user-guide/netx-events/Image137.png)    | **TCP Socket Delete** (*nx_tcp_socket_delete*) |
| ![T C P Socket Disconnect icon](../media/user-guide/netx-events/Image138.png)    | **TCP Socket Disconnect** (*nx_tcp_socket_disconnect*) |
| ![T C P Socket Information Get icon](../media/user-guide/netx-events/Image139.png)    | **TCP Socket Information Get** (*nx_tcp_socket_info_get*) |
| ![T C P Socket MSS Get icon](../media/user-guide/netx-events/Image140.png)    | **TCP Socket MSS Get** (*nx_tcp_socket_mss_get*) |
| ![T C P Socket MSS Peer Get icon](../media/user-guide/netx-events/Image141.png)    | **TCP Socket MSS Peer Get** (*nx_tcp_socket_mss_peer_get*) |
| ![T C P Socket MSS Set icon](../media/user-guide/netx-events/Image142.png)    | **TCP Socket MSS Set** (*nx_tcp_socket_mss_set*) |
| ![T C P Socket Peer Info Get icon](../media/user-guide/netx-events/Image143.png)    | **TCP Socket Peer Info Get** (*nx_tcp_socket_peer_info_get*) |
| ![T C P Socket Receive icon](../media/user-guide/netx-events/Image144.png)    | **TCP Socket Receive** (*nx_tcp_socket_receive*) |
| ![T C P Socket Receive Notify icon](../media/user-guide/netx-events/Image145.png)    | **TCP Socket Receive Notify** (*nx_tcp_socket_receive_notify*) |
| ![T C P Socket Send icon](../media/user-guide/netx-events/Image146.png)    | **TCP Socket Send** (*nx_tcp_socket_send*) |
| ![T C P Socket State Wait icon](../media/user-guide/netx-events/Image147.png)    | **TCP Socket State Wait** (*nx_tcp_socket_state_wait*) |
| ![T C P Socket Transmit Configure icon](../media/user-guide/netx-events/Image148.png)    | **TCP Socket Transmit Configure** (*nx_tcp_socket_transmit_configure*) |
| ![T C P Socket Window Update Notify Set icon](../media/user-guide/netx-events/Image149.png)    | **TCP Socket Window Update Notify Set** (*nx_tcp_socket_window_update_notify_set*)  |
| ![U D P Enable icon](../media/user-guide/netx-events/Image150.png)    | **UDP Enable** (*nx_udp_enable*) |
| ![U D P Free Port Find icon](../media/user-guide/netx-events/Image151.png)    | **UDP Free Port Find** (*nx_udp_free_port_find*) |
| ![U D P Information Get icon](../media/user-guide/netx-events/Image152.png)    | **UDP Information Get** (*nx_udp_info_get*) |
| ![U D P Socket Bind icon](../media/user-guide/netx-events/Image153.png)    | **UDP Socket Bind** (*nx_udp_socket_bind*) |
| ![U D P Socket Bytes Available icon](../media/user-guide/netx-events/Image154.png)    | **UDP Socket Bytes Available** (*nx_udp_socket_bytes_available*) |
| ![U D P Socket Checksum Disable icon](../media/user-guide/netx-events/Image155.png)    | **UDP Socket Checksum Disable** (*nx_udp_socket_checksum_disable*) |
| ![U D P Socket Checksum Enable icon](../media/user-guide/netx-events/Image156.png)    | **UDP Socket Checksum Enable** *(nx_udp_socket_checksum_enable)* |
| ![U D P Socket Create icon](../media/user-guide/netx-events/Image157.png)    | **UDP Socket Create** (*nx_udp_socket_create*) |
| ![U D P Socket Delete icon](../media/user-guide/netx-events/Image158.png)    | **UDP Socket Delete** (*nx_udp_socket_delete*) |
| ![U D  Socket Information Get icon](../media/user-guide/netx-events/Image159.png)    | **UDP Socket Information Get** (*nx_udp_socket_info_get*) |
| ![U D P Socket Interface Set icon](../media/user-guide/netx-events/Image160.png)    | **UDP Socket Interface Set** (*nx_udp_socket_interface_set*) |
| ![U D P Socket Port Get icon](../media/user-guide/netx-events/Image161.png)    | **UDP Socket Port Get** (*nx_udp_socket_port_get*) |
| ![U D P Socket Receive icon](../media/user-guide/netx-events/Image162.png)    | **UDP Socket Receive** (*nx_udp_socket_receive*) |
| ![U D P Socket Receive Notify icon](../media/user-guide/netx-events/Image163.png)    | **UDP Socket Receive Notify** (*nx_udp_socket_receive_notify*) |
| ![U D P Socket Send icon](../media/user-guide/netx-events/Image164.png)    | **UDP Socket Send** (*nx_udp_socket_send*) |
| ![U D P Socket Unbind icon](../media/user-guide/netx-events/Image165.png)    | **UDP Socket Unbind** (*nx_udp_socket_unbind*) |
| ![U D P Source Extract icon](../media/user-guide/netx-events/Image166.png)    | **UDP Source Extract** (*nx_udp_source_extract*) |

## Event Descriptions

The following pages describe the NetX Duo Trace Events.

### Internal ARP Request Receive 

#### Internal ARP request receive

**Icon** ![Internal ARP request receive icon](../media/user-guide/netx-events/image1.png)

**Description**

This event represents an internal NetX Duo ARP request receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Not used

### Internal ARP Request Send 

#### Internal ARP request send

**Icon** ![Internal ARP request send icon](../media/user-guide/netx-events/image2.png)

**Description**

This event represents an internal NetX Duo ARP request send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Destination IP address
- Info Field 3: Pointer to packet
- Info Field 4: Not used

### Internal ARP Response Receive 

#### Internal ARP request receive

**Icon** ![The Internal ARP request receive icon](../media/user-guide/netx-events/image3.png)

**Description**

This event represents an internal NetX Duo ARP response receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Not used

### Internal ARP Response Send 

#### Internal ARP request send

**Icon** ![The Internal ARP request send icon](../media/user-guide/netx-events/image4.png)

**Description**

This event represents an internal NetX Duo response send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Destination IP address
- Info Field 3: Pointer to packet
- Info Field 4: Not used

### Internal ICMP Receive 

#### Internal ICMP receive

**Icon** ![Internal I C M P receive icon](../media/user-guide/netx-events/image5.png)

**Description**

This event represents an internal NetX Duo ICMP receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Word 0 of ICMP header

### Internal ICMP Send 

#### Internal ICMP send

**Icon** ![Internal I C M P send icon](../media/user-guide/netx-events/image6.png)

**Description**

This event represents an internal NetX Duo ICMP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Destination IP address
- Info Field 3: Pointer to packet
- Info Field 4: Word 0 of ICMP header

### Internal IGMP Receive 

#### Internal IGMP receive

**Icon** ![Internal I G M P receive icon](../media/user-guide/netx-events/image7.png)

**Description**

This event represents an internal NetX Duo IGMP receive event.

**Information Fields**

- Info Field 1: IP Pointer
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Word 0 of IGMP header

### Internal IP Receive 

#### Internal IP receive

**Icon** ![Internal I P receive icon](../media/user-guide/netx-events/image8.png)

**Description**

This event represents an internal NetX Duo IP receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Packet length

### Internal IP Send

#### Internal IP send

**Icon** ![Internal I P send icon](../media/user-guide/netx-events/image9.png)

**Description**

This event represents an internal NetX Duo IP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Destination IP address
- Info Field 3: Pointer to packet
- Info Field 4: Packet length

### Internal TCP Data Receive 

#### Internal TCP Data Receive

**Icon** ![Internal T C P data receive icon](../media/user-guide/netx-events/image10.png)

**Description**

This event represents an internal NetX Duo TCP data receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Receive sequence number

### Internal TCP data send 

#### Internal TCP Data Send

**Icon** ![Internal T C P data send icon](../media/user-guide/netx-events/image11.png)

**Description**

This event represents an internal NetX Duo TCP data send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Transmit sequence number

### Internal TCP FIN Receive 

#### Internal TCP fin receive

**Icon** ![Internal T C P F I N receive icon](../media/user-guide/netx-events/image12.png)

**Description**

This event represents an internal NetX Duo TCP FIN receive event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Receive sequence number

### Internal TCP FIN Send 

#### Internal TCP fin send

**Icon** ![Internal T C P F I N send icon](../media/user-guide/netx-events/image13.png)

**Description**

This event represents an internal NetX Duo TCP FIN send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4:Transmit sequence number

### Internal TCP RST Receive 

#### Internal TCP RST receive

**Icon** ![Internal T C P R S T receive icon](../media/user-guide/netx-events/image14.png)

**Description**

This event represents an internal NetX Duo TCP reset receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance.
- Info Field 2: Pointer to socket.
- Info Field 3: Pointer to packet.
- Info Field 4: Receive sequence number.

### Internal TCP RST Send 

#### Internal TCP RST send

**Icon** ![Internal T C P R S T send icon](../media/user-guide/netx-events/image15.png)

**Description**

This event represents an internal NetX Duo TCP reset send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Transmit sequence number

### Internal TCP SYN Receive 

#### Internal TCP SYN receive

**Icon** ![Internal T C P S Y N receive icon](../media/user-guide/netx-events/image16.png)

**Description**

This event represents an internal NetX Duo TCP SYN receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Receive sequence number

### Internal TCP SYN Send 

#### Internal TCP SYN send

**Icon** ![Internal T C P S Y N send icon](../media/user-guide/netx-events/image17.png)

**Description**

This event represents an internal NetX Duo TCP SYN send event.

Information Fields 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
-Info Field 4: Transmit sequence number

### Internal UDP Receive 

#### Internal UDP receive

**Icon** ![Internal U D P receive icon](../media/user-guide/netx-events/image18.png)

**Description**

This event represents an internal NetX Duo UDP receive event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Word 0 of UDP header

### Internal UDP Send 

#### Internal UDP send

**Icon** ![Internal U D P send icon](../media/user-guide/netx-events/image19.png)

**Description**

This event represents an internal NetX Duo UDP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Word 0 of UDP header

### Internal RARP Receive 

#### Internal RARP receive 

**Icon** ![Internal R A R P receive icon](../media/user-guide/netx-events/image20.png)

**Description**

This event represents an internal NetX Duo RARP receive event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Target IP address
- Info Field 3: Pointer to packet
- Info Field 4: Word 1 of RARP header

### Internal RARP Send 

#### Internal RARP send 

**Icon** ![Internal R A R P send icon](../media/user-guide/netx-events/image21.png)

**Description**

This event represents an internal NetX Duo RARP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Target IP address
- Info Field 3: Pointer to packet
- Info Field 4: Word 1 of RARP header

### Internal TCP Retry 

#### Internal TCP retry 

**Icon** ![Internal T C P retry icon](../media/user-guide/netx-events/image22.png)

**Description**

This event represents an internal NetX Duo TCP retry event.

**Information Fields**
- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Number of retries

### Internal TCP State Change 

#### Internal TCP state change 

**Icon** ![Internal T C P state change icon](../media/user-guide/netx-events/image23.png)

**Description**

This event represents an internal NetX Duo TCP socket state change event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Previous state
- Info Field 4: New state

### Internal I/O Driver Packet Send 

#### Internal I/O driver packet send

**Icon** ![Internal I / O driver packet send icon](../media/user-guide/netx-events/image24.png)

**Description**

This event represents an internal NetX Duo I/O driver packet send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet size
- Info Field 4: Not used

### Internal I/O Driver Initialize 

#### Internal I/O driver initialize

**Icon** ![Internal I / O driver initialize icon](../media/user-guide/netx-events/image25.png)

**Description**

This event represents an internal NetX Duo I/O driver initialize event.

**Information Fields**

- Info Field 1: Pointer to the IP instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Link Enable 

#### Internal I/O driver link enable

**Icon** ![Internal I / O driver link enable icon](../media/user-guide/netx-events/image26.png)

**Description**

This event represents an internal NetX Duo I/O driver link enable event.

**Information Fields**

- Info Field 1: Pointer to the IP instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Link Disable 

#### Internal I/O driver link disable

**Icon** ![Internal I / O driver link disable icon](../media/user-guide/netx-events/image27.png)

**Description**

This event represents an internal NetX Duo I/O driver link disable event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Packet Broadcast 

#### Internal I/O driver packet broadcast

**Icon** ![Internal I / O driver packet broadcast icon](../media/user-guide/netx-events/image28.png)

**Description**

This event represents an internal NetX Duo I/O driver packet broadcast event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet size
- Info Field 4: Not used

### Internal I/O Driver ARP Send 

#### Internal I/O driver ARP send

**Icon** ![Internal I / O driver ARP send icon](../media/user-guide/netx-events/image29.png)

**Description**

This event represents an internal NetX Duo I/O driver ARP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet size
- Info Field 4: Not used

### Internal I/O Driver ARP Response Send 

#### Internal I/O driver ARP response send

**Icon** ![Internal I / O driver ARP response send icon](../media/user-guide/netx-events/image30.png)

**Description**

This event represents an internal NetX Duo I/O driver ARP response send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet size
- Info Field 4: Not used

### Internal I/O Driver RARP Send 

#### Internal I/O driver RARP send

**Icon** ![Internal I / O driver RARP send icon](../media/user-guide/netx-events/image31.png)

**Description**

This event represents an internal NetX Duo I/O driver RARP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet size
- Info Field 4: Not used

### Internal I/O Driver Multicast Join 

#### Internal I/O driver multicast join

**Icon** ![Internal I / O driver multicast join icon](../media/user-guide/netx-events/image32.png)

**Description**

This event represents an internal NetX Duo I/O driver multicast join event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Multicast Leave 

#### Internal I/O driver multicast leave

**Icon** ![Internal I / O driver multicast leave icon](../media/user-guide/netx-events/image33.png)

**Description**

This event represents an internal NetX Duo I/O driver multicast leave event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get Status 

#### Internal I/O driver get status

**Icon** ![Internal I / O driver get status icon](../media/user-guide/netx-events/image34.png)

**Description**

This event represents an internal NetX Duo I/O driver get status event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get Speed 

#### Internal I/O driver get speed

**Icon** ![Internal I / O driver get speed icon](../media/user-guide/netx-events/image35.png)

**Description**

This event represents an internal NetX Duo I/O driver get speed event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get Duplex Type 

#### Internal I/O driver get duplex type

**Icon** ![Internal I / O driver get duplex type icon](../media/user-guide/netx-events/image36.png)

**Description**

This event represents an internal NetX Duo I/O driver get duplex type event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get Error Count

#### Internal I/O driver get error count

**Icon** ![Internal I / O driver get error count icon](../media/user-guide/netx-events/image37.png)

**Description**

This event represents an internal NetX Duo I/O driver get error count event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get RX Count 

#### Internal I/O driver get RX count

**Icon** ![Internal I / O driver get RX count icon](../media/user-guide/netx-events/image38.png)

**Description**

This event represents an internal NetX Duo I/O driver get RX count event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get TX Count 

#### Internal I/O driver get TX count

**Icon** ![Internal I / O driver get T X count icon](../media/user-guide/netx-events/image39.png)

**Description**

This event represents an internal NetX Duo I/O driver get TX count event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get Allocation Errors 

#### Internal I/O driver get allocation errors

**Icon** ![Internal I / O driver get allocation errors icon](../media/user-guide/netx-events/image40.png)

**Description**

This event represents an internal NetX Duo I/O driver get allocation errors event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Un-initialize 

#### Internal I/O driver un-initialize 

**Icon** ![Internal I / O driver un-initialize icon](../media/user-guide/netx-events/image41.png)

**Description**

This event represents an internal NetX Duo I/O driver un-initialize event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Deferred Processing 

#### Internal I/O driver deferred processing 

**Icon** ![Internal I / O driver deferred processing icon](../media/user-guide/netx-events/image42.png)

**Description**

This event represents an internal NetX Duo I/O driver deferred processing event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet length
- Info Field 4: Not used

### ARP Dynamic Entries Invalidate 

#### nx_arp_dynamic_entries_invalidate

**Icon** ![A R P Dynamic Entries Invalidate icon](../media/user-guide/netx-events/image43.png)

**Description**

This event represents invalidating all dynamic ARP entires via nx_arp_dynamic_entries_invalidate.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Entries invalidated
- Info Field 3: Not used
- Info Field 4: Not used

### ARP Dynamic Entry Set 

#### nx_arp_dynamic_entry_set

**Icon** ![A R P Dynamic Entry Set icon](../media/user-guide/netx-events/image44.png)

**Description**

This event represents setting a dynamic ARP entry via nx_arp_dynamic_entry_set.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Physical address (MSW)
- Info Field 4: Physical address (LSW)

### ARP Enable 

#### nx_arp_enable

**Icon** ![A R P enable icon](../media/user-guide/netx-events/image45.png)

**Description**

This event represents enabling ARP via nx_arp_enable.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: ARP cache memory pointer
- Info Field 3: ARP cache memory size
- Info Field 4: Not used

### ARP Gratuitous Send 

#### nx_arp_gratuitous_send

**Icon** ![A R P gratuitous send icon](../media/user-guide/netx-events/image46.png)

**Description**

This event represents a gratuitous ARP send via nx_arp_gratuitous_send.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### ARP Hardware Address Find 

#### nx_arp_hardware_address_find

**Icon** ![A R P Hardware Address Find icon](../media/user-guide/netx-events/image47.png)

**Description**

This event represents finding a physical address via nx_arp_hardware_address_find.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Physical address (MSW)
- Info Field 4: Physical address (LSW)

### ARP Information Get 

#### nx_arp_info_get

**Icon** ![A R P information get icon](../media/user-guide/netx-events/image48.png)

**Description**

This event represents getting information via nx_arp_info_get.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: ARPs sent
- Info Field 3: ARP responses
- Info Field 4: ARPs received

### ARP IP Address Find 

#### nx_arp_ip_address_find

**Icon** ![A R P I P address find icon](../media/user-guide/netx-events/image49.png)

**Description**

This event represents finding an IP address associated with the supplied physical address via nx_arp_ip_address_find.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Physical address (MSW)
- Info Field 4: Physical address (LSW)

### ARP Static Entry Create 

#### nx_arp_static_entry_create

**Icon** ![A R P static entry create icon](../media/user-guide/netx-events/image50.png)

**Description**

This event represents creating a static ARP entry via nx_arp_static_entry_create.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Physical address (MSW)
- Info Field 4: Physical address (LSW)

### ARP Static Entries Delete 

#### nx_arp_static_entries_delete

**Icon** ![A R P static entries delete icon](../media/user-guide/netx-events/image51.png)

**Description**

This event represents deleting all ARP static entries via nx_arp_static_entries_delete.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Entries deleted
- Info Field 3: Not used
- Info Field 4: Not used

### ARP Static Entry Delete 

### nx_arp_static_entry_delete

**Icon** ![A R P static entry delete icon](../media/user-guide/netx-events/image52.png)

**Description**

This event represents deleting a static ARP entry via nx_arp_static_entry_delete.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Physical address (MSW)
- Info Field 4: Physical address (LSW)

### Duo Cache Entry Delete 

#### nxd_und_cache_entry_delete

**Icon** ![Duo cache entry delete icon](../media/user-guide/netx-events/image53.png)

**Description**

This event represents deleting an entry in the neighbor cache table via nx_udp_socket_create.

**Information Fields** 

- Info Field 1: Fourth (least significant) word of the IPv6 link local address to delete
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Duo Cache Entry Set 

#### nxd_nd_cache_entry_set

**Icon** ![Duo cache entry set icon](../media/user-guide/netx-events/image54.png)

**Description**

This event represents creating a cache entry and adding to the neighbor cache table via nxd_nd_cache_entry_set.

**Information Fields** 

- Info Field 1: Fourth (least significant) word of the IPv6 address to add
- Info Field 2: Physical address msb
- Info Field 3: Physical address lsb
- Info Field 4: Not used

### Duo Cache Invalidate 

#### nxd_nd_cache_invalidate

**Icon** ![Duo cache invalidate icon](../media/user-guide/netx-events/image55.png)

**Description**

This event represents invalidating the entire neighbor cache table via nxd_nd_cache_invalidate.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Duo Cache IP Address Find 

#### nxd_nd_cache_ip_address_find

**Icon** ![Duo cache I P address find icon](../media/user-guide/netx-events/image56.png)

**Description**

This event represents retrieving an IP address matching the supplied physical address from the cache table via nxd_nd_cache_ip_address_find.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth (least significant) word of the IPv6 address
- Info Field 3: Physical address msb
- Info Field 4: Physical address lsb

### Duo ICMP Enable 

#### nxd_icmp_enable

**Icon** ![Duo I C M P enable icon](../media/user-guide/netx-events/image57.png)

**Description**

This event represents ICMPv4 and ICMPv6 services being enabled on the specified IP instance via nxd_icmp_enable.

**Information Fields**
- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Duo ICMP Ping 

#### nxd_icmp_ping

**Icon** ![Duo I C M P ping icon](../media/user-guide/netx-events/image58.png)

**Description**

This event represents sending a ping (echo request) to an IPv6 host via nxd_icmp_ping.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: IPv6 address
- Info Field 3: Pointer to echo data
- Info Field 4: Size of echo data

### Duo IP Max Payload Size Find 

#### nxd_ip_max_payload_size

**Icon** ![Duo I P max payload size find icon](../media/user-guide/netx-events/image59.png)

**Description**

This event computes the max payload the specified packet can carry without requiring fragmentation.

**Information Fields**

- Info Field 1: Socket pointer
- Info Field 2: Peer IP address
- Info Field 3: Peer port Info Field 4: Not used

### Duo IP Raw Packet Send 

#### nxd_ip_max_packet_send

**Icon** ![Duo I P raw packet send icon](../media/user-guide/netx-events/image60.png)

**Description**

This event represents sending a raw IP packet out the specified network interface to the supplied IP destination addressvia nxd_ip_raw_packet_send.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to packet to send
- Info Field 3: Pointer to destination address
- Info Field 4: Packet protocol

### Duo IPv6 Default Router Add 

#### nxd_ipv6_default_router_add

**Icon** ![Duo I P v 6 default router add icon](../media/user-guide/netx-events/image61.png)

**Description**

This event represents adding a default router to the IP instance's IPv6 routing table via nxd_ipv6_default_router_add.

**Information Fields**

- Info Field 1: Pointer to IP instance.
- Info Field 2: Destination Network address.
- Info Field 3: Life time information.
- Info Field 4: Not used.

### Duo IPv6 Default Router Delete 

#### nxd_ipv6_default_router_delete

**Icon** ![Duo I P v 6 default router delete icon](../media/user-guide/netx-events/image62.png)

**Description**

This event represents removing a default router from the IP instance's IPv6 routing table via nxd_ipv6_default_router_delete.

**Information Fields**

- Info Field 1: Pointer to IP instance.
- Info Field 2: Fourth word (least significant) of the default router IPv6 address.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Duo IPv6 Enable 

#### nxd_ipv6_enable

**Icon** ![Duo I P v 6 enable icon](../media/user-guide/netx-events/image63.png)

**Description**

This event represents enabling IPv6 services on the supplied IP instance via nxd_ipv6_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Duo IPv6 Global Address Get 

#### nxd_ipv6_global_address_get

**Icon** ![Duo I P v 6 global address get icon](../media/user-guide/netx-events/image64.png)

**Description**

This event represents retrieving the global (primary) IP address on the IP instance located at index 1 in the IP instance interface table via nxd_ipv6_global_address_get.

**Information Fields**

- Info Field 1: Pointer to IP instance.
- Info Field 2: Fourth word (least significant) of the global address
- Info Field 3: IPv6 address prefix length.
- Info Field 4: Index into IP interface table (1).

### Duo IPv6 Global Address Set 

#### nxd_ipv6_global_address_set

**Icon** ![Duo I P v 6 global address set icon](../media/user-guide/netx-events/image65.png)

**Description**

This event represents setting the global (primary) IP address on the IP instance located at index 1 in the IP instance interface table via nxd_ipv6_global_address_set.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth word (least significant) of the global address
- Info Field 3: IPv6 address prefix length
- Info Field 4: Index into IP interface table (1)

### Duo IPv6 Initiate Dad Process 

#### nxd_ipv6_initiate_dad_process

**Icon** ![Duo I P v 6 initiate dad process icon](../media/user-guide/netx-events/image66.png)

**Description**

This event represents the start of the Duplicate Address Detection (DAD) process when the IP instance is assigned a link local or an IP interface address via nxd_ipv6_initiate_dad_process.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Duo IPv6 Interface Address Get 

#### nxd_ipv6_interface_address_get

**Icon** ![Duo I P v 6 interface address get icon](../media/user-guide/netx-events/image67.png)

**Description**

This event represents retrieving the IP address and prefix at the specified index into the IP instance interface address table via nxd_ipv6_interface_address_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth word (least significant) of the IPv6 address to return
- Info Field 4: Index of interface into the IP instance interface table

### Duo IPv6 Interface Address Set 

### nxd_ipv6_interface_address_set

**Icon** ![Duo I P v 6 interface address set icon](../media/user-guide/netx-events/image68.png)

**Description**

This event represents setting the IP address and prefix at the specified index into the IP instance interface address table. Not permitted on index zero (link local address) via nxd_ipv6_interface_address_set.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth word (least significant) of the IPv6 address to return
- Info Field 3: Prefix length
- Info Field 4: Index of interface into the IP instance interface table

### Duo IPv6 Link Local Address Get 

#### nxd_ipv6_linklocal_address_get

**Icon** ![Duo I P v 6 link local address get icon](../media/user-guide/netx-events/image69.png)

**Description**

This event represents retrieving the link local address of the specified IP instance via nxd_ipv6_linklocal_address_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth word (least significant) of the IP v6 link local address
- Info Field 3: Not used
- Info Field 4: Not used

### Duo IPv6 Link Local Address Set 

#### nxd_ipv6_linklocal_address_set

**Icon** ![Duo I P v 6 link local address set icon](../media/user-guide/netx-events/image70.png)

**Description**

This event represents setting the link local address of the IP instance via nxd_ipv6_linklocal_address_set.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth (least significant) word of the IPv6 link local address
- Info Field 3: Not used
- Info Field 4: Not used

### Duo IPv6 Raw Packet Send 

#### nxd_ipv6_raw_packet_send

**Icon** ![Duo I P v 6 raw packet send icon](../media/user-guide/netx-events/image71.png)

**Description**

This event represents sending a raw IP packet through the primary IP interface to the specified destination via nxd_ip_raw_packet_send.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to packet to send
- Info Field 3: Destination IP address
- Info Field 4: Not used

### Duo TCP Socket Peer Info Get 

#### nxd_tcp_socket_peer_info_get

**Icon** ![Duo T C P socket peer info get icon](../media/user-guide/netx-events/image72.png)

**Description**

This event extracts the sender data from a received TCP packet on the specified socket. It returns the IP address and port of the sender.

**Information Fields**

- Info Field 1: Socket pointer
- Info Field 2: Peer IP address
- Info Field 3: Peer port
- Info Field 4: The lease significant 32-bit of the IP address

### Duo TCP Socket Set Interface 

#### nxd_tcp_socket_set_interface

**Icon** ![Duo T C P socket set interface icon](../media/user-guide/netx-events/image73.png)

**Description**

This event represents setting the outgoing socket interface after a client connects with a TCP server on the specified server IP address via nxd_tcp_client_socket_connect.

**Information Fields** 

- Info Field 1: Pointer to TCP Socket
- Info Field 2: Interface ID
- Info Field 3: Not used
- Info Field 4: Not used

### Duo UDP Socket Send 

#### nxd_udp_socket_send

**Icon** ![Duo U D P socket send icon](../media/user-guide/netx-events/image74.png)

**Description**

This event represents sending a UDP packet through the specified socket with the input IP address and port via nxd_udp_socket_send.

**Information Fields** 

- Info Field 1: Pointer UDP Socket
- Info Field 2: Pointer to UDP packet
- Info Field 3: Packet length
- Info Field 4: Not used


### Duo UDP Socket Set Interface 

#### nxd_udp_socket_set_interface

**Icon** ![Duo U D P socket set interface icon](../media/user-guide/netx-events/image75.png)

**Description**

This event represents setting the specified UDP socket outgoing interface to the interface corresponding to the input interface ID via nxd_udp_socket_set_interface.

**Information Fields** 

- Info Field 1: Pointer to UDP Socket
- Info Field 2: Interface ID
- Info Field 3: Not used
- Info Field 4: Not used

### Duo UDP Source Extract 

#### nxd_udp_socket_extract

**Icon** ![Duo U D P source extract icon](../media/user-guide/netx-events/image76.png)

**Description**

This event represents extracting the IP address and source port of a received packet (either IPv4 or IPv6). If IPv6, the fourth word (least significant) of the IP address is returned via nxd_udp_source_extract.

**Information Fields**

- Info Field 1: Pointer to the packet
- Info Field 2: IP version
- Info Field 3: Source IP address (IPv4 or IPv6)
- Info Field 4: Source port

### ICMP Enable 

#### nx_icmp_enable

**Icon** ![I C M P enable icon](../media/user-guide/netx-events/image77.png)

**Description**

This event represents enabling ICMP via nx_icmp_enable.

**Information Fields**

- Info Field 1: Pointer to the IP instance l;
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### ICMP Information Get 

#### nx_icmp_info_get

**Icon** ![I C M P information get icon](../media/user-guide/netx-events/image78.png)

**Description**

This event represents getting information via nx_icmp_info_get.

**Information Fields**
- Info Field 1: Pointer to the IP instance
- Info Field 2: Pings sent
- Info Field 3: Ping responses
- Info Field 4: Pings received

### ICMP Ping 

#### nx_icmp_ping

**Icon** ![I C M P ping icon](../media/user-guide/netx-events/image79.png)

**Description**

This event represents pinging a target IP address via nx_icmp_ping.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Pointer to data
- Info Field 4: Size of data

### IGMP Enable 

#### nx_icmp_enable

**Icon** ![I G M P enable icon](../media/user-guide/netx-events/image80.png)

**Description**

This event represents enabling IGMP via nx_igmp_enable.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IGMP Information Get 

#### nx_icmp_info_get

**Icon** ![I G M P information get icon](../media/user-guide/netx-events/image81.png)

**Description**

This event represents getting information via nx_igmp_info_get.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Reports sent
- Info Field 3: Queries received
- Info Field 4: Groups joined

### IGMP Loopback Disable 

#### nx_igmp_loopback_disable

**Icon** ![I G M P loopback disable icon](../media/user-guide/netx-events/image82.png)

**Description**

This event represents disabling IGMP loopback via nx_igmp_loopback_disable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IGMP Loopback Enable 

#### nx_igmp_loopback_enable

**Icon** ![I G M P loopback enable icon](../media/user-guide/netx-events/image83.png)

**Description**

This event represents enabling IGMP loopback via nx_igmp_loopback_enable.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IGMP Multicast Join 

#### nx_igmp_multicast_join

**Icon** ![I G M P multicast join icon](../media/user-guide/netx-events/image84.png)

**Description**

This event represents joining a multicast group via nx_igmp_multicast_join.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Group IP address
- Info Field 3: Not used
- Info Field 4: Not used

### IGMP Multicast Leave 

#### nx_igmp_multicast_leave

**Icon** ![I G M P multicast leave icon](../media/user-guide/netx-events/image85.png)

**Description**

This event represents leaving a multicast group via nx_igmp_multicast_leave.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Group IP address
- Info Field 3: Not used
- Info Field 4: Not used

### IP Address Change Notify 

#### nx_ip_address_change_notify

**Icon** ![I P address change notify icon](../media/user-guide/netx-events/image86.png)

**Description**

This event represents registering for IP change notification via nx_ip_address_change_notify.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Callback function pointer
- Info Field 3: Additional information pointer
- Info Field 4: Not used

### IP Address Get 

#### nx_ip_address_get

**Icon** ![I P address get icon](../media/user-guide/netx-events/image87.png)

**Description**

This event represents getting the IP address via nx_ip_address_get.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Network mask
- Info Field 4: Not used

### IP Address Set 

#### nx_ip_address_set

**Icon** ![I P address set icon](../media/user-guide/netx-events/image88.png)

**Description**

This event represents setting the IP address via nx_ip_address_set.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Network mask
- Info Field 4: Not used

### IP Create 

#### nx_ip_create

**Icon** ![I P create icon](../media/user-guide/netx-events/image89.png)

**Description**

This event represents creating an IP instance via nx_ip_create.

**Information Fields** 
- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Network mask
- Info Field 4: Default packet pool pointer

### IP Delete 

#### nx_ip_delete

**Icon** ![I P delete icon](../media/user-guide/netx-events/image90.png)

**Description**

This event represents deleting an IP instance via nx_ip_delete.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Driver Direct Command 

#### nx_ip_driver_direct_command

**Icon** ![I P driver direct command icon](../media/user-guide/netx-events/image91.png)

**Description**

This event represents a direct I/O driver command via nx_ip_driver_direct_command.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Driver command
- Info Field 3: Return value
- Info Field 4: Not used

### IP Forwarding Disable 

#### nx_ip_forwarding_disable

**Icon** ![I P forwarding disable icon](../media/user-guide/netx-events/image92.png)

**Description**

This event represents disabling IP forwarding via nx_ip_forwarding_disable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Forwarding Enable 

#### nx_ip_forwarding_enable

**Icon** ![I P forwarding enable icon](../media/user-guide/netx-events/image93.png)

**Description**

This event represents enabling IP forwarding via nx_ip_forwarding_enable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Fragment Disable 

#### nx_ip_fragment_disable

**Icon** ![I P fragment disable icon](../media/user-guide/netx-events/image94.png)

**Description**

This event represents disabling IP fragmenting via nx_ip_fragment_disable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Fragment Enable 

#### nx_ip_fragment_enable

**Icon** ![I P fragment enable icon](../media/user-guide/netx-events/image95.png)

**Description**

This event represents enabling IP fragmenting via nx_ip_fragment_enable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Gateway Address Set 

#### nx_ip_gateway_address_set

**Icon** ![I P gateway address set icon](../media/user-guide/netx-events/image96.png)

**Description**

This event represents setting the gateway IP address via nx_ip_gateway_address_set.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Gateway IP address
- Info Field 3: Not used
- Info Field 4: Not used

### IP Information Get 

#### nx_ip_info_get

**Icon** ![I P information get icon](../media/user-guide/netx-events/image97.png)

**Description**
This event represents getting IP information via nx_ip_info_get.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP bytes sent
- Info Field 3: IP bytes received
- Info Field 4: IP packets dropped

### IP Interface Attach 

#### nx_interface_attach

**Icon** ![I P interface attach icon](../media/user-guide/netx-events/image98.png)

**Description**

This event represents a secondary network interface being attached to the IP instance via nx_ip_interface_attach.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Interface IP Address
- Info Field 3: Index into IP interface table
- Info Field 4: Not used

### IP Interface Info Get 

#### nx_ip_interface_info_get

**Icon** ![ IP interface info get icon](../media/user-guide/netx-events/image99.png)

**Description**

This event represents information retrieved from the specified network interface via nx_ip_interface_info_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Interface IP address
- Info Field 3: Interface MAC address msb
- Info Field 4: Interface MAC address lsb

### IP Raw Packet Disable 

#### nx_ip_raw_packet_disable

**Icon** ![I P raw packet disable icon](../media/user-guide/netx-events/image100.png)

**Description**

This event represents disabling raw IP packet communication via nx_ip_raw_packet_disable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Raw Packet Enable 

#### nx_ip_raw_packet_enable

**Icon** ![I P raw packet enable icon](../media/user-guide/netx-events/image101.png)

**Description**

This event represents enabling raw IP packet communication via nx_ip_raw_packet_enable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Raw Packet Receive 

#### nx_ip_raw_packet_receive

**Icon** ![I P raw packet receive icon](../media/user-guide/netx-events/image102.png)

**Description**

This event represents receiving a raw IP packet via nx_ip_raw_packet_receive.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Wait option
- Info Field 4: Not used

### IP Raw Packet Send 

#### nx_ip_raw_packet_send

**Icon** ![I P raw packet send icon](../media/user-guide/netx-events/image103.png)

**Description**

This event represents sending a raw IP packet via nx_ip_raw_packet_send.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Destination IP address
- Info Field 4: Type of service

### IP Static Route Add 

#### nx_ip_static_route_add

**Icon** ![I P static route add icon](../media/user-guide/netx-events/image104.png)

**Description**

This event represents a static route being added to the IP instance routing table via nx_ip_static_route_add.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Network address
- Info Field 3: Network mask
- Info Field 4: Next hop

### IP Static Route Delete 

#### nx_ip_static_route_delete

**Icon** ![I P static route delete icon](../media/user-guide/netx-events/image105.png)

**Description**

This event represents a static route being removed from the IP instance routing table via nx_ip_static_route_delete.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Network address
- Info Field 3: Network mask
- Info Field 4: Not used

### IP Status Check 

#### nx_ip_status_check

**Icon** ![I P status check icon](../media/user-guide/netx-events/image106.png)

**Description**

This event represents checking for an IP status via nx_ip_status_check.

Information Fields 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Requested status
- Info Field 3: Actual status
- Info Field 4: Wait option

### IPSEC Enable 

#### nx_ipsec_enable

**Icon** ![I P S E C enable icon](../media/user-guide/netx-events/Image107.png)

**Description**

This event represents enabling IPSec services on the supplied IP instance via nx_ipsec_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Packet Allocate 

#### nx_packet_allocate

**Icon** ![Packet allocate icon](../media/user-guide/netx-events/Image108.png)

**Description**

This event represents allocating a packet via nx_packet_allocate.

**Information Fields**

- Info Field 1: Pointer to the packet pool
- Info Field 2: Pointer to packet allocated
- Info Field 3: Packet type
- Info Field 4: Available packets

### Packet Copy 

#### nx_packet_copy

**Icon** ![Packet cpy icon](../media/user-guide/netx-events/Image109.png)

**Description**

This event represents copying a packet via nx_packet_copy.

**Information Fields**

- Info Field 2: New packet pointer
- Info Field 3: Pointer to packet pool
- Info Field 4: Wait option

### Packet Data Append 

#### nx_packet_data_append

**Icon** ![Packet data append icon](../media/user-guide/netx-events/Image110.png)

**Description**

This event represents appending data to a packet via nx_packet_data_append.

**Information Fields**

- Info Field 1: Pointer to the packet
- Info Field 2: Pointer to data
- Info Field 3: Size of data
- Info Field 4: Pointer to packet pool

### Packet Data Extract Offset 

#### nx_udp_source_extract_offset

**Icon** ![Packet data extract offset icon](../media/user-guide/netx-events/Image111.png)

**Description**

This event represents packet data that is extracted into a supplied buffer from a packet via nx_udp_source_extract_offset.

**Information Fields**

- Info Field 1: Pointer to packet
- Info Field 2: Size of specified buffer
- Info Field 3: Number of bytes copied
- Info Field 4: Not used

### Packet Data Retrieve 

#### nx_packet_data_retrieve

**Icon** ![Packet data retrieve icon](../media/user-guide/netx-events/Image112.png)

**Description**

This event represents retrieving data from a packet via nx_packet_data_retrieve.

**Information Fields** 

- Info Field 1: Pointer to the packet
- Info Field 2: Pointer to start of buffer
- Info Field 3: Bytes copied
- Info Field 4: Not used

### Packet Length Get 

#### nx_packet_length_get

**Icon** ![Packet length get icon](../media/user-guide/netx-events/Image113.png)

**Description**

This event represents getting the length of a packet via nx_packet_length_get.

**Information Fields**

- Info Field 1: Pointer to the packet
- Info Field 2: Packet length
- Info Field 3: Not used
- Info Field 4: Not used

### Packet Pool Create 

#### nx_packet_pool_create

**Icon** ![Packet pool create icon](../media/user-guide/netx-events/Image114.png)

**Description**

This event represents creating a packet pool via nx_packet_pool_create.

**Information Fields** 

- Info Field 1: Pointer to the packet pool
- Info Field 2: Packet payload size
- Info Field 3: Pointer to pool memory area
- Info Field 4: Size of pool memory area

### Packet Pool Delete 

#### nx_packet_pool_delete

**Icon** ![Packet pool delete icon](../media/user-guide/netx-events/Image115.png)

**Description**

This event represents deleting a packet pool via nx_packet_pool_delete.

**Information Fields** 

- Info Field 1: Pointer to the packet pool
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

#### Packet Pool Information Get 

#### nx_packet_pool_info_get

**Icon** ![Packet pool information get icon](../media/user-guide/netx-events/Image116.png)

**Description**

This event represents getting packet pool information via nx_packet_pool_info_get.

**Information Fields**

- Info Field 1: Pointer to packet pool
- Info Field 2: Total packets
- Info Field 3: Available packets
- Info Field 4: Empty requests

### Packet Release 

#### nx_packet_data_release

**Icon** ![Packet release icon](../media/user-guide/netx-events/Image117.png)

**Description**

This event represents releasing a packet via nx_packet_release.

**Information Fields**

- Info Field 1: Pointer to the packet
- Info Field 2: Packet status
- Info Field 3: Available packets
- Info Field 4: Not used

### Packet Transmit Release 

#### nx_packet_transmit_release

**Icon** ![Packet transmit release icon](../media/user-guide/netx-events/Image118.png)

**Description**

This event represents releasing a transmit packet via nx_packet_transmit_release.

**Information Fields**

- Info Field 1: Pointer to the packet
- Info Field 2: Packet status
- Info Field 3: Available packets
- Info Field 4: Not used

### RARP Disable 

#### nx_rarp_disable

**Icon** ![R A R P disable icon](../media/user-guide/netx-events/Image119.png)

**Description**

This event represents disabling RARP via nx_rarp_disable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### RARP Enable 

#### nx_rarp_enable

**Icon** ![R A R P enable icon](../media/user-guide/netx-events/Image120.png)

**Description**

This event represents enabling RARP via nx_rarp_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### RARP Information Get 

#### nx_rarp_info_get

**Icon** ![R A R P information get icon](../media/user-guide/netx-events/Image121.png)

**Description**

This event represents getting RARP information via nx_rarp_info_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Requests sent
- Info Field 3: Responses received
- Info Field 4: Invalid responses

### System Initialize 

#### nx_system_initialize

**Icon** ![System initialize icon](../media/user-guide/netx-events/Image122.png)

**Description**

This event represents initializing NetX via nx_system_initialize.

**Information Fields**

- Info Field 1: Not used
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### TCP Client Socket Bind 

#### nx_tcp_client_socket_bind

**Icon** ![T  P client socket bind icon](../media/user-guide/netx-events/Image123.png)

**Description**

This event represents binding a client socket to a port via nx_tcp_client_socket_bind.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Port requested
- Info Field 4: Wait option

### TCP Client Socket Connect 

#### nx_tcp_client_socket_connect

**Icon** ![T C P client socket connect icon](../media/user-guide/netx-events/Image124.png)

**Description**

This event represents making a client socket connection via nx_tcp_client_socket_connect.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Server IP address
- Info Field 4: Server port requested

### TCP Client Socket Port Get 

#### nx_tcp_client_socket_port_get

**Icon** ![T C P client socket port get icon](../media/user-guide/netx-events/Image125.png)

**Description**

This event represents getting the client socket port number via nx_tcp_client_socket_port_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Port number
- Info Field 4: Not used

### TCP Client Socket Unbind 

#### nx_tcp_client_socket_unbind

**Icon** ![T C P client socket unbind icon](../media/user-guide/netx-events/Image126.png)

**Description**

This event represents unbinding the port associated with the socket via nx_tcp_client_socket_unbind.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Not used
- Info Field 4: Not used

### TCP Enable 

#### nx_tcp_enable

**Icon** ![T C P enable icon](../media/user-guide/netx-events/Image127.png)

**Description**

This event represents enabling TCP via nx_tcp_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

###  TCP Free Port Find TCP Free Port Find 

#### nx_tcp_free_port_find

**Icon** ![T  CP free port find icon](../media/user-guide/netx-events/Image128.png)

**Description**

This event represents finding a free TCP port via nx_tcp_free_port_find.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Starting search port number
- Info Field 3: Free port number
- Info Field 4: Not used

###  TCP Information Get 

#### nx_tcp_info_get

**Icon** ![T C P information get icon](../media/user-guide/netx-events/Image129.png)

**Description**

This event represents retrieving TCP information for the specified IP instance via nx_tcp_info_get.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Number of bytes sent
- Info Field 3: Number of bytes received
- Info Field 4: Number of invalid packets

###  TCP Server Socket Accept 

#### nx_tcp_server_socket_accept

**Icon** ![T C P server socket accept icon](../media/user-guide/netx-events/Image130.png)

**Description**

This event represents setting up the server socket after an active connection request was received via nx_tcp_server_socket_accept.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Wait option
- Info Field 4: Socket state

###  TCP Server Socket Listen 

#### nx_tcp_server_socket_listen

**Icon** ![T C P server socket listen icon](../media/user-guide/netx-events/Image131.png)

**Description**

This event represents register a listen request and a server socket for the specified TCP port via nx_tcp_server_socket_listen.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: TCP port number
- Info Field 3: Pointer to socket
- Info Field 4: Maximum number of connections that can be queued

###  TCP Server Socket Relisten 

#### nx_tcp_server_socket_relisten

**Icon** ![T C P server socket relisten icon](../media/user-guide/netx-events/Image132.png)

**Description**

This event represents register another server socket for an existing listen request on the specified TCP port via nx_tcp_server_socket_relisten.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: TCP port number
- Info Field 3: Pointer to socket
- Info Field 4: Socket state

###  TCP Server Socket Unaccept 

#### nx_tcp_server_socket_unaccept

**Icon** ![T C P server socket unaccept icon](../media/user-guide/netx-events/Image133.png)

**Description**

This event represents removing the server socket from association with the port receiving an earlier passive connection via nx_tcp_server_socket_unaccept.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Socket state
- Info Field 4: Not used

###  TCP Server Socket Unlisten TCP Server Socket Unlisten 

#### nx_tcp_server_socket_unlisten

**Icon** ![T C P server socket unlisten icon](../media/user-guide/netx-events/Image134.png)

**Description**

This event represents removing a previous listen request for the specified TCP port via nx_tcp_server_socket_unlisten.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: TCP port number
- Info Field 3: Not used
- Info Field 4: Not used

### TCP Socket Bytes Available 

#### nx_tcp_socket_bytes_available

**Icon** ![T C P socket bytes available icon](../media/user-guide/netx-events/Image135.png)

**Description**

This event represents the number of bytes currently available on the specified TCP receiving socket via nx_tcp_socket_bytes_available.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to TCP socket
- Info Field 3: Bytes received on the socket
- Info Field 4: Not used

### TCP Socket Create 

#### nx_tcp_socket_create

**Icon** ![T C P socket create icon](../media/user-guide/netx-events/Image136.png)

**Description**

This event represents creating a TCP socket via nx_tcp_socket_create.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Type of service
- Info Field 4: Receive window size

### TCP Socket Delete 

#### nx_tcp_socket_delete

**Icon** ![T C P socket delete icon](../media/user-guide/netx-events/Image137.png)

**Description**

This event represents deleting a socket via nx_tcp_socket_delete.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Socket state
- Info Field 4: Not used

### TCP Socket Disconnect 

#### nx_tcp_socket_disconnect

**Icon** ![T C P socket disconnect icon](../media/user-guide/netx-events/Image138.png)

**Description**

This event represents disconnecting a socket via nx_tcp_socket_disconnect.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Wait option
- Info Field 4: Socket state

### TCP Socket Information Get 

#### nx_tcp_socket_info_get

**Icon** ![T C P socket information get icon](../media/user-guide/netx-events/Image139.png)

**Description**

This event represents getting information about a socket via nx_tcp_socket_info_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Bytes sent through this socket
- Info Field 4: Bytes received through this socket

### TCP Socket MSS Get 

#### nx_tcp_socket_mss_get

**Icon** ![T C P socket M S S get icon](../media/user-guide/netx-events/Image140.png)

**Description**

This event represents getting the socket's MSS via nx_tcp_socket_mss_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Maximum Segment Size (MSS)
- Info Field 4: Socket state

### TCP Socket MSS Peer Get 

#### nx_tcp_socket_mss_peer_get

**Icon** ![T C P socket M S S peer get icon](../media/user-guide/netx-events/Image141.png)

**Description**

This event represents getting the MSS value of the socket's peer via nx_tcp_socket_mss_peer_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Peer's MSS
- Info Field 4: Socket state

### TCP Socket MSS Set 

#### nx_tcp_socket_mss_set

**Icon** ![T C P socket M S S set icon](../media/user-guide/netx-events/Image142.png)

**Description**

This event represents setting a socket's MSS via nx_tcp_socket_mss_set.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: MSS
- Info Field 4: Socket state

### TCP Socket Peer Info Get 

#### nx_tcp_socket_peer_info_get

**Icon** ![T C P socket peer info get icon](../media/user-guide/netx-events/Image143.png)

**Description**

This event represents information retrieved from the TCP socket regarding the peer (e.g. >connecting host) IP address and port via nx_tcp_socket_peer_info_get.

**Information Fields**

- Info Field 1: Pointer to TCP socket
- Info Field 2: Peer IP address
- Info Field 3: Peer port number
- Info Field 4: Not used

### TCP Socket Receive 

#### nx_tcp_socket_receive

**Icon** ![T C P socket receive icon](../media/user-guide/netx-events/Image144.png)

**Description**

This event represents receiving data from a socket via nx_tcp_socket_receive.

**Information Fields**

- Info Field 1: Pointer to socket
- Info Field 2: Pointer to received packet
- Info Field 3: Received packet length
- Info Field 4: Receive sequence number

### TCP Socket Receive Notify 

#### nx_tcp_socket_receive_notify

**Icon** ![T C P socket receive notify icon](../media/user-guide/netx-events/Image145.png)

**Description**

This event represents registering a receive notify callback for a socket via nx_tcp_socket_receive_notify.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to receive notify callback Info Field 4: Not used

### TCP Socket Send 

#### nx_tcp_socket_send

**Icon** ![T C P socket send icon](../media/user-guide/netx-events/Image146.png)

**Description**

This event represents sending data on a socket via nx_tcp_socket_send.

**Information Fields**

- Info Field 1: Pointer to socket
- Info Field 2: Pointer to packet
- Info Field 3: Length of packet
- Info Field 4: Transmit sequence number

### TCP Socket State Wait 

#### nx_tcp_socket_state_wait

**Icon** ![T C P socket state wait icon](../media/user-guide/netx-events/Image147.png)

**Description**

This event represents waiting for a socket to enter a particular state via nx_tcp_socket_state_wait.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Desired socket state
- Info Field 4: Previous socket state

### TCP Socket Transmit Configure 

#### nx_tcp_socket_transmit_configure

**Icon** ![T C P socket transmit configure icon](../media/user-guide/netx-events/Image148.png)

**Description**

This event represents configuring the transmit options for a socket via nx_tcp_socket_transmit_configure.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Transmit queue depth
- Info Field 4: Timeout value

### TCP Socket Window Update Notify Set 

#### nx_tcp_window_update_notify_set

**Icon** ![T C P socket window update notify set icon](../media/user-guide/netx-events/Image149.png)

**Description**

This event represents a TCP socket receiving notification of an increase in the remote host receive window via nx_tcp_window_update_notify_set.

**Information Fields** 

- Info Field 1: Pointer to TCP socket
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Enable 

#### nx_udp_enable

**Icon** ![U D P enable icon](../media/user-guide/netx-events/Image150.png)

**Description**

This event represents enabling UDP via nx_udp_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Free Port Find 

#### nx_udp_free_port_find

**Icon** ![U D P free port find icon](../media/user-guide/netx-events/Image151.png)

**Description**

This event represents finding a free UDP port via nx_udp_free_port_find.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Starting port to search from
- Info Field 3: Free port
- Info Field 4: Not used

### UDP Information Get 

#### nx_udp_info_get

**Icon** ![U D P information get icon](../media/user-guide/netx-events/Image152.png)

**Description**

This event represents getting information via nx_udp_info_get.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: UDP bytes sent
- Info Field 3: UDP bytes received
- Info Field 4: Invalid packets

### UDP Socket Bind 

#### nx_udp_socket_bind

**Icon** ![U D P socket bind icon](../media/user-guide/netx-events/Image153.png)

**Description**

This event represents binding a UDP socket to a port via nx_udp_socket_bind.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Port number
- Info Field 4: Wait option

### UDP Socket Bytes Available 

#### nx_udp_socket_bytes_available

**Icon** ![U D P socket bytes available icon](../media/user-guide/netx-events/Image154.png)

**Description**

This event represents the current number of bytes received on the UDP socket via nx_udp_socket_bytes_available.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Bytes received on socket
- Info Field 4: Not used

### UDP Socket Checksum Disable 

#### nx_udp_socket_checksum_disable

**Icon** ![U D P socket checksum disable icon](../media/user-guide/netx-events/Image155.png)

**Description**

This event represents disabling the checksum for data on a UDP socket via nx_udp_socket_checksum_disable.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Socket Checksum Enable 

#### nx_udp_socket_checksum_enable

**Icon** ![U D P socket checksum enable icon](../media/user-guide/netx-events/Image156.png)

**Description**

This event represents enabling checksum processing on a socket via nx_udp_socket_checksum_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Socket Create 

#### nx_udp_socket_create

**Icon** ![U D P socket create icon](../media/user-guide/netx-events/Image157.png)

**Description**

This event represents creating a UDP socket via nx_udp_socket_create.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Type of service
- Info Field 4: Maximum receive queue

### UDP Socket Delete Event 

#### nx_udp_socket_delete event

**Icon** ![U D P socket delete event icon](../media/user-guide/netx-events/Image158.png)

**Description**

This event represents deleting a UDP socket via nx_udp_socket_delete.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Socket Information Get Event 

#### nx_udp_socket_info_get event

**Icon** ![U D P socket information get event icon](../media/user-guide/netx-events/Image159.png)

**Description**

This event represents getting information about a UDP socket via nx_udp_socket_info_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Bytes sent through socket
- Info Field 4: Bytes received through socket

### UDP Socket Interface Set 

#### nx_udp_socket_interface_set event

**Icon** ![U D P socket interface set icon](../media/user-guide/netx-events/Image160.png)

**Description**

This event represents setting the outgoing interface of the specified UDP socket with the specified interface via nx_udp_socket_interface_set.

**Information Fields**

- Info Field 1: Pointer to UDP socket
- Info Field 2: Index corresponding to the interface for the socket
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Socket Port Get 

#### nx_udp_socket_port_get

**Icon** ![U D P socket port get icon](../media/user-guide/netx-events/Image161.png)

**Description**

This event represents retrieving the UDP port the specified UDP socket is bound to via nx_udp_socket_port_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to UDP socket
- Info Field 3: Port number
- Info Field 4: Not used

### UDP Socket Receive 

#### nx_udp_socket_receive

**Icon** ![U D P socket receive icon](../media/user-guide/netx-events/Image162.png)

**Description**

This event represents receiving data on the specified UDP socket via nx_udp_socket_receive.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to UDP socket
- Info Field 3: Pointer to received packet
- Info Field 4: Received packet size

### UDP Socket Receive Notify 

#### nx_udp_socket_receive_notify

**Icon** ![U D P socket receive notify icon](../media/user-guide/netx-events/Image163.png)s
**Description**

This event represents registering a receive notify callback via nx_udp_socket_receive_notify.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to receive notify function Info Field 4: Not used

### UDP Socket Send 

#### nx_udp_socket_send

**Icon** ![U D P socket send icon](../media/user-guide/netx-events/Image164.png)

**Description**

This event represents sending data through a UDP socket via nx_udp_socket_send.

**Information Fields**

- Info Field 1: Pointer to socket
- Info Field 2: Pointer to packet
- Info Field 3: Packet length
- Info Field 4: Destination IP address

### UDP Socket Unbind 

#### nx_udp_socket_unbind

**Icon** ![U D P socket unbind icon](../media/user-guide/netx-events/Image165.png)

**Description**

This event represents unbinding a UDP port with a socket via nx_udp_socket_unbind.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Port number
- Info Field 4: Not used

### UDP Source Extract 

#### nx_udp_socket_create

**Icon** ![U D P source extract icon](../media/user-guide/netx-events/Image166.png)

**Description**

This event represents getting the IP address and port number of a received UDP packet via nx_udp_source_extract.

**Information Fields**

- Info Field 1: Pointer to packet
- Info Field 2: Sender's IP address
- Info Field 3: Sender's port number
- Info Field 4: Not used