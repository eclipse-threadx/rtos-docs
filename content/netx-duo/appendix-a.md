---
title: Appendix A - NetX Duo Services
description: Learn about the NetX Duo Services.
---


## Address Resolution Protocol (ARP)  

```c
UINT     nx_arp_dynamic_entries_invalidate(NX_IP *ip_ptr);
```
```c
UINT     nx_arp_dynamic_entry_set(NX_IP *ip_ptr, ULONG
            ip_address, ULONG physical_msw, ULONG physical_lsw);
```
```c
UINT     nx_arp_enable(NX_IP *ip_ptr, VOID *arp_cache_memory,
            ULONG arp_cache_size);
```
```c
UINT     nx_arp_gratuitous_send(NX_IP *ip_ptr,
            VOID (*response_handler)(NX_IP *ip_ptr,
            NX_PACKET *packet_ptr));
```
```c
UINT     nx_arp_hardware_address_find(NX_IP *ip_ptr,
         ULONG ip_address, ULONG*physical_msw,
         ULONG *physical_lsw); 
```
```c
UINT     nx_arp_info_get(NX_IP *ip_ptr, ULONG
            *arp_requests_sent, ULONG*arp_requests_received,
            ULONG *arp_responses_sent,
            ULONG*arp_responses_received,
            ULONG *arp_dynamic_entries,
            ULONG *arp_static_entries,
            ULONG *arp_aged_entries,
            ULONG *arp_invalid_messages);                    
```
```c
UINT     nx_arp_ip_address_find(NX_IP *ip_ptr,
            ULONG *ip_address, ULONG physical_msw,
            ULONG physical_lsw);
```
```c
UINT     nx_arp_static_entries_delete(NX_IP *ip_ptr);
```            
```c
UINT     nx_arp_static_entry_create(NX_IP *ip_ptr,
            ULONG ip_address,
            ULONG physical_msw, ULONG physical_lsw);
```
```c
UINT     nx_arp_static_entry_delete(NX_IP *ip_ptr,
            ULONG ip_address, ULONG physical_msw,
            ULONG physical_lsw);
```

## Internet Control Message Protocol (ICMP)  

```c
UINT     nx_icmp_enable(NX_IP *ip_ptr);
```
```c
UINT     nx_icmp_info_get(NX_IP *ip_ptr, ULONG *pings_sent,
            ULONG *ping_timeouts, ULONG *ping_threads_suspended,
            ULONG *ping_responses_received,
            ULONG *icmp_checksum_errors,
            ULONG *icmp_unhandled_messages);
```
```c
UINT     nx_icmp_ping(NX_IP *ip_ptr,
            ULONG ip_address, CHAR *data,
            ULONG data_size, NX_PACKET **response_ptr,
            ULONG wait_option);
```
```c
UINT     nxd_icmp_enable(NX_IP *ip_ptr)
```
```c
UINT     nxd_icmp_ping(NX_IP *ip_ptr, NXD_ADDRESS *ip_address,
            CHAR *data_ptr, ULONG data_size, NX_PACKET
            **response_ptr, ULONG wait_option)
```
```c
UINT     nxd_icmp_interface_ping(NX_IP *ip_ptr, NXD_ADDRESS
            *ip_address, UINT source_index, CHAR *data_ptr,
            ULONG data_size, NX_PACKET **response_ptr, ULONG
            wait_option);
```

## Internet Group Management Protocol (IGMP)  

```c
UINT     nx_igmp_enable(NX_IP *ip_ptr);
```
```c
UINT     nx_igmp_info_get(NX_IP *ip_ptr, ULONG
            *igmp_reports_sent, ULONG *igmp_queries_received,
            ULONG *igmp_checksum_errors,
            ULONG *current_groups_joined);
```
```c
UINT     nx_igmp_loopback_disable(NX_IP *ip_ptr);
```
```c
UINT     nx_igmp_loopback_enable(NX_IP *ip_ptr);
```
```c
UINT     nx_igmp_multicast_interface_join(NX_IP *ip_ptr,
            ULONG group_address, UINT interface_index);
```
```c
UINT     nx_igmp_multicast_join(NX_IP *ip_ptr,
            ULONG group_address);
```
```c
UINT     nx_igmp_multicast_leave(NX_IP *ip_ptr,
            ULONG group_address);
```
## Internet Protocol (IP) 

```c
UINT     nx_ip_address_change_notify(NX_IP *ip_ptr,
            VOID (*change_notify)(NX_IP *, VOID *),
            VOID *additional_info);
```
```c
UINT     nx_ip_address_get(NX_IP *ip_ptr, ULONG *ip_address,
            ULONG *network_mask);
```
```c
UINT     nx_ip_address_set(NX_IP *ip_ptr, ULONG ip_address,
ULONG network_mask);
```
```c
UINT     nx_ip_create(NX_IP *ip_ptr, CHAR *name,
            ULONG ip_address,
            ULONG network_mask, NX_PACKET_POOL *default_pool,
            VOID (*ip_network_driver)(NX_IP_DRIVER *),
            VOID *memory_ptr, ULONG memory_size, UINT priority);
```
```c
UINT     nx_ip_delete(NX_IP *ip_ptr);
```
```c
UINT     nx_ip_driver_direct_command(NX_IP *ip_ptr, UINT
            command, ULONG *return_value_ptr);
```
```c
UINT     nx_ip_driver_interface_direct_command(NX_IP *ip_ptr,
            UINT command, UINT interface_index, ULONG
            *return_value_ptr);
```
```c
UINT     nx_ip_forwarding_disable(NX_IP *ip_ptr);
```
```c
UINT     nx_ip_forwarding_enable(NX_IP *ip_ptr);
```
```c
UINT     nx_ip_fragment_disable(NX_IP *ip_ptr);
```
```c
UINT     nx_ip_fragment_enable(NX_IP *ip_ptr);
```
```c
UINT     nx_ip_gateway_address_set(NX_IP *ip_ptr,
            ULONG ip_address);
```
```c
UINT     nx_ip_info_get(NX_IP *ip_ptr,
            ULONG *ip_total_packets_sent,
            ULONG *ip_total_bytes_sent,
            ULONG *ip_total_packets_received,
            ULONG *ip_total_bytes_received,
            ULONG *ip_invalid_packets,
            ULONG *ip_receive_packets_dropped,
            ULONG *ip_receive_checksum_errors,
            ULONG *ip_send_packets_dropped,
            ULONG *ip_total_fragments_sent,
            ULONG *ip_total_fragments_received);
```
```c
UINT     nx_ip_interface_address_get(NX_IP *ip_ptr,
            ULONG interface_index,
            ULONG *ip_address,
            ULONG *network_mask);
```
```c
UINT     nx_ip_interface_address_set(NX_IP *ip_ptr,
            ULONG interface_index, ULONG ip_address, ULONG
            network_mask);
```
```c
UINT     nx_ip_interface_attach(NX_IP *ip_ptr, CHAR*
            interface_name, ULONG ip_address, ULONG
            network_mask,
            VOID (*ip_link_driver)(struct NX_IP_DRIVER_STRUCT
            *));
```
```c
UINT     nx_ip_interface_info_get(NX_IP *ip_ptr, UINT
            interface_index, CHAR **interface_name, ULONG
            *ip_address,
            ULONG *network_mask, ULONG *mtu_size,
            ULONG *physical_address_msw, ULONG
            *physical_address_lsw);
```
```c
UINT     nx_ip_interface_status_check(NX_IP *ip_ptr,
            UINT interface_index, ULONG needed_status,
            ULONG *actual_status, ULONG wait_option);
```
```c
UINT     nx_ip_max_payload_size_find(NX_IP *ip_ptr,
            NXD_ADDRESS *dest_address, UINT if_index,
            UINT src_port,
            UINT dest_port, ULONG protocol,
            ULONG *start_offset_ptr,
            ULONG *payload_length_ptr)
```
```c
UINT     nx_ip_raw_packet_disable(NX_IP *ip_ptr);
```
```c
UINT     nx_ip_raw_packet_enable(NX_IP *ip_ptr);
```
```c
UINT     nx_ip_raw_packet_interface_send(NX_IP *ip_ptr,
            NX_PACKET *packet_ptr, ULONG destination_ip,
            UINT interface_index, ULONG type_of_service);
```
```c
UINT     nx_ip_raw_packet_receive(NX_IP *ip_ptr,
            NX_PACKET **packet_ptr,
            ULONG wait_option);
```
```c
UINT     nx_ip_raw_packet_send(NX_IP *ip_ptr,
            NX_PACKET *packet_ptr,
            ULONG destination_ip, ULONG type_of_service);
```
```c
UINT     nx_ip_static_route_add(NX_IP *ip_ptr, ULONG
            network_address, ULONG net_mask, ULONG next_hop);
```
```c
UINT     nx_ip_static_route_delete(NX_IP *ip_ptr, ULONG
            network_address, ULONG net_mask);
```
```c
UINT     nx_ip_status_check(NX_IP *ip_ptr, ULONG needed_status,
            ULONG *actual_status, ULONG wait_option);
```
```c
UINT     nxd_ipv6_default_router_add(NX_IP *ip_ptr, NXD_ADDRESS
            *router_address, ULONG router_lifetime, UINT
            if_index)
```
```c
UINT     nxd_ipv6_default_router_delete(NX_IP *ip_ptr,
            NXD_ADDRESS *router_address)
```
```c
UINT     nxd_ipv6_default_router_get(NX_IP *ip_ptr, UINT
            if_index, NXD_ADDRESS *router_address, ULONG
            *router_lifetime, ULONG *prefix_length)
```
```c
UINT     nxd_ipv6_enable(NX_IP *ip_ptr)
```
```c
UINT     nxd_ip_raw_packet_send(NX_IP *ip_ptr, NX_PACKET
            *packet_ptr, NXD_ADDRESS *destination_ip, ULONG
            protocol)
```
```c
UINT     nxd_ip_raw_packet_interface_send(NX_IP *ip_ptr,
            NX_PACKET *packet_ptr, NXD_ADDRESS *destination_ip,
            UINT if_index, ULONG protocol);
```
```c
UINT     nxd_ipv6_address_delete(NX_IP *ip_ptr, UINT
            address_index);
```
```c
UINT     nxd_ipv6_address_get(NX_IP *ip_ptr, UINT address_index,
            NXD_ADDRESS *ip_address, ULONG *prefix_length, UINT
            *if_index);
```
```c
UINT     nxd_ipv6_address_set(UINT nxd_ipv6_address_set(NX_IP
            *ip_ptr, UINT address_index);
```

## Neighbor Discovery  

```c
UINT     nxd_nd_cache_entry_delete(NX_IP ip_ptr, ULONG
            *ip_address)
```
```c
UINT     nxd_nd_cache_entry_set(NX_IP *ip_ptr, ULONG
            *ip_address, char *mac)
```
```c
UINT     nxd_nd_cache_hardware_address_find(NX_IP *ip_ptr,
            NXD_ADDRESS *ip_address, ULONG *physical_msw,
            ULONG *physical_lsw)
```
```c
UINT     nxd_nd_cache_invalidate(NX_IP *ip_ptr)
```
```c
UINT     nxd_nd_cache_ip_address_find(NX_IP *ip_ptr, NXD_ADDRESS
            *ip_address,ULONG physical_msw, ULONG physical_lsw,
            UINT *if_index)
```

## Packet Management  

```c
UINT     nx_packet_allocate(NX_PACKET_POOL *pool_ptr,
			   NX_PACKET **packet_ptr, ULONG packet_type,
			   ULONG wait_option);
```
```c
UINT     nx_packet_copy(NX_PACKET *packet_ptr,
			   NX_PACKET **new_packet_ptr, NX_PACKET_POOL
			   *pool_ptr,
			   ULONG wait_option);
```
```c
UINT     nx_packet_data_append(NX_PACKET *packet_ptr,
			   VOID *data_start, ULONG data_size,
			   NX_PACKET_POOL *pool_ptr, ULONG wait_option);
```
```c
UINT     nx_packet_data_extract_offset(NX_PACKET *packet_ptr,
			   ULONG offset, VOID *buffer_start, ULONG
			   buffer_length, ULONG *bytes_copied);
```
```c
UINT     nx_packet_data_retrieve(NX_PACKET *packet_ptr,
			   VOID *buffer_start, ULONG *bytes_copied);
```                  
```c
UINT     nx_packet_length_get(NX_PACKET *packet_ptr, ULONG
			   *length);
```         
```c
UINT     nx_packet_pool_create(NX_PACKET_POOL *pool_ptr,
			   CHAR *name, ULONG block_size, VOID *memory_ptr,
			   ULONG memory_size);
```         
```c
UINT     nx_packet_pool_delete(NX_PACKET_POOL *pool_ptr);
```
```c
UINT     nx_packet_pool_info_get(NX_PACKET_POOL *pool_ptr, ULONG
    			*total_packets, ULONG *free_packets,
    			ULONG *empty_pool_requests,
    			ULONG *empty_pool_suspensions,
    			ULONG *invalid_packet_releases);
```         
```c
UINT     nx_packet_release(NX_PACKET *packet_ptr);
```
```c
UINT     nx_packet_transmit_release(NX_PACKET *packet_ptr);
```

## Reverse Address Resolution Protocol (RARP)  

```c
UINT     nx_rarp_disable(NX_IP *ip_ptr);
```
```c
UINT     nx_rarp_enable(NX_IP *ip_ptr);
```
```c
UINT     nx_rarp_info_get(NX_IP *ip_ptr,
				ULONG *rarp_requests_sent,
				ULONG *rarp_responses_received,
				ULONG *rarp_invalid_messages);
```

## System Management  

```c
VOID     nx_system_initialize(VOID);
```

## Transmission Control Protocol (TCP)  

```c
UINT     nx_tcp_client_socket_bind(NX_TCP_SOCKET *socket_ptr,
				UINT port, ULONG wait_option);
```
```c
UINT     nx_tcp_client_socket_connect(NX_TCP_SOCKET
				*socket_ptr, ULONG server_ip, UINT server_port,
				ULONG wait_option);
```
```c
UINT     nx_tcp_client_socket_port_get(NX_TCP_SOCKET
				*socket_ptr, UINT *port_ptr);
```
```c
UINT     nx_tcp_client_socket_unbind(NX_TCP_SOCKET
				*socket_ptr);
```
```c
UINT     nx_tcp_enable(NX_IP *ip_ptr);
```
```c
UINT     nx_tcp_free_port_find(NX_IP *ip_ptr, UINT port,
				UINT *free_port_ptr);
```            
```c
UINT     nx_tcp_info_get(NX_IP *ip_ptr, ULONG *tcp_packets_sent,
				ULONG *tcp_bytes_sent, ULONG *tcp_packets_received,
				ULONG *tcp_bytes_received, ULONG
				*tcp_invalid_packets, ULONG
				*tcp_receive_packets_dropped,
				ULONG *tcp_checksum_errors,ULONG *tcp_connections,
				ULONG *tcp_disconnections,
				ULONG *tcp_connections_dropped,
				ULONG*tcp_retransmit_packets);
```
```c
UINT     nx_tcp_server_socket_accept(NX_TCP_SOCKET *socket_ptr,
				ULONG wait_option);
```
```c
UINT     nx_tcp_server_socket_listen(NX_IP *ip_ptr,
				UINT port, NX_TCP_SOCKET *socket_ptr,
				UINT listen_queue_size,
				VOID (*tcp_listen_callback)(NX_TCP_SOCKET
				*socket_ptr, UINT port));
```
```c
UINT     nx_tcp_server_socket_relisten(NX_IP *ip_ptr,
				UINT port, NX_TCP_SOCKET *socket_ptr);
```
```c
UINT     nx_tcp_server_socket_unaccept(NX_TCP_SOCKET
				*socket_ptr);
```
```c
UINT     nx_tcp_server_socket_unlisten(NX_IP *ip_ptr, UINT
				port);
```
```c
UINT     nx_tcp_socket_bytes_available(NX_TCP_SOCKET
				*socket_ptr, ULONG *bytes_available);
```
```c
UINT     nx_tcp_socket_create(NX_IP *ip_ptr,
				NX_TCP_SOCKET *socket_ptr, CHAR *name,
				ULONG type_of_service, ULONG fragment,
				UINT time_to_live, ULONG window_size,
				VOID (*tcp_urgent_data_callback)(NX_TCP_SOCKET
				*socket_ptr),
				VOID (*tcp_disconnect_callback)(NX_TCP_SOCKET
				*socket_ptr));
```
```c
UINT     nx_tcp_socket_delete(NX_TCP_SOCKET *socket_ptr);
```  
```c
UINT     nx_tcp_socket_disconnect(NX_TCP_SOCKET *socket_ptr,
				ULONG wait_option);
```
```c
UINT     nx_tcp_socket_info_get(NX_TCP_SOCKET *socket_ptr,
				ULONG *tcp_packets_sent, ULONG *tcp_bytes_sent,
				ULONG *tcp_packets_received, ULONG
				*tcp_bytes_received,
				ULONG *tcp_retransmit_packets, ULONG
				*tcp_packets_queued,
				ULONG *tcp_checksum_errors, ULONG *tcp_socket_state,
				ULONG *tcp_transmit_queue_depth, ULONG
				*tcp_transmit_window,
				ULONG *tcp_receive_window);
```
```c
UINT     nx_tcp_socket_mss_get(NX_TCP_SOCKET *socket_ptr,
				ULONG *mss);
```
```c
UINT     nx_tcp_socket_mss_peer_get(NX_TCP_SOCKET *socket_ptr,
				ULONG *peer_mss);
```                                                
```c
UINT     nx_tcp_socket_mss_set(NX_TCP_SOCKET *socket_ptr,
				ULONG mss);
```
```c
UINT     nx_tcp_socket_peer_info_get(NX_TCP_SOCKET *socket_ptr,
				ULONG *peer_ip_address, ULONG *peer_port);
```
```c
UINT     nx_tcp_socket_receive(NX_TCP_SOCKET *socket_ptr,
				NX_PACKET **packet_ptr, ULONG wait_option);
```
```c
UINT     nx_tcp_socket_receive_notify(NX_TCP_SOCKET
				*socket_ptr, VOID
				(*tcp_receive_notify)(NX_TCP_SOCKET *socket_ptr));
```
```c
UINT     nx_tcp_socket_send(NX_TCP_SOCKET *socket_ptr,
				NX_PACKET *packet_ptr, ULONG wait_option);
```
```c
UINT     nx_tcp_socket_state_wait(NX_TCP_SOCKET *socket_ptr,
				UINT desired_state, ULONG wait_option);
```
```c
UINT     nx_tcp_socket_transmit_configure(NX_TCP_SOCKET
				*socket_ptr, ULONG max_queue_depth, ULONG timeout,
				ULONG max_retries, ULONG timeout_shift);
```
```c
UINT     nx_tcp_socket_window_update_notify_set
				(NX_TCP_SOCKET *socket_ptr,
				VOID (*tcp_window_update_notify)
				(NX_TCP_SOCKET *socket_ptr));
```
```c
UINT     nxd_tcp_client_socket_connect(NX_TCP_SOCKET
				*socket_ptr, NXD_ADDRESS *server_ip, UINT
				server_port, ULONG wait_option)
```
```c
UINT     nxd_tcp_socket_peer_info_get(NX_TCP_SOCKET
				*socket_ptr, NXD_ADDRESS *peer_ip_address,
				ULONG *peer_port)
```                                                                        

## User Datagram Protocol (UDP)  

```c
UINT     nx_udp_enable(NX_IP *ip_ptr);
```
```c
UINT     nx_udp_free_port_find(NX_IP *ip_ptr, UINT port,
				UINT *free_port_ptr);
```
```c
UINT     nx_udp_info_get(NX_IP *ip_ptr, ULONG *udp_packets_sent,
				ULONG *udp_bytes_sent, ULONG *udp_packets_received,
				ULONG *udp_bytes_received,
				ULONG *udp_invalid_packets,
				ULONG *udp_receive_packets_dropped,
				ULONG *udp_checksum_errors);
```
```c
UINT     nx_udp_packet_info_extract(NX_PACKET *packet_ptr,
				ULONG *ip_address, UINT *protocol, UINT *port,
				UINT *interface_index);
```                                    
```c
UINT     nx_udp_socket_bind(NX_UDP_SOCKET *socket_ptr,
				UINT port, ULONG wait_option);
```
```c
UINT     nx_udp_socket_bytes_available(NX_UDP_SOCKET
				*socket_ptr, ULONG *bytes_available);
```
```c
UINT     nx_udp_socket_checksum_disable(NX_UDP_SOCKET
				*socket_ptr);
```
```c
UINT     nx_udp_socket_checksum_enable(NX_UDP_SOCKET
				*socket_ptr);
```
```c
UINT     nx_udp_socket_create(NX_IP *ip_ptr, NX_UDP_SOCKET
				*socket_ptr, CHAR *name, ULONG type_of_service,
				ULONG fragment,
				UINT time_to_live, ULONG queue_maximum);
```
```c
UINT     nx_udp_socket_delete(NX_UDP_SOCKET *socket_ptr);
```
```c
UINT     nx_udp_socket_info_get(NX_UDP_SOCKET *socket_ptr,
				ULONG *udp_packets_sent, ULONG *udp_bytes_sent,
				ULONG *udp_packets_received, ULONG
				*udp_bytes_received,
				ULONG *udp_packets_queued,
				ULONG *udp_receive_packets_dropped,
				ULONG *udp_checksum_errors);
```
```c
UINT     nx_udp_socket_interface_send(NX_UDP_SOCKET
				*socket_ptr, NX_PACKET *packet_ptr, ULONG
				ip_address, UINT port, UINT address_index);
```
```c
UINT     nx_udp_socket_port_get(NX_UDP_SOCKET *socket_ptr,
				UINT *port_ptr);
```
```c
UINT     nx_udp_socket_receive(NX_UDP_SOCKET *socket_ptr,
				NX_PACKET **packet_ptr, ULONG wait_option);
```
```c
UINT     nx_udp_socket_receive_notify(NX_UDP_SOCKET
				*socket_ptr, VOID
				(*udp_receive_notify)(NX_UDP_SOCKET *socket_ptr));
```
```c
UINT     nx_udp_socket_send(NX_UDP_SOCKET *socket_ptr,
				NX_PACKET *packet_ptr, ULONG ip_address, UINT port);
```
```c
UINT     nx_udp_socket_unbind(NX_UDP_SOCKET *socket_ptr);
```
```c
UINT     nx_udp_source_extract(NX_PACKET *packet_ptr,
				ULONG *ip_address, UINT *port);
```
```c
UINT     nxd_udp_packet_info_extract(NX_PACKET *packet_ptr,
				NXD_ADDRESS *ip_address, UINT *protocol, UINT *port,
				UINT *interface_index);
```
```c
UINT     nxd_udp_source_extract (NX_PACKET *packet_ptr,
				NXD_ADDRESS *ip_address, UINT *port)
```
```c
UINT     nxd_udp_socket_interface_send(NX_UDP_SOCKET
				*socket_ptr, NX_PACKET *packet_ptr, NXD_ADDRESS
				*ip_address, UINT port, UINT address_index)
```
```c
UINT     nxd_udp_socket_send(NX_UDP_SOCKET *socket_ptr,
				NX_PACKET *packet_ptr, NXD_ADDRESS *ip_address,
				UINT port)
```