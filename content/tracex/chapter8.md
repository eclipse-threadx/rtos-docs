---
title: Chapter 8 - NetX Duo trace events
description: This chapter contains a description of the NetX Duo events. 
---


This chapter contains a description of the NetX Duo events. 

## List of Events and Icons

The following is a list of NetX Duo events displayed by TraceX.

| Icon                                           | Meaning                 |
| -------------------------------- | ------------------------------------- |
| {{< figure src="../media/user-guide/netx-events/image1.png" title="Internal A R P Request Receive icon" imgClass="img-responsive center-block" >}}    | Internal ARP Request Receive |
| {{< figure src="../media/user-guide/netx-events/image2.png" title="Internal A R P Request Send icon" imgClass="img-responsive center-block" >}}    | Internal ARP Request Send |
| {{< figure src="../media/user-guide/netx-events/image3.png" title="Internal A R P Response Receive icon" imgClass="img-responsive center-block" >}}    | Internal ARP Response Receive |
| {{< figure src="../media/user-guide/netx-events/image4.png" title="Internal A R P Response Send icon" imgClass="img-responsive center-block" >}}    | Internal ARP Response Send |
| {{< figure src="../media/user-guide/netx-events/image5.png" title="Internal I C M P Receive icon" imgClass="img-responsive center-block" >}}    | Internal ICMP Receive |
| {{< figure src="../media/user-guide/netx-events/image6.png" title="Internal I C M P Send icon" imgClass="img-responsive center-block" >}}    | Internal ICMP Send |
| {{< figure src="../media/user-guide/netx-events/image7.png" title="Internal NetX Duo I G M P Receive icon" imgClass="img-responsive center-block" >}}    | Internal NetX Duo IGMP Receive |
| {{< figure src="../media/user-guide/netx-events/image8.png" title="Internal I P Receive icon" imgClass="img-responsive center-block" >}}    | Internal IP Receive |
| {{< figure src="../media/user-guide/netx-events/image9.png" title="Internal I P Send icon" imgClass="img-responsive center-block" >}}    | Internal IP Send |
| {{< figure src="../media/user-guide/netx-events/image10.png" title="Internal T C P Data Receive icon" imgClass="img-responsive center-block" >}}    | Internal TCP Data Receive |
| {{< figure src="../media/user-guide/netx-events/image11.png" title="Internal T C P Data Send icon" imgClass="img-responsive center-block" >}}    | Internal TCP Data Send |
| {{< figure src="../media/user-guide/netx-events/image12.png" title="Internal T C P FIN Receive icon" imgClass="img-responsive center-block" >}}    | Internal TCP FIN Receive |
| {{< figure src="../media/user-guide/netx-events/image13.png" title="Internal T C P F I N Send icon" imgClass="img-responsive center-block" >}}    | Internal TCP FIN Send |
| {{< figure src="../media/user-guide/netx-events/image14.png" title="Internal T C P R S T Receive icon" imgClass="img-responsive center-block" >}}    | Internal TCP RST Receive |
| {{< figure src="../media/user-guide/netx-events/image15.png" title="Internal T C P R S T Send icon" imgClass="img-responsive center-block" >}}    | Internal TCP RST Send |
| {{< figure src="../media/user-guide/netx-events/image16.png" title="Internal T C P S Y N Receive icon" imgClass="img-responsive center-block" >}}    | Internal TCP SYN Receive |
| {{< figure src="../media/user-guide/netx-events/image17.png" title="Internal T C P S Y N Send icon" imgClass="img-responsive center-block" >}}    | Internal TCP SYN Send |
| {{< figure src="../media/user-guide/netx-events/image18.png" title="Internal U D P Receive icon" imgClass="img-responsive center-block" >}}    | Internal UDP Receive |
| {{< figure src="../media/user-guide/netx-events/image19.png" title="Internal U D P Send icon" imgClass="img-responsive center-block" >}}    | Internal UDP Send |
| {{< figure src="../media/user-guide/netx-events/image20.png" title="Internal R A R P Receive icon" imgClass="img-responsive center-block" >}}    | Internal RARP Receive |
| {{< figure src="../media/user-guide/netx-events/image21.png" title="Internal R A R P Send icon" imgClass="img-responsive center-block" >}}    | Internal RARP Send |
| {{< figure src="../media/user-guide/netx-events/image22.png" title="Internal T C P Retry icon" imgClass="img-responsive center-block" >}}    | Internal TCP Retry |
| {{< figure src="../media/user-guide/netx-events/image23.png" title="Internal T C P State Change icon" imgClass="img-responsive center-block" >}}    | Internal TCP State Change |
| {{< figure src="../media/user-guide/netx-events/image24.png" title="Internal I / O Driver Packet Send icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Packet Send |
| {{< figure src="../media/user-guide/netx-events/image25.png" title="icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Initialize |
| {{< figure src="../media/user-guide/netx-events/image26.png" title="Internal I / O Driver Initialize icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Link Enable |
| {{< figure src="../media/user-guide/netx-events/image27.png" title="Internal I / O Driver Link Disable icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Link Disable |
| {{< figure src="../media/user-guide/netx-events/image28.png" title="Internal I / O Driver Packet Broadcast icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Packet Broadcast |
| {{< figure src="../media/user-guide/netx-events/image29.png" title="Internal I / O Driver ARP Send icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver ARP Send |
| {{< figure src="../media/user-guide/netx-events/image30.png" title="Internal I / O Driver ARP Response Send icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver ARP Response Send |
| {{< figure src="../media/user-guide/netx-events/image31.png" title="Internal I / O Driver RARP Send icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver RARP Send |
| {{< figure src="../media/user-guide/netx-events/image32.png" title="Internal I / O Driver Multicast Join icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Multicast Join |
| {{< figure src="../media/user-guide/netx-events/image33.png" title="Internal I / O Driver Multicast Leave icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Multicast Leave |
| {{< figure src="../media/user-guide/netx-events/image34.png" title="Internal I / O Driver Get Status icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Get Status |
| {{< figure src="../media/user-guide/netx-events/image35.png" title="Internal I / O Driver Get Speed icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Get Speed |
| {{< figure src="../media/user-guide/netx-events/image36.png" title="Internal I / O Driver Get Duplex Type icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Get Duplex Type |
| {{< figure src="../media/user-guide/netx-events/image37.png" title="Internal I / O Driver Get Error Count icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Get Error Count |
| {{< figure src="../media/user-guide/netx-events/image38.png" title="Internal I / O Driver Get RX Count icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Get RX Count |
| {{< figure src="../media/user-guide/netx-events/image39.png" title="Internal I / O Driver Get TX Count icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Get TX Count |
| {{< figure src="../media/user-guide/netx-events/image40.png" title="Internal I / O Driver Get Allocation Errors icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Get Allocation Errors |
| {{< figure src="../media/user-guide/netx-events/image41.png" title="Internal I / O Driver Un-initialize icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Un-initialize |
| {{< figure src="../media/user-guide/netx-events/image42.png" title="Internal I / O Driver Deferred Processing icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Deferred Processing |
| {{< figure src="../media/user-guide/netx-events/image43.png" title="A R P Dynamic Entries Invalidate icon" imgClass="img-responsive center-block" >}}    | **ARP Dynamic Entries Invalidate** (*nx_arp_dynamic_entries_invalidate*) |
| {{< figure src="../media/user-guide/netx-events/image44.png" title="A R P Dynamic Entry Set icon" imgClass="img-responsive center-block" >}}    | **ARP Dynamic Entry Set** (*nx_arp_dynamic_entry_set*) |
| {{< figure src="../media/user-guide/netx-events/image45.png" title="A R P Enable icon" imgClass="img-responsive center-block" >}}    | **ARP Enable** (*nx_arp_enable*) |
| {{< figure src="../media/user-guide/netx-events/image46.png" title="A R P Gratuitous Send icon" imgClass="img-responsive center-block" >}}    | **ARP Gratuitous Send** (*nx_arp_gratuitous_send*) |
| {{< figure src="../media/user-guide/netx-events/image47.png" title="A R P Hardware Address Find icon" imgClass="img-responsive center-block" >}}    | **ARP Hardware Address Find** (*nx_arp_hardware_address_find*) |
| {{< figure src="../media/user-guide/netx-events/image48.png" title="A R P Information Get icon" imgClass="img-responsive center-block" >}}    | **ARP Information Get** (*nx_arp_info_get*) |
| {{< figure src="../media/user-guide/netx-events/image49.png" title="A R P IP Address Find icon" imgClass="img-responsive center-block" >}}    | **ARP IP Address Find** (*nx_arp_ip_address_find*) |
| {{< figure src="../media/user-guide/netx-events/image50.png" title="A R P Static Entry Create icon" imgClass="img-responsive center-block" >}}    | **ARP Static Entry Create** (*nx_arp_static_entry_create*) |
| {{< figure src="../media/user-guide/netx-events/image51.png" title="A R P Static Entries Delete icon" imgClass="img-responsive center-block" >}}    | **ARP Static Entries Delete** (*nx_arp_static_entries_delete*) |
| {{< figure src="../media/user-guide/netx-events/image52.png" title="A R P Static Entry Delete icon" imgClass="img-responsive center-block" >}}    | **ARP Static Entry Delete** (*nx_arp_static_entry_delete*) |
| {{< figure src="../media/user-guide/netx-events/image53.png" title="Duo Cache Entry Delete icon" imgClass="img-responsive center-block" >}}    | **Duo Cache Entry Delete** (*nxd_nd_cache_entry_delete*) |
| {{< figure src="../media/user-guide/netx-events/image54.png" title="Duo Cache Entry Set icon" imgClass="img-responsive center-block" >}}    | **Duo Cache Entry Set** (*nxd_nd_cache_entry_set*) |
| {{< figure src="../media/user-guide/netx-events/image55.png" title="Duo Cache Invalidate icon" imgClass="img-responsive center-block" >}}    | **Duo Cache Invalidate** (*nxd_nd_cache_invalidate*) |
| {{< figure src="../media/user-guide/netx-events/image56.png" title="Duo Cache I P Address Find icon" imgClass="img-responsive center-block" >}}    | **Duo Cache IP Address Find** (*nxd_nd_cache_ip_address_find*) |
| {{< figure src="../media/user-guide/netx-events/image57.png" title="Duo I C M P Enable icon" imgClass="img-responsive center-block" >}}    | **Duo ICMP Enable** (*nxd_icmp_enable*) |
| {{< figure src="../media/user-guide/netx-events/image58.png" title="Duo I C M P I P v 6 Ping icon" imgClass="img-responsive center-block" >}}    | **Duo ICMP IPv6 Ping** (*nxd_icmp_ping*) |
| {{< figure src="../media/user-guide/netx-events/image59.png" title="Duo I P Max Payload Size Find icon" imgClass="img-responsive center-block" >}}    | **Duo IP Max Payload Size Find** (*nx_max_payload_size_find*) |
| {{< figure src="../media/user-guide/netx-events/image60.png" title="Duo I P Raw Packet Send icon" imgClass="img-responsive center-block" >}}    | **Duo IP Raw Packet Send** (*nxd_ip_raw_packet_send*) |
| {{< figure src="../media/user-guide/netx-events/image61.png" title="Duo I P v 6 Default Router Add icon" imgClass="img-responsive center-block" >}}    | **Duo IPv6 Default Router Add** (*nxd_ipv6_default_router_add*) |
| {{< figure src="../media/user-guide/netx-events/image62.png" title="Duo I P v 6 Default Router Delete icon" imgClass="img-responsive center-block" >}}    | **Duo IPv6 Default Router Delete** (*nxd_ipv6_default_router_delete)* |
| {{< figure src="../media/user-guide/netx-events/image63.png" title="Duo I P v 6 Enable icon" imgClass="img-responsive center-block" >}}    | **Duo IPv6 Enable** (*nxd_ipv6_enable)* |
| {{< figure src="../media/user-guide/netx-events/image64.png" title="Duo I P v 6 Global Address Get icon" imgClass="img-responsive center-block" >}}    | **Duo IPv6 Global Address Get** (*nxd_ipv6_global_address_get*) |
| {{< figure src="../media/user-guide/netx-events/image65.png" title="Duo I P v 6 Global Address Set icon" imgClass="img-responsive center-block" >}}    | **Duo IPv6 Global Address Set** (*nxd_ipv6_global_address_set*) |
| {{< figure src="../media/user-guide/netx-events/image66.png" title="Duo I P v 6 Initiate Dad Process icon" imgClass="img-responsive center-block" >}}    | **Duo IPv6 Initiate Dad Process** (*nxd_ipv6_initiate_dad_process*) |
| {{< figure src="../media/user-guide/netx-events/image67.png" title="Duo I P v 6 Interface Address Get icon" imgClass="img-responsive center-block" >}}    | **Duo IPv6 Interface Address Get** (*nxd_ipv6_interface_address_get*) |
| {{< figure src="../media/user-guide/netx-events/image68.png" title="Duo I P v 6 Interface Address Set icon" imgClass="img-responsive center-block" >}}    | **Duo IPv6 Interface Address Set** (*nxd_ipv6_interface_address_set*) |
| {{< figure src="../media/user-guide/netx-events/image69.png" title="Duo I P v 6 Link Local Address Get icon" imgClass="img-responsive center-block" >}}    | **Duo IPv6 Link Local Address Get** (*nxd_ipv6_linklocal_address_get*) |
| {{< figure src="../media/user-guide/netx-events/image70.png" title="Duo I P v 6 Link Local Address Set icon" imgClass="img-responsive center-block" >}}    | **Duo IPv6 Link Local Address Set** (*nxd_ipv6_linklocal_address_set*) |
| {{< figure src="../media/user-guide/netx-events/image71.png" title="Duo I P v 6 Raw Packet Send icon" imgClass="img-responsive center-block" >}}    | **Duo IPv6 Raw Packet Send** (*nxd_ipv6_raw_packet_send)* |
| {{< figure src="../media/user-guide/netx-events/image72.png" title="Duo T C P Socket Peer Info Get icon" imgClass="img-responsive center-block" >}}    | **Duo TCP Socket Peer Info Get** (*nxd_tcp_socket_peer_info_get*) |
| {{< figure src="../media/user-guide/netx-events/image73.png" title="Duo T C P Socket Set Interface icon" imgClass="img-responsive center-block" >}}    | **Duo TCP Socket Set Interface** (*nxd_tcp_socket_set_interface*) |
| {{< figure src="../media/user-guide/netx-events/image74.png" title="Duo U D P Socket Send icon" imgClass="img-responsive center-block" >}}    | **Duo UDP Socket Send** (*nxd_udp_socket_send*) |
| {{< figure src="../media/user-guide/netx-events/image75.png" title="Duo U D P Socket Set Interface icon" imgClass="img-responsive center-block" >}}    | **Duo UDP Socket Set Interface** (*nxd_udp_socket_set_interface*) |
| {{< figure src="../media/user-guide/netx-events/image76.png" title="Duo U D P Source Extract icon" imgClass="img-responsive center-block" >}}    | **Duo UDP Source Extract** (*nxd_udp_source_extract*) |
| {{< figure src="../media/user-guide/netx-events/image77.png" title="I C M P Enable icon" imgClass="img-responsive center-block" >}}    | **ICMP Enable** (*nx_icmp_enable*) |
| {{< figure src="../media/user-guide/netx-events/image78.png" title="I C M P Information Get icon" imgClass="img-responsive center-block" >}}    | **ICMP Information Get** (*nx_icmp_info_get*) |
| {{< figure src="../media/user-guide/netx-events/image79.png" title="I C M P Ping icon" imgClass="img-responsive center-block" >}}    | **ICMP Ping** (*nx_icmp_ping*) |
| {{< figure src="../media/user-guide/netx-events/image80.png" title="I G M P Enable icon" imgClass="img-responsive center-block" >}}    | **IGMP Enable** (*nx_igmp_enable*) |
| {{< figure src="../media/user-guide/netx-events/image81.png" title="I G M P Information Get icon" imgClass="img-responsive center-block" >}}    | **IGMP Information Get** (*nx_igmp_info_get*) |
| {{< figure src="../media/user-guide/netx-events/image82.png" title="I G M P Loopback Disable icon" imgClass="img-responsive center-block" >}}    | **IGMP Loopback Disable** (*nx_igmp_loopback_disable*) |
| {{< figure src="../media/user-guide/netx-events/image83.png" title="I G M P Loopback Enable icon" imgClass="img-responsive center-block" >}}    | **IGMP Loopback Enable** (*nx_igmp_loopback_enable*) |
| {{< figure src="../media/user-guide/netx-events/image84.png" title="I G M P Multicast Join icon" imgClass="img-responsive center-block" >}}    | **IGMP Multicast Join** (*nx_igmp_multicast_join*) |
| {{< figure src="../media/user-guide/netx-events/image85.png" title="I G M P Multicast Leave icon" imgClass="img-responsive center-block" >}}    | **IGMP Multicast Leave** (*nx_igmp_multicast_leave*) |
| {{< figure src="../media/user-guide/netx-events/image86.png" title="I P Address Change Notify icon" imgClass="img-responsive center-block" >}}    | **IP Address Change Notify** (*nx_ip_address_change_notify*) |
| {{< figure src="../media/user-guide/netx-events/image87.png" title="I P Address Get icon" imgClass="img-responsive center-block" >}}    | **IP Address Get** (*nx_ip_address_get*) |
| {{< figure src="../media/user-guide/netx-events/image88.png" title="I P Address Set icon" imgClass="img-responsive center-block" >}}    | **IP Address Set** (*nx_ip_address_set*) |
| {{< figure src="../media/user-guide/netx-events/image89.png" title="I P Create icon" imgClass="img-responsive center-block" >}}    | **IP Create** (*nx_ip_create*) |
| {{< figure src="../media/user-guide/netx-events/image90.png" title="I P Delete icon" imgClass="img-responsive center-block" >}}    | **IP Delete** (*nx_ip_delete*) |
| {{< figure src="../media/user-guide/netx-events/image91.png" title="I P Driver Direct Command icon" imgClass="img-responsive center-block" >}}    | **IP Driver Direct Command** (*nx_ip_driver_direct_command*) |
| {{< figure src="../media/user-guide/netx-events/image92.png" title="I P Forwarding Disable icon" imgClass="img-responsive center-block" >}}    | **IP Forwarding Disable** (*nx_ip_forwarding_disable*) |
| {{< figure src="../media/user-guide/netx-events/image93.png" title="I P Forwarding Enable icon" imgClass="img-responsive center-block" >}}    | **IP Forwarding Enable** (*nx_ip_forwarding_enable*) |
| {{< figure src="../media/user-guide/netx-events/image94.png" title="I P Fragment Disable icon" imgClass="img-responsive center-block" >}}    | **IP Fragment Disable** (*nx_ip_fragment_disable*) |
| {{< figure src="../media/user-guide/netx-events/image95.png" title="I P Fragment Enable icon" imgClass="img-responsive center-block" >}}    | **IP Fragment Enable** (*nx_ip_fragment_enable*)  |
| {{< figure src="../media/user-guide/netx-events/image96.png" title="I P Gateway Address Set icon" imgClass="img-responsive center-block" >}}    | **IP Gateway Address Set** (*nx_ip_gateway_address_set*) |
| {{< figure src="../media/user-guide/netx-events/image97.png" title="I P Information Get icon" imgClass="img-responsive center-block" >}}    | **IP Information Get** (*nx_ip_info_get*) |
| {{< figure src="../media/user-guide/netx-events/image98.png" title="I P Interface Attach icon" imgClass="img-responsive center-block" >}}    | **IP Interface Attach** (*nx_ip_interface_attach*) |
| {{< figure src="../media/user-guide/netx-events/image99.png" title="I P Interface Info Get icon" imgClass="img-responsive center-block" >}}    | **IP Interface Info Get** (*nx_ip_interface_info_get*) |
| {{< figure src="../media/user-guide/netx-events/image100.png" title="I P Raw Packet Disable icon" imgClass="img-responsive center-block" >}}    | **IP Raw Packet Disable** (*nx_ip_raw_packet_disable*) |
| {{< figure src="../media/user-guide/netx-events/image101.png" title="I P Raw Packet Enable icon" imgClass="img-responsive center-block" >}}    | **IP Raw Packet Enable** (*nx_ip_raw_packet_enable*) |
| {{< figure src="../media/user-guide/netx-events/image102.png" title="I P Raw Packet Receive icon" imgClass="img-responsive center-block" >}}    | **IP Raw Packet Receive** (*nx_ip_raw_packet_receive*) |
| {{< figure src="../media/user-guide/netx-events/image103.png" title="I P Raw Packet Send icon" imgClass="img-responsive center-block" >}}    | **IP Raw Packet Send** (*nx_ip_raw_packet_send*) |
| {{< figure src="../media/user-guide/netx-events/image104.png" title="I P Static Route Add icon" imgClass="img-responsive center-block" >}}    | **IP Static Route Add** (*nx_ip_static_route_add*) |
| {{< figure src="../media/user-guide/netx-events/image105.png" title="I P Static Route Delete icon" imgClass="img-responsive center-block" >}}    | **IP Static Route Delete** (*nx_ip_static_route_delete)* |
| {{< figure src="../media/user-guide/netx-events/image106.png" title="I P Status Check icon" imgClass="img-responsive center-block" >}}    | **IP Status Check** (*nx_ip_status_check*)|
| {{< figure src="../media/user-guide/netx-events/Image107.png" title="I P S E C Enable icon" imgClass="img-responsive center-block" >}}    | **IPSEC Enable** (*nx_ipsec_enable)* |
| {{< figure src="../media/user-guide/netx-events/Image108.png" title="Packet Allocate icon" imgClass="img-responsive center-block" >}}    | **Packet Allocate** (*nx_packet_allocate*) |
| {{< figure src="../media/user-guide/netx-events/Image109.png" title="Packet Copy icon" imgClass="img-responsive center-block" >}}    | **Packet Copy** (*nx_packet_copy*) |
| {{< figure src="../media/user-guide/netx-events/Image110.png" title="Packet Data Append icon" imgClass="img-responsive center-block" >}}    | **Packet Data Append** (*nx_packet_data_append*) |
| {{< figure src="../media/user-guide/netx-events/Image111.png" title="Packet Data Extract Offset icon" imgClass="img-responsive center-block" >}}    | **Packet Data Extract Offset** (*nx_packet_data_extract_offset*) |
| {{< figure src="../media/user-guide/netx-events/Image112.png" title="Packet Data Retrieve icon" imgClass="img-responsive center-block" >}}    | **Packet Data Retrieve** (*nx_packet_data_retrieve*) |
| {{< figure src="../media/user-guide/netx-events/Image113.png" title="Packet Length Get icon" imgClass="img-responsive center-block" >}}    | **Packet Length Get** (*nx_packet_length_get*) |
| {{< figure src="../media/user-guide/netx-events/Image114.png" title="Packet Pool Create icon" imgClass="img-responsive center-block" >}}    | **Packet Pool Create** (*nx_packet_pool_create*) |
| {{< figure src="../media/user-guide/netx-events/Image115.png" title="Packet Pool Delete icon" imgClass="img-responsive center-block" >}}    | **Packet Pool Delete** (*nx_packet_pool_delete*) |
| {{< figure src="../media/user-guide/netx-events/Image116.png" title="Packet Pool Information Get icon" imgClass="img-responsive center-block" >}}    | **Packet Pool Information Get** (*nx_packet_pool_info_get*) |
| {{< figure src="../media/user-guide/netx-events/Image117.png" title="Packet Release icon" imgClass="img-responsive center-block" >}}    | **Packet Release** (*nx_packet_release*) |
| {{< figure src="../media/user-guide/netx-events/Image118.png" title="Packet Transmit Release icon" imgClass="img-responsive center-block" >}}    | **Packet Transmit Release** (*nx_packet_transmit_release*) |
| {{< figure src="../media/user-guide/netx-events/Image119.png" title="R A R P Disable icon" imgClass="img-responsive center-block" >}}    | **RARP Disable** (*nx_rarp_disable*) |
| {{< figure src="../media/user-guide/netx-events/Image120.png" title="R A R P Enable icon" imgClass="img-responsive center-block" >}}    | **RARP Enable** (*nx_rarp_enable*) |
| {{< figure src="../media/user-guide/netx-events/Image121.png" title="R A R P Information Get icon" imgClass="img-responsive center-block" >}}    | **RARP Information Get** (*nx_rarp_info_get*) |
| {{< figure src="../media/user-guide/netx-events/Image122.png" title="System Initialize icon" imgClass="img-responsive center-block" >}}    | **System Initialize** (*nx_system_initialize*) |
| {{< figure src="../media/user-guide/netx-events/Image123.png" title="T C P Client Socket Bind icon" imgClass="img-responsive center-block" >}}    | **TCP Client Socket Bind** (*nx_tcp_client_socket_bind*) |
| {{< figure src="../media/user-guide/netx-events/Image124.png" title="T C P Client Socket Connect icon" imgClass="img-responsive center-block" >}}    | **TCP Client Socket Connect** (*nx_tcp_client_socket_connect*) |
| {{< figure src="../media/user-guide/netx-events/Image125.png" title="T C P Client Socket Port Get icon" imgClass="img-responsive center-block" >}}    | **TCP Client Socket Port Get** (*nx_tcp_client_socket_port_get*) |
| {{< figure src="../media/user-guide/netx-events/Image126.png" title="T C P Client Socket Unbind icon" imgClass="img-responsive center-block" >}}    | **TCP Client Socket Unbind** (*nx_tcp_client_socket_unbind*) |
| {{< figure src="../media/user-guide/netx-events/Image127.png" title="T C P Enable icon" imgClass="img-responsive center-block" >}}    | **TCP Enable** (*nx_tcp_enable*) |
| {{< figure src="../media/user-guide/netx-events/Image128.png" title="T C P Free Port Find icon" imgClass="img-responsive center-block" >}}    | **TCP Free Port Find** (*nx_tcp_free_port_find*) |
| {{< figure src="../media/user-guide/netx-events/Image129.png" title="T C P Information Get icon" imgClass="img-responsive center-block" >}}    | **TCP Information Get** (*nx_tcp_info_get*) |
| {{< figure src="../media/user-guide/netx-events/Image130.png" title="T C P Server Socket Accept icon" imgClass="img-responsive center-block" >}}    | **TCP Server Socket Accept** (*nx_tcp_server_socket_accept*) |
| {{< figure src="../media/user-guide/netx-events/Image131.png" title="T C P Server Socket Listen icon" imgClass="img-responsive center-block" >}}    | **TCP Server Socket Listen** (*nx_tcp_server_socket_listen*) |
| {{< figure src="../media/user-guide/netx-events/Image132.png" title="T C P Server Socket Relisten icon" imgClass="img-responsive center-block" >}}    | **TCP Server Socket Relisten** (*nx_tcp_server_socket_relisten*) |
| {{< figure src="../media/user-guide/netx-events/Image133.png" title="T C P Server Socket Unaccept icon" imgClass="img-responsive center-block" >}}    | **TCP Server Socket Unaccept** (*nx_tcp_server_socket_unaccept*) |
| {{< figure src="../media/user-guide/netx-events/Image134.png" title="T C P Server Socket Unlisten icon" imgClass="img-responsive center-block" >}}    | **TCP Server Socket Unlisten** (*nx_tcp_server_socket_unlisten*) |
| {{< figure src="../media/user-guide/netx-events/Image135.png" title="T C P Socket Bytes Available icon" imgClass="img-responsive center-block" >}}    | **TCP Socket Bytes Available** (*nx_tcp_socket_bytes_available*) |
| {{< figure src="../media/user-guide/netx-events/Image136.png" title="T C P Socket Create icon" imgClass="img-responsive center-block" >}}    | **TCP Socket Create** (*nx_tcp_socket_create*) |
| {{< figure src="../media/user-guide/netx-events/Image137.png" title="T C P Socket Delete icon" imgClass="img-responsive center-block" >}}    | **TCP Socket Delete** (*nx_tcp_socket_delete*) |
| {{< figure src="../media/user-guide/netx-events/Image138.png" title="T C P Socket Disconnect icon" imgClass="img-responsive center-block" >}}    | **TCP Socket Disconnect** (*nx_tcp_socket_disconnect*) |
| {{< figure src="../media/user-guide/netx-events/Image139.png" title="T C P Socket Information Get icon" imgClass="img-responsive center-block" >}}    | **TCP Socket Information Get** (*nx_tcp_socket_info_get*) |
| {{< figure src="../media/user-guide/netx-events/Image140.png" title="T C P Socket MSS Get icon" imgClass="img-responsive center-block" >}}    | **TCP Socket MSS Get** (*nx_tcp_socket_mss_get*) |
| {{< figure src="../media/user-guide/netx-events/Image141.png" title="T C P Socket MSS Peer Get icon" imgClass="img-responsive center-block" >}}    | **TCP Socket MSS Peer Get** (*nx_tcp_socket_mss_peer_get*) |
| {{< figure src="../media/user-guide/netx-events/Image142.png" title="T C P Socket MSS Set icon" imgClass="img-responsive center-block" >}}    | **TCP Socket MSS Set** (*nx_tcp_socket_mss_set*) |
| {{< figure src="../media/user-guide/netx-events/Image143.png" title="T C P Socket Peer Info Get icon" imgClass="img-responsive center-block" >}}    | **TCP Socket Peer Info Get** (*nx_tcp_socket_peer_info_get*) |
| {{< figure src="../media/user-guide/netx-events/Image144.png" title="T C P Socket Receive icon" imgClass="img-responsive center-block" >}}    | **TCP Socket Receive** (*nx_tcp_socket_receive*) |
| {{< figure src="../media/user-guide/netx-events/Image145.png" title="T C P Socket Receive Notify icon" imgClass="img-responsive center-block" >}}    | **TCP Socket Receive Notify** (*nx_tcp_socket_receive_notify*) |
| {{< figure src="../media/user-guide/netx-events/Image146.png" title="T C P Socket Send icon" imgClass="img-responsive center-block" >}}    | **TCP Socket Send** (*nx_tcp_socket_send*) |
| {{< figure src="../media/user-guide/netx-events/Image147.png" title="T C P Socket State Wait icon" imgClass="img-responsive center-block" >}}    | **TCP Socket State Wait** (*nx_tcp_socket_state_wait*) |
| {{< figure src="../media/user-guide/netx-events/Image148.png" title="T C P Socket Transmit Configure icon" imgClass="img-responsive center-block" >}}    | **TCP Socket Transmit Configure** (*nx_tcp_socket_transmit_configure*) |
| {{< figure src="../media/user-guide/netx-events/Image149.png" title="T C P Socket Window Update Notify Set icon" imgClass="img-responsive center-block" >}}    | **TCP Socket Window Update Notify Set** (*nx_tcp_socket_window_update_notify_set*)  |
| {{< figure src="../media/user-guide/netx-events/Image150.png" title="U D P Enable icon" imgClass="img-responsive center-block" >}}    | **UDP Enable** (*nx_udp_enable*) |
| {{< figure src="../media/user-guide/netx-events/Image151.png" title="U D P Free Port Find icon" imgClass="img-responsive center-block" >}}    | **UDP Free Port Find** (*nx_udp_free_port_find*) |
| {{< figure src="../media/user-guide/netx-events/Image152.png" title="U D P Information Get icon" imgClass="img-responsive center-block" >}}    | **UDP Information Get** (*nx_udp_info_get*) |
| {{< figure src="../media/user-guide/netx-events/Image153.png" title="U D P Socket Bind icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Bind** (*nx_udp_socket_bind*) |
| {{< figure src="../media/user-guide/netx-events/Image154.png" title="U D P Socket Bytes Available icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Bytes Available** (*nx_udp_socket_bytes_available*) |
| {{< figure src="../media/user-guide/netx-events/Image155.png" title="U D P Socket Checksum Disable icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Checksum Disable** (*nx_udp_socket_checksum_disable*) |
| {{< figure src="../media/user-guide/netx-events/Image156.png" title="U D P Socket Checksum Enable icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Checksum Enable** *(nx_udp_socket_checksum_enable)* |
| {{< figure src="../media/user-guide/netx-events/Image157.png" title="U D P Socket Create icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Create** (*nx_udp_socket_create*) |
| {{< figure src="../media/user-guide/netx-events/Image158.png" title="U D P Socket Delete icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Delete** (*nx_udp_socket_delete*) |
| {{< figure src="../media/user-guide/netx-events/Image159.png" title="U D  Socket Information Get icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Information Get** (*nx_udp_socket_info_get*) |
| {{< figure src="../media/user-guide/netx-events/Image160.png" title="U D P Socket Interface Set icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Interface Set** (*nx_udp_socket_interface_set*) |
| {{< figure src="../media/user-guide/netx-events/Image161.png" title="U D P Socket Port Get icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Port Get** (*nx_udp_socket_port_get*) |
| {{< figure src="../media/user-guide/netx-events/Image162.png" title="U D P Socket Receive icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Receive** (*nx_udp_socket_receive*) |
| {{< figure src="../media/user-guide/netx-events/Image163.png" title="U D P Socket Receive Notify icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Receive Notify** (*nx_udp_socket_receive_notify*) |
| {{< figure src="../media/user-guide/netx-events/Image164.png" title="U D P Socket Send icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Send** (*nx_udp_socket_send*) |
| {{< figure src="../media/user-guide/netx-events/Image165.png" title="U D P Socket Unbind icon" imgClass="img-responsive center-block" >}}    | **UDP Socket Unbind** (*nx_udp_socket_unbind*) |
| {{< figure src="../media/user-guide/netx-events/Image166.png" title="U D P Source Extract icon" imgClass="img-responsive center-block" >}}    | **UDP Source Extract** (*nx_udp_source_extract*) |

## Event Descriptions

The following pages describe the NetX Duo Trace Events.

### Internal ARP Request Receive 

#### Internal ARP request receive

**Icon** {{< figure src="../media/user-guide/netx-events/image1.png" title="Internal ARP request receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo ARP request receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Not used

### Internal ARP Request Send 

#### Internal ARP request send

**Icon** {{< figure src="../media/user-guide/netx-events/image2.png" title="Internal ARP request send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo ARP request send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Destination IP address
- Info Field 3: Pointer to packet
- Info Field 4: Not used

### Internal ARP Response Receive 

#### Internal ARP request receive

**Icon** {{< figure src="../media/user-guide/netx-events/image3.png" title="The Internal ARP request receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo ARP response receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Not used

### Internal ARP Response Send 

#### Internal ARP request send

**Icon** {{< figure src="../media/user-guide/netx-events/image4.png" title="The Internal ARP request send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo response send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Destination IP address
- Info Field 3: Pointer to packet
- Info Field 4: Not used

### Internal ICMP Receive 

#### Internal ICMP receive

**Icon** {{< figure src="../media/user-guide/netx-events/image5.png" title="Internal I C M P receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo ICMP receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Word 0 of ICMP header

### Internal ICMP Send 

#### Internal ICMP send

**Icon** {{< figure src="../media/user-guide/netx-events/image6.png" title="Internal I C M P send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo ICMP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Destination IP address
- Info Field 3: Pointer to packet
- Info Field 4: Word 0 of ICMP header

### Internal IGMP Receive 

#### Internal IGMP receive

**Icon** {{< figure src="../media/user-guide/netx-events/image7.png" title="Internal I G M P receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo IGMP receive event.

**Information Fields**

- Info Field 1: IP Pointer
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Word 0 of IGMP header

### Internal IP Receive 

#### Internal IP receive

**Icon** {{< figure src="../media/user-guide/netx-events/image8.png" title="Internal I P receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo IP receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Packet length

### Internal IP Send

#### Internal IP send

**Icon** {{< figure src="../media/user-guide/netx-events/image9.png" title="Internal I P send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo IP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Destination IP address
- Info Field 3: Pointer to packet
- Info Field 4: Packet length

### Internal TCP Data Receive 

#### Internal TCP Data Receive

**Icon** {{< figure src="../media/user-guide/netx-events/image10.png" title="Internal T C P data receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo TCP data receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Source IP address
- Info Field 3: Pointer to packet
- Info Field 4: Receive sequence number

### Internal TCP data send 

#### Internal TCP Data Send

**Icon** {{< figure src="../media/user-guide/netx-events/image11.png" title="Internal T C P data send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo TCP data send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Transmit sequence number

### Internal TCP FIN Receive 

#### Internal TCP fin receive

**Icon** {{< figure src="../media/user-guide/netx-events/image12.png" title="Internal T C P F I N receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo TCP FIN receive event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Receive sequence number

### Internal TCP FIN Send 

#### Internal TCP fin send

**Icon** {{< figure src="../media/user-guide/netx-events/image13.png" title="Internal T C P F I N send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo TCP FIN send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4:Transmit sequence number

### Internal TCP RST Receive 

#### Internal TCP RST receive

**Icon** {{< figure src="../media/user-guide/netx-events/image14.png" title="Internal T C P R S T receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo TCP reset receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance.
- Info Field 2: Pointer to socket.
- Info Field 3: Pointer to packet.
- Info Field 4: Receive sequence number.

### Internal TCP RST Send 

#### Internal TCP RST send

**Icon** {{< figure src="../media/user-guide/netx-events/image15.png" title="Internal T C P R S T send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo TCP reset send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Transmit sequence number

### Internal TCP SYN Receive 

#### Internal TCP SYN receive

**Icon** {{< figure src="../media/user-guide/netx-events/image16.png" title="Internal T C P S Y N receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo TCP SYN receive event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Receive sequence number

### Internal TCP SYN Send 

#### Internal TCP SYN send

**Icon** {{< figure src="../media/user-guide/netx-events/image17.png" title="Internal T C P S Y N send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo TCP SYN send event.

Information Fields 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
-Info Field 4: Transmit sequence number

### Internal UDP Receive 

#### Internal UDP receive

**Icon** {{< figure src="../media/user-guide/netx-events/image18.png" title="Internal U D P receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo UDP receive event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Word 0 of UDP header

### Internal UDP Send 

#### Internal UDP send

**Icon** {{< figure src="../media/user-guide/netx-events/image19.png" title="Internal U D P send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo UDP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Word 0 of UDP header

### Internal RARP Receive 

#### Internal RARP receive 

**Icon** {{< figure src="../media/user-guide/netx-events/image20.png" title="Internal R A R P receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo RARP receive event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Target IP address
- Info Field 3: Pointer to packet
- Info Field 4: Word 1 of RARP header

### Internal RARP Send 

#### Internal RARP send 

**Icon** {{< figure src="../media/user-guide/netx-events/image21.png" title="Internal R A R P send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo RARP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Target IP address
- Info Field 3: Pointer to packet
- Info Field 4: Word 1 of RARP header

### Internal TCP Retry 

#### Internal TCP retry 

**Icon** {{< figure src="../media/user-guide/netx-events/image22.png" title="Internal T C P retry icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo TCP retry event.

**Information Fields**
- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to packet
- Info Field 4: Number of retries

### Internal TCP State Change 

#### Internal TCP state change 

**Icon** {{< figure src="../media/user-guide/netx-events/image23.png" title="Internal T C P state change icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo TCP socket state change event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Previous state
- Info Field 4: New state

### Internal I/O Driver Packet Send 

#### Internal I/O driver packet send

**Icon** {{< figure src="../media/user-guide/netx-events/image24.png" title="Internal I / O driver packet send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver packet send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet size
- Info Field 4: Not used

### Internal I/O Driver Initialize 

#### Internal I/O driver initialize

**Icon** {{< figure src="../media/user-guide/netx-events/image25.png" title="Internal I / O driver initialize icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver initialize event.

**Information Fields**

- Info Field 1: Pointer to the IP instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Link Enable 

#### Internal I/O driver link enable

**Icon** {{< figure src="../media/user-guide/netx-events/image26.png" title="Internal I / O driver link enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver link enable event.

**Information Fields**

- Info Field 1: Pointer to the IP instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Link Disable 

#### Internal I/O driver link disable

**Icon** {{< figure src="../media/user-guide/netx-events/image27.png" title="Internal I / O driver link disable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver link disable event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Packet Broadcast 

#### Internal I/O driver packet broadcast

**Icon** {{< figure src="../media/user-guide/netx-events/image28.png" title="Internal I / O driver packet broadcast icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver packet broadcast event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet size
- Info Field 4: Not used

### Internal I/O Driver ARP Send 

#### Internal I/O driver ARP send

**Icon** {{< figure src="../media/user-guide/netx-events/image29.png" title="Internal I / O driver ARP send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver ARP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet size
- Info Field 4: Not used

### Internal I/O Driver ARP Response Send 

#### Internal I/O driver ARP response send

**Icon** {{< figure src="../media/user-guide/netx-events/image30.png" title="Internal I / O driver ARP response send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver ARP response send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet size
- Info Field 4: Not used

### Internal I/O Driver RARP Send 

#### Internal I/O driver RARP send

**Icon** {{< figure src="../media/user-guide/netx-events/image31.png" title="Internal I / O driver RARP send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver RARP send event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet size
- Info Field 4: Not used

### Internal I/O Driver Multicast Join 

#### Internal I/O driver multicast join

**Icon** {{< figure src="../media/user-guide/netx-events/image32.png" title="Internal I / O driver multicast join icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver multicast join event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Multicast Leave 

#### Internal I/O driver multicast leave

**Icon** {{< figure src="../media/user-guide/netx-events/image33.png" title="Internal I / O driver multicast leave icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver multicast leave event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get Status 

#### Internal I/O driver get status

**Icon** {{< figure src="../media/user-guide/netx-events/image34.png" title="Internal I / O driver get status icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver get status event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get Speed 

#### Internal I/O driver get speed

**Icon** {{< figure src="../media/user-guide/netx-events/image35.png" title="Internal I / O driver get speed icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver get speed event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get Duplex Type 

#### Internal I/O driver get duplex type

**Icon** {{< figure src="../media/user-guide/netx-events/image36.png" title="Internal I / O driver get duplex type icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver get duplex type event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get Error Count

#### Internal I/O driver get error count

**Icon** {{< figure src="../media/user-guide/netx-events/image37.png" title="Internal I / O driver get error count icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver get error count event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get RX Count 

#### Internal I/O driver get RX count

**Icon** {{< figure src="../media/user-guide/netx-events/image38.png" title="Internal I / O driver get RX count icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver get RX count event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get TX Count 

#### Internal I/O driver get TX count

**Icon** {{< figure src="../media/user-guide/netx-events/image39.png" title="Internal I / O driver get T X count icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver get TX count event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Get Allocation Errors 

#### Internal I/O driver get allocation errors

**Icon** {{< figure src="../media/user-guide/netx-events/image40.png" title="Internal I / O driver get allocation errors icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver get allocation errors event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Un-initialize 

#### Internal I/O driver un-initialize 

**Icon** {{< figure src="../media/user-guide/netx-events/image41.png" title="Internal I / O driver un-initialize icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver un-initialize event.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Internal I/O Driver Deferred Processing 

#### Internal I/O driver deferred processing 

**Icon** {{< figure src="../media/user-guide/netx-events/image42.png" title="Internal I / O driver deferred processing icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver deferred processing event.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Packet length
- Info Field 4: Not used

### ARP Dynamic Entries Invalidate 

#### nx_arp_dynamic_entries_invalidate

**Icon** {{< figure src="../media/user-guide/netx-events/image43.png" title="A R P Dynamic Entries Invalidate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents invalidating all dynamic ARP entires via nx_arp_dynamic_entries_invalidate.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Entries invalidated
- Info Field 3: Not used
- Info Field 4: Not used

### ARP Dynamic Entry Set 

#### nx_arp_dynamic_entry_set

**Icon** {{< figure src="../media/user-guide/netx-events/image44.png" title="A R P Dynamic Entry Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents setting a dynamic ARP entry via nx_arp_dynamic_entry_set.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Physical address (MSW)
- Info Field 4: Physical address (LSW)

### ARP Enable 

#### nx_arp_enable

**Icon** {{< figure src="../media/user-guide/netx-events/image45.png" title="A R P enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling ARP via nx_arp_enable.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: ARP cache memory pointer
- Info Field 3: ARP cache memory size
- Info Field 4: Not used

### ARP Gratuitous Send 

#### nx_arp_gratuitous_send

**Icon** {{< figure src="../media/user-guide/netx-events/image46.png" title="A R P gratuitous send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a gratuitous ARP send via nx_arp_gratuitous_send.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### ARP Hardware Address Find 

#### nx_arp_hardware_address_find

**Icon** {{< figure src="../media/user-guide/netx-events/image47.png" title="A R P Hardware Address Find icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents finding a physical address via nx_arp_hardware_address_find.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Physical address (MSW)
- Info Field 4: Physical address (LSW)

### ARP Information Get 

#### nx_arp_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/image48.png" title="A R P information get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting information via nx_arp_info_get.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: ARPs sent
- Info Field 3: ARP responses
- Info Field 4: ARPs received

### ARP IP Address Find 

#### nx_arp_ip_address_find

**Icon** {{< figure src="../media/user-guide/netx-events/image49.png" title="A R P I P address find icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents finding an IP address associated with the supplied physical address via nx_arp_ip_address_find.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Physical address (MSW)
- Info Field 4: Physical address (LSW)

### ARP Static Entry Create 

#### nx_arp_static_entry_create

**Icon** {{< figure src="../media/user-guide/netx-events/image50.png" title="A R P static entry create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents creating a static ARP entry via nx_arp_static_entry_create.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Physical address (MSW)
- Info Field 4: Physical address (LSW)

### ARP Static Entries Delete 

#### nx_arp_static_entries_delete

**Icon** {{< figure src="../media/user-guide/netx-events/image51.png" title="A R P static entries delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents deleting all ARP static entries via nx_arp_static_entries_delete.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Entries deleted
- Info Field 3: Not used
- Info Field 4: Not used

### ARP Static Entry Delete 

### nx_arp_static_entry_delete

**Icon** {{< figure src="../media/user-guide/netx-events/image52.png" title="A R P static entry delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents deleting a static ARP entry via nx_arp_static_entry_delete.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Physical address (MSW)
- Info Field 4: Physical address (LSW)

### Duo Cache Entry Delete 

#### nxd_und_cache_entry_delete

**Icon** {{< figure src="../media/user-guide/netx-events/image53.png" title="Duo cache entry delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents deleting an entry in the neighbor cache table via nx_udp_socket_create.

**Information Fields** 

- Info Field 1: Fourth (least significant) word of the IPv6 link local address to delete
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Duo Cache Entry Set 

#### nxd_nd_cache_entry_set

**Icon** {{< figure src="../media/user-guide/netx-events/image54.png" title="Duo cache entry set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents creating a cache entry and adding to the neighbor cache table via nxd_nd_cache_entry_set.

**Information Fields** 

- Info Field 1: Fourth (least significant) word of the IPv6 address to add
- Info Field 2: Physical address msb
- Info Field 3: Physical address lsb
- Info Field 4: Not used

### Duo Cache Invalidate 

#### nxd_nd_cache_invalidate

**Icon** {{< figure src="../media/user-guide/netx-events/image55.png" title="Duo cache invalidate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents invalidating the entire neighbor cache table via nxd_nd_cache_invalidate.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Duo Cache IP Address Find 

#### nxd_nd_cache_ip_address_find

**Icon** {{< figure src="../media/user-guide/netx-events/image56.png" title="Duo cache I P address find icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents retrieving an IP address matching the supplied physical address from the cache table via nxd_nd_cache_ip_address_find.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth (least significant) word of the IPv6 address
- Info Field 3: Physical address msb
- Info Field 4: Physical address lsb

### Duo ICMP Enable 

#### nxd_icmp_enable

**Icon** {{< figure src="../media/user-guide/netx-events/image57.png" title="Duo I C M P enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents ICMPv4 and ICMPv6 services being enabled on the specified IP instance via nxd_icmp_enable.

**Information Fields**
- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Duo ICMP Ping 

#### nxd_icmp_ping

**Icon** {{< figure src="../media/user-guide/netx-events/image58.png" title="Duo I C M P ping icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents sending a ping (echo request) to an IPv6 host via nxd_icmp_ping.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: IPv6 address
- Info Field 3: Pointer to echo data
- Info Field 4: Size of echo data

### Duo IP Max Payload Size Find 

#### nxd_ip_max_payload_size

**Icon** {{< figure src="../media/user-guide/netx-events/image59.png" title="Duo I P max payload size find icon" imgClass="img-responsive center-block" >}}

**Description**

This event computes the max payload the specified packet can carry without requiring fragmentation.

**Information Fields**

- Info Field 1: Socket pointer
- Info Field 2: Peer IP address
- Info Field 3: Peer port Info Field 4: Not used

### Duo IP Raw Packet Send 

#### nxd_ip_max_packet_send

**Icon** {{< figure src="../media/user-guide/netx-events/image60.png" title="Duo I P raw packet send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents sending a raw IP packet out the specified network interface to the supplied IP destination addressvia nxd_ip_raw_packet_send.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to packet to send
- Info Field 3: Pointer to destination address
- Info Field 4: Packet protocol

### Duo IPv6 Default Router Add 

#### nxd_ipv6_default_router_add

**Icon** {{< figure src="../media/user-guide/netx-events/image61.png" title="Duo I P v 6 default router add icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents adding a default router to the IP instance's IPv6 routing table via nxd_ipv6_default_router_add.

**Information Fields**

- Info Field 1: Pointer to IP instance.
- Info Field 2: Destination Network address.
- Info Field 3: Life time information.
- Info Field 4: Not used.

### Duo IPv6 Default Router Delete 

#### nxd_ipv6_default_router_delete

**Icon** {{< figure src="../media/user-guide/netx-events/image62.png" title="Duo I P v 6 default router delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents removing a default router from the IP instance's IPv6 routing table via nxd_ipv6_default_router_delete.

**Information Fields**

- Info Field 1: Pointer to IP instance.
- Info Field 2: Fourth word (least significant) of the default router IPv6 address.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Duo IPv6 Enable 

#### nxd_ipv6_enable

**Icon** {{< figure src="../media/user-guide/netx-events/image63.png" title="Duo I P v 6 enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling IPv6 services on the supplied IP instance via nxd_ipv6_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Duo IPv6 Global Address Get 

#### nxd_ipv6_global_address_get

**Icon** {{< figure src="../media/user-guide/netx-events/image64.png" title="Duo I P v 6 global address get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents retrieving the global (primary) IP address on the IP instance located at index 1 in the IP instance interface table via nxd_ipv6_global_address_get.

**Information Fields**

- Info Field 1: Pointer to IP instance.
- Info Field 2: Fourth word (least significant) of the global address
- Info Field 3: IPv6 address prefix length.
- Info Field 4: Index into IP interface table (1).

### Duo IPv6 Global Address Set 

#### nxd_ipv6_global_address_set

**Icon** {{< figure src="../media/user-guide/netx-events/image65.png" title="Duo I P v 6 global address set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents setting the global (primary) IP address on the IP instance located at index 1 in the IP instance interface table via nxd_ipv6_global_address_set.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth word (least significant) of the global address
- Info Field 3: IPv6 address prefix length
- Info Field 4: Index into IP interface table (1)

### Duo IPv6 Initiate Dad Process 

#### nxd_ipv6_initiate_dad_process

**Icon** {{< figure src="../media/user-guide/netx-events/image66.png" title="Duo I P v 6 initiate dad process icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents the start of the Duplicate Address Detection (DAD) process when the IP instance is assigned a link local or an IP interface address via nxd_ipv6_initiate_dad_process.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Duo IPv6 Interface Address Get 

#### nxd_ipv6_interface_address_get

**Icon** {{< figure src="../media/user-guide/netx-events/image67.png" title="Duo I P v 6 interface address get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents retrieving the IP address and prefix at the specified index into the IP instance interface address table via nxd_ipv6_interface_address_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth word (least significant) of the IPv6 address to return
- Info Field 4: Index of interface into the IP instance interface table

### Duo IPv6 Interface Address Set 

### nxd_ipv6_interface_address_set

**Icon** {{< figure src="../media/user-guide/netx-events/image68.png" title="Duo I P v 6 interface address set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents setting the IP address and prefix at the specified index into the IP instance interface address table. Not permitted on index zero (link local address) via nxd_ipv6_interface_address_set.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth word (least significant) of the IPv6 address to return
- Info Field 3: Prefix length
- Info Field 4: Index of interface into the IP instance interface table

### Duo IPv6 Link Local Address Get 

#### nxd_ipv6_linklocal_address_get

**Icon** {{< figure src="../media/user-guide/netx-events/image69.png" title="Duo I P v 6 link local address get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents retrieving the link local address of the specified IP instance via nxd_ipv6_linklocal_address_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth word (least significant) of the IP v6 link local address
- Info Field 3: Not used
- Info Field 4: Not used

### Duo IPv6 Link Local Address Set 

#### nxd_ipv6_linklocal_address_set

**Icon** {{< figure src="../media/user-guide/netx-events/image70.png" title="Duo I P v 6 link local address set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents setting the link local address of the IP instance via nxd_ipv6_linklocal_address_set.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Fourth (least significant) word of the IPv6 link local address
- Info Field 3: Not used
- Info Field 4: Not used

### Duo IPv6 Raw Packet Send 

#### nxd_ipv6_raw_packet_send

**Icon** {{< figure src="../media/user-guide/netx-events/image71.png" title="Duo I P v 6 raw packet send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents sending a raw IP packet through the primary IP interface to the specified destination via nxd_ip_raw_packet_send.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to packet to send
- Info Field 3: Destination IP address
- Info Field 4: Not used

### Duo TCP Socket Peer Info Get 

#### nxd_tcp_socket_peer_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/image72.png" title="Duo T C P socket peer info get icon" imgClass="img-responsive center-block" >}}

**Description**

This event extracts the sender data from a received TCP packet on the specified socket. It returns the IP address and port of the sender.

**Information Fields**

- Info Field 1: Socket pointer
- Info Field 2: Peer IP address
- Info Field 3: Peer port
- Info Field 4: The lease significant 32-bit of the IP address

### Duo TCP Socket Set Interface 

#### nxd_tcp_socket_set_interface

**Icon** {{< figure src="../media/user-guide/netx-events/image73.png" title="Duo T C P socket set interface icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents setting the outgoing socket interface after a client connects with a TCP server on the specified server IP address via nxd_tcp_client_socket_connect.

**Information Fields** 

- Info Field 1: Pointer to TCP Socket
- Info Field 2: Interface ID
- Info Field 3: Not used
- Info Field 4: Not used

### Duo UDP Socket Send 

#### nxd_udp_socket_send

**Icon** {{< figure src="../media/user-guide/netx-events/image74.png" title="Duo U D P socket send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents sending a UDP packet through the specified socket with the input IP address and port via nxd_udp_socket_send.

**Information Fields** 

- Info Field 1: Pointer UDP Socket
- Info Field 2: Pointer to UDP packet
- Info Field 3: Packet length
- Info Field 4: Not used


### Duo UDP Socket Set Interface 

#### nxd_udp_socket_set_interface

**Icon** {{< figure src="../media/user-guide/netx-events/image75.png" title="Duo U D P socket set interface icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents setting the specified UDP socket outgoing interface to the interface corresponding to the input interface ID via nxd_udp_socket_set_interface.

**Information Fields** 

- Info Field 1: Pointer to UDP Socket
- Info Field 2: Interface ID
- Info Field 3: Not used
- Info Field 4: Not used

### Duo UDP Source Extract 

#### nxd_udp_socket_extract

**Icon** {{< figure src="../media/user-guide/netx-events/image76.png" title="Duo U D P source extract icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents extracting the IP address and source port of a received packet (either IPv4 or IPv6). If IPv6, the fourth word (least significant) of the IP address is returned via nxd_udp_source_extract.

**Information Fields**

- Info Field 1: Pointer to the packet
- Info Field 2: IP version
- Info Field 3: Source IP address (IPv4 or IPv6)
- Info Field 4: Source port

### ICMP Enable 

#### nx_icmp_enable

**Icon** {{< figure src="../media/user-guide/netx-events/image77.png" title="I C M P enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling ICMP via nx_icmp_enable.

**Information Fields**

- Info Field 1: Pointer to the IP instance l;
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### ICMP Information Get 

#### nx_icmp_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/image78.png" title="I C M P information get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting information via nx_icmp_info_get.

**Information Fields**
- Info Field 1: Pointer to the IP instance
- Info Field 2: Pings sent
- Info Field 3: Ping responses
- Info Field 4: Pings received

### ICMP Ping 

#### nx_icmp_ping

**Icon** {{< figure src="../media/user-guide/netx-events/image79.png" title="I C M P ping icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents pinging a target IP address via nx_icmp_ping.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Pointer to data
- Info Field 4: Size of data

### IGMP Enable 

#### nx_icmp_enable

**Icon** {{< figure src="../media/user-guide/netx-events/image80.png" title="I G M P enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling IGMP via nx_igmp_enable.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IGMP Information Get 

#### nx_icmp_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/image81.png" title="I G M P information get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting information via nx_igmp_info_get.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Reports sent
- Info Field 3: Queries received
- Info Field 4: Groups joined

### IGMP Loopback Disable 

#### nx_igmp_loopback_disable

**Icon** {{< figure src="../media/user-guide/netx-events/image82.png" title="I G M P loopback disable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents disabling IGMP loopback via nx_igmp_loopback_disable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IGMP Loopback Enable 

#### nx_igmp_loopback_enable

**Icon** {{< figure src="../media/user-guide/netx-events/image83.png" title="I G M P loopback enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling IGMP loopback via nx_igmp_loopback_enable.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IGMP Multicast Join 

#### nx_igmp_multicast_join

**Icon** {{< figure src="../media/user-guide/netx-events/image84.png" title="I G M P multicast join icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents joining a multicast group via nx_igmp_multicast_join.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Group IP address
- Info Field 3: Not used
- Info Field 4: Not used

### IGMP Multicast Leave 

#### nx_igmp_multicast_leave

**Icon** {{< figure src="../media/user-guide/netx-events/image85.png" title="I G M P multicast leave icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents leaving a multicast group via nx_igmp_multicast_leave.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Group IP address
- Info Field 3: Not used
- Info Field 4: Not used

### IP Address Change Notify 

#### nx_ip_address_change_notify

**Icon** {{< figure src="../media/user-guide/netx-events/image86.png" title="I P address change notify icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents registering for IP change notification via nx_ip_address_change_notify.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Callback function pointer
- Info Field 3: Additional information pointer
- Info Field 4: Not used

### IP Address Get 

#### nx_ip_address_get

**Icon** {{< figure src="../media/user-guide/netx-events/image87.png" title="I P address get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting the IP address via nx_ip_address_get.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Network mask
- Info Field 4: Not used

### IP Address Set 

#### nx_ip_address_set

**Icon** {{< figure src="../media/user-guide/netx-events/image88.png" title="I P address set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents setting the IP address via nx_ip_address_set.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Network mask
- Info Field 4: Not used

### IP Create 

#### nx_ip_create

**Icon** {{< figure src="../media/user-guide/netx-events/image89.png" title="I P create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents creating an IP instance via nx_ip_create.

**Information Fields** 
- Info Field 1: Pointer to the IP instance
- Info Field 2: IP address
- Info Field 3: Network mask
- Info Field 4: Default packet pool pointer

### IP Delete 

#### nx_ip_delete

**Icon** {{< figure src="../media/user-guide/netx-events/image90.png" title="I P delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents deleting an IP instance via nx_ip_delete.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Driver Direct Command 

#### nx_ip_driver_direct_command

**Icon** {{< figure src="../media/user-guide/netx-events/image91.png" title="I P driver direct command icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a direct I/O driver command via nx_ip_driver_direct_command.

**Information Fields** 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Driver command
- Info Field 3: Return value
- Info Field 4: Not used

### IP Forwarding Disable 

#### nx_ip_forwarding_disable

**Icon** {{< figure src="../media/user-guide/netx-events/image92.png" title="I P forwarding disable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents disabling IP forwarding via nx_ip_forwarding_disable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Forwarding Enable 

#### nx_ip_forwarding_enable

**Icon** {{< figure src="../media/user-guide/netx-events/image93.png" title="I P forwarding enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling IP forwarding via nx_ip_forwarding_enable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Fragment Disable 

#### nx_ip_fragment_disable

**Icon** {{< figure src="../media/user-guide/netx-events/image94.png" title="I P fragment disable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents disabling IP fragmenting via nx_ip_fragment_disable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Fragment Enable 

#### nx_ip_fragment_enable

**Icon** {{< figure src="../media/user-guide/netx-events/image95.png" title="I P fragment enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling IP fragmenting via nx_ip_fragment_enable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Gateway Address Set 

#### nx_ip_gateway_address_set

**Icon** {{< figure src="../media/user-guide/netx-events/image96.png" title="I P gateway address set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents setting the gateway IP address via nx_ip_gateway_address_set.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Gateway IP address
- Info Field 3: Not used
- Info Field 4: Not used

### IP Information Get 

#### nx_ip_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/image97.png" title="I P information get icon" imgClass="img-responsive center-block" >}}

**Description**
This event represents getting IP information via nx_ip_info_get.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: IP bytes sent
- Info Field 3: IP bytes received
- Info Field 4: IP packets dropped

### IP Interface Attach 

#### nx_interface_attach

**Icon** {{< figure src="../media/user-guide/netx-events/image98.png" title="I P interface attach icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a secondary network interface being attached to the IP instance via nx_ip_interface_attach.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Interface IP Address
- Info Field 3: Index into IP interface table
- Info Field 4: Not used

### IP Interface Info Get 

#### nx_ip_interface_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/image99.png" title=" IP interface info get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents information retrieved from the specified network interface via nx_ip_interface_info_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Interface IP address
- Info Field 3: Interface MAC address msb
- Info Field 4: Interface MAC address lsb

### IP Raw Packet Disable 

#### nx_ip_raw_packet_disable

**Icon** {{< figure src="../media/user-guide/netx-events/image100.png" title="I P raw packet disable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents disabling raw IP packet communication via nx_ip_raw_packet_disable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Raw Packet Enable 

#### nx_ip_raw_packet_enable

**Icon** {{< figure src="../media/user-guide/netx-events/image101.png" title="I P raw packet enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling raw IP packet communication via nx_ip_raw_packet_enable.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### IP Raw Packet Receive 

#### nx_ip_raw_packet_receive

**Icon** {{< figure src="../media/user-guide/netx-events/image102.png" title="I P raw packet receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents receiving a raw IP packet via nx_ip_raw_packet_receive.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Wait option
- Info Field 4: Not used

### IP Raw Packet Send 

#### nx_ip_raw_packet_send

**Icon** {{< figure src="../media/user-guide/netx-events/image103.png" title="I P raw packet send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents sending a raw IP packet via nx_ip_raw_packet_send.

**Information Fields**

- Info Field 1: Pointer to the IP instance
- Info Field 2: Pointer to packet
- Info Field 3: Destination IP address
- Info Field 4: Type of service

### IP Static Route Add 

#### nx_ip_static_route_add

**Icon** {{< figure src="../media/user-guide/netx-events/image104.png" title="I P static route add icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a static route being added to the IP instance routing table via nx_ip_static_route_add.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Network address
- Info Field 3: Network mask
- Info Field 4: Next hop

### IP Static Route Delete 

#### nx_ip_static_route_delete

**Icon** {{< figure src="../media/user-guide/netx-events/image105.png" title="I P static route delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a static route being removed from the IP instance routing table via nx_ip_static_route_delete.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Network address
- Info Field 3: Network mask
- Info Field 4: Not used

### IP Status Check 

#### nx_ip_status_check

**Icon** {{< figure src="../media/user-guide/netx-events/image106.png" title="I P status check icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents checking for an IP status via nx_ip_status_check.

Information Fields 

- Info Field 1: Pointer to the IP instance
- Info Field 2: Requested status
- Info Field 3: Actual status
- Info Field 4: Wait option

### IPSEC Enable 

#### nx_ipsec_enable

**Icon** {{< figure src="../media/user-guide/netx-events/Image107.png" title="I P S E C enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling IPSec services on the supplied IP instance via nx_ipsec_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### Packet Allocate 

#### nx_packet_allocate

**Icon** {{< figure src="../media/user-guide/netx-events/Image108.png" title="Packet allocate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents allocating a packet via nx_packet_allocate.

**Information Fields**

- Info Field 1: Pointer to the packet pool
- Info Field 2: Pointer to packet allocated
- Info Field 3: Packet type
- Info Field 4: Available packets

### Packet Copy 

#### nx_packet_copy

**Icon** {{< figure src="../media/user-guide/netx-events/Image109.png" title="Packet cpy icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents copying a packet via nx_packet_copy.

**Information Fields**

- Info Field 2: New packet pointer
- Info Field 3: Pointer to packet pool
- Info Field 4: Wait option

### Packet Data Append 

#### nx_packet_data_append

**Icon** {{< figure src="../media/user-guide/netx-events/Image110.png" title="Packet data append icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents appending data to a packet via nx_packet_data_append.

**Information Fields**

- Info Field 1: Pointer to the packet
- Info Field 2: Pointer to data
- Info Field 3: Size of data
- Info Field 4: Pointer to packet pool

### Packet Data Extract Offset 

#### nx_udp_source_extract_offset

**Icon** {{< figure src="../media/user-guide/netx-events/Image111.png" title="Packet data extract offset icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents packet data that is extracted into a supplied buffer from a packet via nx_udp_source_extract_offset.

**Information Fields**

- Info Field 1: Pointer to packet
- Info Field 2: Size of specified buffer
- Info Field 3: Number of bytes copied
- Info Field 4: Not used

### Packet Data Retrieve 

#### nx_packet_data_retrieve

**Icon** {{< figure src="../media/user-guide/netx-events/Image112.png" title="Packet data retrieve icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents retrieving data from a packet via nx_packet_data_retrieve.

**Information Fields** 

- Info Field 1: Pointer to the packet
- Info Field 2: Pointer to start of buffer
- Info Field 3: Bytes copied
- Info Field 4: Not used

### Packet Length Get 

#### nx_packet_length_get

**Icon** {{< figure src="../media/user-guide/netx-events/Image113.png" title="Packet length get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting the length of a packet via nx_packet_length_get.

**Information Fields**

- Info Field 1: Pointer to the packet
- Info Field 2: Packet length
- Info Field 3: Not used
- Info Field 4: Not used

### Packet Pool Create 

#### nx_packet_pool_create

**Icon** {{< figure src="../media/user-guide/netx-events/Image114.png" title="Packet pool create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents creating a packet pool via nx_packet_pool_create.

**Information Fields** 

- Info Field 1: Pointer to the packet pool
- Info Field 2: Packet payload size
- Info Field 3: Pointer to pool memory area
- Info Field 4: Size of pool memory area

### Packet Pool Delete 

#### nx_packet_pool_delete

**Icon** {{< figure src="../media/user-guide/netx-events/Image115.png" title="Packet pool delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents deleting a packet pool via nx_packet_pool_delete.

**Information Fields** 

- Info Field 1: Pointer to the packet pool
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

#### Packet Pool Information Get 

#### nx_packet_pool_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/Image116.png" title="Packet pool information get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting packet pool information via nx_packet_pool_info_get.

**Information Fields**

- Info Field 1: Pointer to packet pool
- Info Field 2: Total packets
- Info Field 3: Available packets
- Info Field 4: Empty requests

### Packet Release 

#### nx_packet_data_release

**Icon** {{< figure src="../media/user-guide/netx-events/Image117.png" title="Packet release icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents releasing a packet via nx_packet_release.

**Information Fields**

- Info Field 1: Pointer to the packet
- Info Field 2: Packet status
- Info Field 3: Available packets
- Info Field 4: Not used

### Packet Transmit Release 

#### nx_packet_transmit_release

**Icon** {{< figure src="../media/user-guide/netx-events/Image118.png" title="Packet transmit release icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents releasing a transmit packet via nx_packet_transmit_release.

**Information Fields**

- Info Field 1: Pointer to the packet
- Info Field 2: Packet status
- Info Field 3: Available packets
- Info Field 4: Not used

### RARP Disable 

#### nx_rarp_disable

**Icon** {{< figure src="../media/user-guide/netx-events/Image119.png" title="R A R P disable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents disabling RARP via nx_rarp_disable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### RARP Enable 

#### nx_rarp_enable

**Icon** {{< figure src="../media/user-guide/netx-events/Image120.png" title="R A R P enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling RARP via nx_rarp_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### RARP Information Get 

#### nx_rarp_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/Image121.png" title="R A R P information get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting RARP information via nx_rarp_info_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Requests sent
- Info Field 3: Responses received
- Info Field 4: Invalid responses

### System Initialize 

#### nx_system_initialize

**Icon** {{< figure src="../media/user-guide/netx-events/Image122.png" title="System initialize icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents initializing NetX via nx_system_initialize.

**Information Fields**

- Info Field 1: Not used
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### TCP Client Socket Bind 

#### nx_tcp_client_socket_bind

**Icon** {{< figure src="../media/user-guide/netx-events/Image123.png" title="T  P client socket bind icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents binding a client socket to a port via nx_tcp_client_socket_bind.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Port requested
- Info Field 4: Wait option

### TCP Client Socket Connect 

#### nx_tcp_client_socket_connect

**Icon** {{< figure src="../media/user-guide/netx-events/Image124.png" title="T C P client socket connect icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents making a client socket connection via nx_tcp_client_socket_connect.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Server IP address
- Info Field 4: Server port requested

### TCP Client Socket Port Get 

#### nx_tcp_client_socket_port_get

**Icon** {{< figure src="../media/user-guide/netx-events/Image125.png" title="T C P client socket port get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting the client socket port number via nx_tcp_client_socket_port_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Port number
- Info Field 4: Not used

### TCP Client Socket Unbind 

#### nx_tcp_client_socket_unbind

**Icon** {{< figure src="../media/user-guide/netx-events/Image126.png" title="T C P client socket unbind icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents unbinding the port associated with the socket via nx_tcp_client_socket_unbind.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Not used
- Info Field 4: Not used

### TCP Enable 

#### nx_tcp_enable

**Icon** {{< figure src="../media/user-guide/netx-events/Image127.png" title="T C P enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling TCP via nx_tcp_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

###  TCP Free Port Find TCP Free Port Find 

#### nx_tcp_free_port_find

**Icon** {{< figure src="../media/user-guide/netx-events/Image128.png" title="T  CP free port find icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents finding a free TCP port via nx_tcp_free_port_find.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Starting search port number
- Info Field 3: Free port number
- Info Field 4: Not used

###  TCP Information Get 

#### nx_tcp_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/Image129.png" title="T C P information get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents retrieving TCP information for the specified IP instance via nx_tcp_info_get.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Number of bytes sent
- Info Field 3: Number of bytes received
- Info Field 4: Number of invalid packets

###  TCP Server Socket Accept 

#### nx_tcp_server_socket_accept

**Icon** {{< figure src="../media/user-guide/netx-events/Image130.png" title="T C P server socket accept icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents setting up the server socket after an active connection request was received via nx_tcp_server_socket_accept.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Wait option
- Info Field 4: Socket state

###  TCP Server Socket Listen 

#### nx_tcp_server_socket_listen

**Icon** {{< figure src="../media/user-guide/netx-events/Image131.png" title="T C P server socket listen icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents register a listen request and a server socket for the specified TCP port via nx_tcp_server_socket_listen.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: TCP port number
- Info Field 3: Pointer to socket
- Info Field 4: Maximum number of connections that can be queued

###  TCP Server Socket Relisten 

#### nx_tcp_server_socket_relisten

**Icon** {{< figure src="../media/user-guide/netx-events/Image132.png" title="T C P server socket relisten icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents register another server socket for an existing listen request on the specified TCP port via nx_tcp_server_socket_relisten.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: TCP port number
- Info Field 3: Pointer to socket
- Info Field 4: Socket state

###  TCP Server Socket Unaccept 

#### nx_tcp_server_socket_unaccept

**Icon** {{< figure src="../media/user-guide/netx-events/Image133.png" title="T C P server socket unaccept icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents removing the server socket from association with the port receiving an earlier passive connection via nx_tcp_server_socket_unaccept.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Socket state
- Info Field 4: Not used

###  TCP Server Socket Unlisten TCP Server Socket Unlisten 

#### nx_tcp_server_socket_unlisten

**Icon** {{< figure src="../media/user-guide/netx-events/Image134.png" title="T C P server socket unlisten icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents removing a previous listen request for the specified TCP port via nx_tcp_server_socket_unlisten.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: TCP port number
- Info Field 3: Not used
- Info Field 4: Not used

### TCP Socket Bytes Available 

#### nx_tcp_socket_bytes_available

**Icon** {{< figure src="../media/user-guide/netx-events/Image135.png" title="T C P socket bytes available icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents the number of bytes currently available on the specified TCP receiving socket via nx_tcp_socket_bytes_available.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to TCP socket
- Info Field 3: Bytes received on the socket
- Info Field 4: Not used

### TCP Socket Create 

#### nx_tcp_socket_create

**Icon** {{< figure src="../media/user-guide/netx-events/Image136.png" title="T C P socket create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents creating a TCP socket via nx_tcp_socket_create.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Type of service
- Info Field 4: Receive window size

### TCP Socket Delete 

#### nx_tcp_socket_delete

**Icon** {{< figure src="../media/user-guide/netx-events/Image137.png" title="T C P socket delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents deleting a socket via nx_tcp_socket_delete.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Socket state
- Info Field 4: Not used

### TCP Socket Disconnect 

#### nx_tcp_socket_disconnect

**Icon** {{< figure src="../media/user-guide/netx-events/Image138.png" title="T C P socket disconnect icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents disconnecting a socket via nx_tcp_socket_disconnect.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Wait option
- Info Field 4: Socket state

### TCP Socket Information Get 

#### nx_tcp_socket_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/Image139.png" title="T C P socket information get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting information about a socket via nx_tcp_socket_info_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Bytes sent through this socket
- Info Field 4: Bytes received through this socket

### TCP Socket MSS Get 

#### nx_tcp_socket_mss_get

**Icon** {{< figure src="../media/user-guide/netx-events/Image140.png" title="T C P socket M S S get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting the socket's MSS via nx_tcp_socket_mss_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Maximum Segment Size (MSS)
- Info Field 4: Socket state

### TCP Socket MSS Peer Get 

#### nx_tcp_socket_mss_peer_get

**Icon** {{< figure src="../media/user-guide/netx-events/Image141.png" title="T C P socket M S S peer get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting the MSS value of the socket's peer via nx_tcp_socket_mss_peer_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Peer's MSS
- Info Field 4: Socket state

### TCP Socket MSS Set 

#### nx_tcp_socket_mss_set

**Icon** {{< figure src="../media/user-guide/netx-events/Image142.png" title="T C P socket M S S set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents setting a socket's MSS via nx_tcp_socket_mss_set.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: MSS
- Info Field 4: Socket state

### TCP Socket Peer Info Get 

#### nx_tcp_socket_peer_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/Image143.png" title="T C P socket peer info get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents information retrieved from the TCP socket regarding the peer (e.g. >connecting host) IP address and port via nx_tcp_socket_peer_info_get.

**Information Fields**

- Info Field 1: Pointer to TCP socket
- Info Field 2: Peer IP address
- Info Field 3: Peer port number
- Info Field 4: Not used

### TCP Socket Receive 

#### nx_tcp_socket_receive

**Icon** {{< figure src="../media/user-guide/netx-events/Image144.png" title="T C P socket receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents receiving data from a socket via nx_tcp_socket_receive.

**Information Fields**

- Info Field 1: Pointer to socket
- Info Field 2: Pointer to received packet
- Info Field 3: Received packet length
- Info Field 4: Receive sequence number

### TCP Socket Receive Notify 

#### nx_tcp_socket_receive_notify

**Icon** {{< figure src="../media/user-guide/netx-events/Image145.png" title="T C P socket receive notify icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents registering a receive notify callback for a socket via nx_tcp_socket_receive_notify.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to receive notify callback Info Field 4: Not used

### TCP Socket Send 

#### nx_tcp_socket_send

**Icon** {{< figure src="../media/user-guide/netx-events/Image146.png" title="T C P socket send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents sending data on a socket via nx_tcp_socket_send.

**Information Fields**

- Info Field 1: Pointer to socket
- Info Field 2: Pointer to packet
- Info Field 3: Length of packet
- Info Field 4: Transmit sequence number

### TCP Socket State Wait 

#### nx_tcp_socket_state_wait

**Icon** {{< figure src="../media/user-guide/netx-events/Image147.png" title="T C P socket state wait icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents waiting for a socket to enter a particular state via nx_tcp_socket_state_wait.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Desired socket state
- Info Field 4: Previous socket state

### TCP Socket Transmit Configure 

#### nx_tcp_socket_transmit_configure

**Icon** {{< figure src="../media/user-guide/netx-events/Image148.png" title="T C P socket transmit configure icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents configuring the transmit options for a socket via nx_tcp_socket_transmit_configure.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Transmit queue depth
- Info Field 4: Timeout value

### TCP Socket Window Update Notify Set 

#### nx_tcp_window_update_notify_set

**Icon** {{< figure src="../media/user-guide/netx-events/Image149.png" title="T C P socket window update notify set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a TCP socket receiving notification of an increase in the remote host receive window via nx_tcp_window_update_notify_set.

**Information Fields** 

- Info Field 1: Pointer to TCP socket
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Enable 

#### nx_udp_enable

**Icon** {{< figure src="../media/user-guide/netx-events/Image150.png" title="U D P enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling UDP via nx_udp_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Free Port Find 

#### nx_udp_free_port_find

**Icon** {{< figure src="../media/user-guide/netx-events/Image151.png" title="U D P free port find icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents finding a free UDP port via nx_udp_free_port_find.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Starting port to search from
- Info Field 3: Free port
- Info Field 4: Not used

### UDP Information Get 

#### nx_udp_info_get

**Icon** {{< figure src="../media/user-guide/netx-events/Image152.png" title="U D P information get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting information via nx_udp_info_get.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: UDP bytes sent
- Info Field 3: UDP bytes received
- Info Field 4: Invalid packets

### UDP Socket Bind 

#### nx_udp_socket_bind

**Icon** {{< figure src="../media/user-guide/netx-events/Image153.png" title="U D P socket bind icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents binding a UDP socket to a port via nx_udp_socket_bind.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Port number
- Info Field 4: Wait option

### UDP Socket Bytes Available 

#### nx_udp_socket_bytes_available

**Icon** {{< figure src="../media/user-guide/netx-events/Image154.png" title="U D P socket bytes available icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents the current number of bytes received on the UDP socket via nx_udp_socket_bytes_available.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Bytes received on socket
- Info Field 4: Not used

### UDP Socket Checksum Disable 

#### nx_udp_socket_checksum_disable

**Icon** {{< figure src="../media/user-guide/netx-events/Image155.png" title="U D P socket checksum disable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents disabling the checksum for data on a UDP socket via nx_udp_socket_checksum_disable.

**Information Fields** 

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Socket Checksum Enable 

#### nx_udp_socket_checksum_enable

**Icon** {{< figure src="../media/user-guide/netx-events/Image156.png" title="U D P socket checksum enable icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents enabling checksum processing on a socket via nx_udp_socket_checksum_enable.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Socket Create 

#### nx_udp_socket_create

**Icon** {{< figure src="../media/user-guide/netx-events/Image157.png" title="U D P socket create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents creating a UDP socket via nx_udp_socket_create.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Type of service
- Info Field 4: Maximum receive queue

### UDP Socket Delete Event 

#### nx_udp_socket_delete event

**Icon** {{< figure src="../media/user-guide/netx-events/Image158.png" title="U D P socket delete event icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents deleting a UDP socket via nx_udp_socket_delete.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Socket Information Get Event 

#### nx_udp_socket_info_get event

**Icon** {{< figure src="../media/user-guide/netx-events/Image159.png" title="U D P socket information get event icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting information about a UDP socket via nx_udp_socket_info_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Bytes sent through socket
- Info Field 4: Bytes received through socket

### UDP Socket Interface Set 

#### nx_udp_socket_interface_set event

**Icon** {{< figure src="../media/user-guide/netx-events/Image160.png" title="U D P socket interface set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents setting the outgoing interface of the specified UDP socket with the specified interface via nx_udp_socket_interface_set.

**Information Fields**

- Info Field 1: Pointer to UDP socket
- Info Field 2: Index corresponding to the interface for the socket
- Info Field 3: Not used
- Info Field 4: Not used

### UDP Socket Port Get 

#### nx_udp_socket_port_get

**Icon** {{< figure src="../media/user-guide/netx-events/Image161.png" title="U D P socket port get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents retrieving the UDP port the specified UDP socket is bound to via nx_udp_socket_port_get.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to UDP socket
- Info Field 3: Port number
- Info Field 4: Not used

### UDP Socket Receive 

#### nx_udp_socket_receive

**Icon** {{< figure src="../media/user-guide/netx-events/Image162.png" title="U D P socket receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents receiving data on the specified UDP socket via nx_udp_socket_receive.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to UDP socket
- Info Field 3: Pointer to received packet
- Info Field 4: Received packet size

### UDP Socket Receive Notify 

#### nx_udp_socket_receive_notify

**Icon** {{< figure src="../media/user-guide/netx-events/Image163.png" title="U D P socket receive notify icon" imgClass="img-responsive center-block" >}}s
**Description**

This event represents registering a receive notify callback via nx_udp_socket_receive_notify.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Pointer to receive notify function Info Field 4: Not used

### UDP Socket Send 

#### nx_udp_socket_send

**Icon** {{< figure src="../media/user-guide/netx-events/Image164.png" title="U D P socket send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents sending data through a UDP socket via nx_udp_socket_send.

**Information Fields**

- Info Field 1: Pointer to socket
- Info Field 2: Pointer to packet
- Info Field 3: Packet length
- Info Field 4: Destination IP address

### UDP Socket Unbind 

#### nx_udp_socket_unbind

**Icon** {{< figure src="../media/user-guide/netx-events/Image165.png" title="U D P socket unbind icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents unbinding a UDP port with a socket via nx_udp_socket_unbind.

**Information Fields**

- Info Field 1: Pointer to IP instance
- Info Field 2: Pointer to socket
- Info Field 3: Port number
- Info Field 4: Not used

### UDP Source Extract 

#### nx_udp_socket_create

**Icon** {{< figure src="../media/user-guide/netx-events/Image166.png" title="U D P source extract icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting the IP address and port number of a received UDP packet via nx_udp_source_extract.

**Information Fields**

- Info Field 1: Pointer to packet
- Info Field 2: Sender's IP address
- Info Field 3: Sender's port number
- Info Field 4: Not used