---
title: Appendix C - NetX Duo Data Types
description: Learn about the NetX Duo Data Types.
---

# Appendix C - NetX Duo Data Types

## NX_ARP

```c
typedef struct NX_ARP_STRUCT
{
    UINT                                 nx_arp_route_static;
    UINT                                 nx_arp_entry_next_update;
    UINT                                 nx_arp_retries;
    struct NX_ARP_STRUCT                 *nx_arp_pool_next,
                                         *nx_arp_pool_previous;
    struct NX_ARP_STRUCT                 *nx_arp_active_next,
                                         *nx_arp_active_previous,
                                         **nx_arp_active_list_head;
    ULONG                                nx_arp_ip_address;
    ULONG                                nx_arp_physical_address_msw;
    ULONG                                nx_arp_physical_address_lsw;
    struct NX_INTERFACE_STRUCT           *nx_arp_ip_interface;
    struct NX_PACKET_STRUCT              *nx_arp_packets_waiting;
}
```
## NX_INTERFACE;

```c
typedef struct NX_INTERFACE_STRUCT
{
    CHAR                                 *nx_interface_name;
    UCHAR                                nx_interface_valid;
    UCHAR                                nx_interface_address_mapping_needed;
    UCHAR                                nx_interface_link_up;
    UCHAR                                nx_interface_index;
    UCHAR                                nx_interface_link_status_change;
    UCHAR                                nx_interface_reserved[3];
    struct NX_IP_STRUCT                  *nx_interface_ip_instance;
    ULONG                                nx_interface_physical_address_msw;
    ULONG                                nx_interface_physical_address_lsw;
    ULONG                                nx_interface_ip_address;
    ULONG                                nx_interface_ip_network_mask;
    ULONG                                nx_interface_ip_network;
    struct NXD_IPV6_ADDRESS_STRUCT       *nxd_interface_ipv6_address_list_head;
    ULONG                                nx_interface_ip_mtu_size;
    #ifndef NX_DISABLE_ICMPV6_ROUTER_SOLICITATION
    ULONG                                nx_ipv6_rtr_solicitation_max;
    ULONG                                nx_ipv6_rtr_solicitation_count;
    ULONG                                nx_ipv6_rtr_solicitation_interval;
    ULONG                                nx_ipv6_rtr_solicitation_timer;
#endif /* NX_DISABLE_ICMPV6_ROUTER_SOLICITATION */

#ifdef NX_IPV6_STATELESS_AUTOCONFIG_CONTROL
    ULONG                                nx_ipv6_stateless_address_autoconfig_status;
#endif /* NX_IPV6_STATELESS_AUTOCONFIG_CONTROL   */
    VOID                                 *nx_interface_additional_link_info;
    VOID                                 (*nx_interface_link_driver_entry)
                                                (struct NX_IP_DRIVER_STRUCT *);
#ifdef NX_ENABLE_INTERFACE_CAPABILITY
    ULONG                                nx_interface_capability_flag;
#endif /* NX_ENABLE_INTERFACE_CAPABILITY */
    ULONG                                nx_interface_arp_defend_timeout;
    CHAR                                 *nx_interface_name;
    UCHAR                                nx_interface_valid;
    UCHAR                                nx_interface_address_mapping_needed;
    UCHAR                                nx_interface_link_up;
    UCHAR                                reserved;
    struct NX_IP_STRUCT                  *nx_interface_ip_instance;
    ULONG                                nx_interface_physical_address_msw;
    ULONG                                nx_interface_physical_address_lsw;
    ULONG                                nx_interface_ip_address;
    ULONG                                nx_interface_ip_network_mask;
    ULONG                                nx_interface_ip_network;
    struct NXD_IPV6_ADDRESS_STRUCT       *nxd_interface_ipv6_address_list_head;
    ULONG                                nx_interface_ip_mtu_size;
    VOID                                 *nx_interface_additional_link_info;
    VOID                                 (*nx_interface_link_driver_entry)
                                                (struct NX_IP_DRIVER_STRUCT *);
}
```
## NX_IP

```c
typedef struct NX_IP_STRUCT
{
    ULONG                                       nx_ip_id;
    CHAR                                        *nx_ip_name;

    #define nx_ip_address                       nx_ip_interface[0].nx_interface_ip_address
    #define nx_ip_driver_mtu                    nx_ip_interface[0].nx_interface_ip_mtu_size
    #define nx_ip_driver_mapping_needed         nx_ip_interface[0].nx_interface_address_mapping_needed
    #define nx_ip_network_mask                  nx_ip_interface[0].nx_interface_ip_network_mask
    #define nx_ip_network                       nx_ip_interface[0].nx_interface_ip_network
    #define nx_ip_arp_physical_address_msw      nx_ip_interface[0].nx_interface_physical_address_msw
    #define nx_ip_arp_physical_address_lsw      nx_ip_interface[0].nx_interface_physical_address_lsw
    #define nx_ip_driver_link_up                nx_ip_interface[0].nx_interface_link_up
    #define nx_ip_link_driver_entry             nx_ip_interface[0].nx_interface_link_driver_entry
    #define nx_ip_additional_link_info          nx_ip_interface[0].nx_interface_additional_link_info

    ULONG                                       nx_ip_gateway_address;
    struct NX_INTERFACE_STRUCT                  *nx_ip_gateway_interface;
#ifdef FEATURE_NX_IPV6
    struct NXD_IPV6_ADDRESS_STRUCT              nx_ipv6_address[NX_MAX_IPV6_ADDRESSES];
#endif /* FEATURE_NX_IPV6 */
    ULONG                                       nx_ip_total_packet_send_requests;
    ULONG                                       nx_ip_total_packets_sent;
    ULONG                                       nx_ip_total_bytes_sent;
    ULONG                                       nx_ip_total_packets_received;
    ULONG                                       nx_ip_total_packets_delivered;
    ULONG                                       nx_ip_total_bytes_received;
    ULONG                                       nx_ip_packets_forwarded;
    ULONG                                       nx_ip_packets_reassembled;
    ULONG                                       nx_ip_reassembly_failures;
    ULONG                                       nx_ip_invalid_packets;
    ULONG                                       nx_ip_invalid_transmit_packets;
    ULONG                                       nx_ip_invalid_receive_address;
    ULONG                                       nx_ip_unknown_protocols_received;
    ULONG                                       nx_ip_transmit_resource_errors;
    ULONG                                       nx_ip_transmit_no_route_errors;
    ULONG                                       nx_ip_receive_packets_dropped;
    ULONG                                       nx_ip_receive_checksum_errors;
    ULONG                                       nx_ip_send_packets_dropped;
    ULONG                                       nx_ip_total_fragment_requests;
    ULONG                                       nx_ip_successful_fragment_requests;
    ULONG                                       nx_ip_fragment_failures;
    ULONG                                       nx_ip_total_fragments_sent;
    ULONG                                       nx_ip_total_fragments_received;
    ULONG                                       nx_ip_arp_requests_sent;
    ULONG                                       nx_ip_arp_requests_received;
    ULONG                                       nx_ip_arp_responses_sent;
    ULONG                                       nx_ip_arp_responses_received;
    ULONG                                       nx_ip_arp_aged_entries;
    ULONG                                       nx_ip_arp_invalid_messages;
    ULONG                                       nx_ip_arp_static_entries;
    ULONG                                       nx_ip_udp_packets_sent;
    ULONG                                       nx_ip_udp_bytes_sent;
    ULONG                                       nx_ip_udp_packets_received;
    ULONG                                       nx_ip_udp_bytes_received;
    ULONG                                       nx_ip_udp_invalid_packets;
    ULONG                                       nx_ip_udp_no_port_for_delivery;
    ULONG                                       nx_ip_udp_receive_packets_dropped;
    ULONG                                       nx_ip_udp_checksum_errors;
    ULONG                                       nx_ip_tcp_packets_sent;
    ULONG                                       nx_ip_tcp_bytes_sent;
    ULONG                                       nx_ip_tcp_packets_received;
    ULONG                                       nx_ip_tcp_bytes_received;
    ULONG                                       nx_ip_tcp_invalid_packets;
    ULONG                                       nx_ip_tcp_receive_packets_dropped;
    ULONG                                       nx_ip_tcp_checksum_errors;
    ULONG                                       nx_ip_tcp_connections;
    ULONG                                       nx_ip_tcp_passive_connections;
    ULONG                                       nx_ip_tcp_active_connections;
    ULONG                                       nx_ip_tcp_disconnections;
    ULONG                                       nx_ip_tcp_connections_dropped;
    ULONG                                       nx_ip_tcp_retransmit_packets;
    ULONG                                       nx_ip_tcp_resets_received;
    ULONG                                       nx_ip_tcp_resets_sent;
    ULONG                                       nx_ip_icmp_total_messages_received;
    ULONG                                       nx_ip_icmp_checksum_errors;
    ULONG                                       nx_ip_icmp_invalid_packets;
    ULONG                                       nx_ip_icmp_unhandled_messages;
    ULONG                                       nx_ip_pings_sent;
    ULONG                                       nx_ip_ping_timeouts;
    ULONG                                       nx_ip_ping_threads_suspended;
    ULONG                                       nx_ip_ping_responses_received;
    ULONG                                       nx_ip_pings_received;
    ULONG                                       nx_ip_pings_responded_to;
    ULONG                                       nx_ip_igmp_invalid_packets;
    ULONG                                       nx_ip_igmp_reports_sent;
    ULONG                                       nx_ip_igmp_queries_received;
    ULONG                                       nx_ip_igmp_checksum_errors;
    ULONG                                       nx_ip_igmp_groups_joined;
#ifndef NX_DISABLE_IGMPV2
    ULONG                                       nx_ip_igmp_router_version;
#endif
    ULONG                                       nx_ip_rarp_requests_sent;
    ULONG                                       nx_ip_rarp_responses_received;
    ULONG                                       nx_ip_rarp_invalid_messages;
    VOID                                        (*nx_ip_forward_packet_process)
                                                        (struct NX_IP_STRUCT *, NX_PACKET *);
#ifdef NX_NAT_ENABLE
    UINT                                        (*nx_ip_nat_packet_process)(struct NX_IP_STRUCT *,
                                                                           NX_PACKET *);
    UINT                                        (*nx_ip_nat_port_verify)(struct NX_IP_STRUCT *, UINT
                                                                        protocol, UINT port);
#endif
    ULONG                                       nx_ip_packet_id;
    struct NX_PACKET_POOL_STRUCT                *nx_ip_default_packet_pool;
#ifdef NX_ENABLE_DUAL_PACKET_POOL
    struct NX_PACKET_POOL_STRUCT                *nx_ip_auxiliary_packet_pool;
#endif /* NX_ENABLE_DUAL_PACKET_POOL */
    TX_MUTEX                                    nx_ip_protection;
    UINT                                        nx_ip_initialize_done;
    NX_PACKET                                   *nx_ip_driver_deferred_packet_head,
                                                *nx_ip_driver_deferred_packet_tail;
    VOID                                        (*nx_ip_driver_deferred_packet_handler)(struct
                                                         NX_IP_STRUCT *, NX_PACKET *);
    NX_PACKET                                   *nx_ip_deferred_received_packet_head,
                                                *nx_ip_deferred_received_packet_tail;
    UINT                                        (*nx_ip_raw_ip_processing)(struct NX_IP_STRUCT *,
                                                                           ULONG, NX_PACKET *);
#ifdef NX_ENABLE_IP_RAW_PACKET_FILTER
    UINT                                        (*nx_ip_raw_packet_filter)(struct NX_IP_STRUCT *,
                                                                           ULONG, NX_PACKET *);
#endif /* NX_ENABLE_IP_RAW_PACKET_FILTER */
    NX_PACKET                                   *nx_ip_raw_received_packet_head,
                                                *nx_ip_raw_received_packet_tail;
    ULONG                                       nx_ip_raw_received_packet_count;
    ULONG                                       nx_ip_raw_received_packet_max;
    TX_THREAD                                   *nx_ip_raw_packet_suspension_list;
    ULONG                                       nx_ip_raw_packet_suspended_count;
    TX_THREAD                                   nx_ip_thread;
    TX_EVENT_FLAGS_GROUP                        nx_ip_events;
    TX_TIMER                                    nx_ip_periodic_timer;
    VOID                                        (*nx_ip_fragment_processing)(struct
                                                            NX_IP_DRIVER_STRUCT *);
    VOID                                        (*nx_ip_fragment_assembly)(struct NX_IP_STRUCT *);
    VOID                                        (*nx_ip_fragment_timeout_check)
                                                            (struct NX_IP_STRUCT *);
    NX_PACKET                                   *nx_ip_timeout_fragment;
    NX_PACKET                                   *nx_ip_received_fragment_head,
                                                *nx_ip_received_fragment_tail;
    NX_PACKET                                   *nx_ip_fragment_assembly_head,
                                                *nx_ip_fragment_assembly_tail;
    VOID                                        (*nx_ip_address_change_notify)(struct NX_IP_STRUCT *,
                                                                              VOID *);
    VOID                                        *nx_ip_address_change_notify_additional_info;

#ifdef FEATURE_NX_IPV6
#ifdef NX_ENABLE_IPV6_ADDRESS_CHANGE_NOTIFY
    VOID                                        (*nx_ipv6_address_change_notify)(struct NX_IP_STRUCT *,
                                                                                 UINT, UINT, UINT,
                                                                                 ULONG*);
#endif /* NX_ENABLE_IPV6_ADDRESS_CHANGE_NOTIFY */
#endif /* FEATURE_NX_IPV6 */
    NX_IPV4_MULTICAST_ENTRY                     nx_ipv4_multicast_entry[NX_MAX_MULTICAST_GROUPS];
    UINT                                        nx_ip_igmp_global_loopback_enable;
    void                                        (*nx_ip_igmp_packet_receive)(struct NX_IP_STRUCT *,
                                                                           struct NX_PACKET_STRUCT *);
    void                                        (*nx_ip_igmp_periodic_processing)
                                                           (struct NX_IP_STRUCT *);
    void                                        (*nx_ip_igmp_queue_process)(struct NX_IP_STRUCT *);
    NX_PACKET                                   *nx_ip_igmp_queue_head;
    ULONG                                       nx_ip_icmp_sequence;
#ifdef NX_ENABLE_IPV6_MULTICAST
    NX_IPV6_MULTICAST_ENTRY                     nx_ipv6_multicast_entry[NX_MAX_MULTICAST_GROUPS];
    ULONG                                       nx_ipv6_multicast_groups_joined;
#endif /* NX_ENABLE_IPV6_MULTICAST */
    void                                        (*nx_ip_icmp_packet_receive)(struct NX_IP_STRUCT *,
                                                                       struct NX_PACKET_STRUCT *);
    void                                        (*nx_ip_icmp_queue_process)(struct NX_IP_STRUCT *);
    void                                        (*nx_ip_icmpv4_packet_process)(struct NX_IP_STRUCT *,
                                                                              NX_PACKET *);
#ifdef FEATURE_NX_IPV6
    void                                        (*nx_ip_icmpv6_packet_process)(struct NX_IP_STRUCT *,
                                                                              NX_PACKET *);
    void                                        (*nx_icmpv6_process_router_advertisement)(struct
                                                                        NX_IP_STRUCT *, NX_PACKET *)
    void                                        (*nx_nd_cache_fast_periodic_update)(struct
                                                                                    NX_IP_STRUCT *);
    void                                        (*nx_nd_cache_slow_periodic_update)(struct
                                                                                    NX_IP_STRUCT *);
    void                                        (*nx_icmpv6_ra_flag_callback)(struct NX_IP_STRUCT *,
                                                                              UINT);    
#ifdef NX_ENABLE_IPV6_PATH_MTU_DISCOVERY
    void                                        (*nx_destination_table_periodic_update)(struct
                                                                              NX_IP_STRUCT *);
#endif

#endif /* FEATURE_NX_IPV6 */
    NX_PACKET                                   *nx_ip_icmp_queue_head;
    TX_THREAD                                   *nx_ip_icmp_ping_suspension_list;
    ULONG                                       nx_ip_icmp_ping_suspended_count;
    struct NX_UDP_SOCKET_STRUCT                 *nx_ip_udp_port_table[NX_UDP_PORT_TABLE_SIZE];
    struct NX_UDP_SOCKET_STRUCT                 *nx_ip_udp_created_sockets_ptr;
    ULONG                                       nx_ip_udp_created_sockets_count;
    void                                        (*nx_ip_udp_packet_receive)(struct NX_IP_STRUCT *,
                                                                           struct NX_PACKET_STRUCT *);
    UINT                                        nx_ip_udp_port_search_start;
    struct NX_TCP_SOCKET_STRUCT                 *nx_ip_tcp_port_table[NX_TCP_PORT_TABLE_SIZE];
    struct NX_TCP_SOCKET_STRUCT                 *nx_ip_tcp_created_sockets_ptr;
    ULONG                                       nx_ip_tcp_created_sockets_count;
    void                                        (*nx_ip_tcp_packet_receive)(struct NX_IP_STRUCT *,
                                                                           struct NX_PACKET_STRUCT *);
    void                                        (*nx_ip_tcp_periodic_processing)
                                                                           (struct NX_IP_STRUCT *);
    void                                        (*nx_ip_tcp_fast_periodic_processing)(struct
                                                                                      NX_IP_STRUCT *);
    void                                        (*nx_ip_tcp_queue_process)(struct NX_IP_STRUCT *);
    NX_PACKET                                   *nx_ip_tcp_queue_head,
                                                *nx_ip_tcp_queue_tail;
    ULONG                                       nx_ip_tcp_received_packet_count;
    struct NX_TCP_LISTEN_STRUCT                 nx_ip_tcp_server_listen_reqs[NX_MAX_LISTEN_REQUESTS];
    struct NX_TCP_LISTEN_STRUCT                 *nx_ip_tcp_available_listen_requests;
    struct NX_TCP_LISTEN_STRUCT                 *nx_ip_tcp_active_listen_requests;
    UINT                                        nx_ip_tcp_port_search_start;
    UINT                                        nx_ip_fast_periodic_timer_created;
    TX_TIMER                                    nx_ip_fast_periodic_timer;
    struct NX_ARP_STRUCT                        *nx_ip_arp_table[NX_ARP_TABLE_SIZE];
    struct NX_ARP_STRUCT                        *nx_ip_arp_static_list;
    struct NX_ARP_STRUCT                        *nx_ip_arp_dynamic_list;
    ULONG                                       nx_ip_arp_dynamic_active_count;
    NX_PACKET                                   *nx_ip_arp_deferred_received_packet_head,
                                                *nx_ip_arp_deferred_received_packet_tail;
    UINT                                        (*nx_ip_arp_allocate)(struct NX_IP_STRUCT *, struct
                                                                      NX_ARP_STRUCT **, UINT);
    void                                        (*nx_ip_arp_periodic_update)(struct NX_IP_STRUCT *);
    void                                        (*nx_ip_arp_queue_process)(struct NX_IP_STRUCT *);
    void                                        (*nx_ip_arp_packet_send)(struct NX_IP_STRUCT *, ULONG
                                                                         destination_ip, NX_INTERFACE
                                                                         *nx_interface);
    void                                        (*nx_ip_arp_gratuitous_response_handler)(struct
                                                                     NX_IP_STRUCT *, NX_PACKET *);
    void                                        (*nx_ip_arp_collision_notify_response_handler)
    (void *);
    void                                        *nx_ip_arp_collision_notify_parameter;
    ULONG                                       nx_ip_arp_collision_notify_ip_address;
    struct NX_ARP_STRUCT                        *nx_ip_arp_cache_memory;
    ULONG                                       nx_ip_arp_total_entries;
    void                                        (*nx_ip_rarp_periodic_update)(struct NX_IP_STRUCT *);
    void                                        (*nx_ip_rarp_queue_process)(struct NX_IP_STRUCT *);
    NX_PACKET                                   *nx_ip_rarp_deferred_received_packet_head,
                                                *nx_ip_rarp_deferred_received_packet_tail;
    struct NX_IP_STRUCT                         *nx_ip_created_next,
                                                *nx_ip_created_previous;
    void                                        *nx_ip_reserved_ptr;
    void                                        (*nx_tcp_deferred_cleanup_check)
                                                                           (struct NX_IP_STRUCT *);
    NX_INTERFACE                                nx_ip_interface[NX_MAX_IP_INTERFACES];
    void                                        (*nx_ipv4_packet_receive)(struct NX_IP_STRUCT *,
                                                                          NX_PACKET *);
#ifdef NX_ENABLE_IP_STATIC_ROUTING
    NX_IP_ROUTING_ENTRY                         nx_ip_routing_table[NX_IP_ROUTING_TABLE_SIZE];
    ULONG                                       nx_ip_routing_table_entry_count;
#endif /* NX_ENABLE_IP_STATIC_ROUTING */
#ifdef FEATURE_NX_IPV6
    USHORT                                      nx_ipv6_default_router_table_size;
    NX_IPV6_DEFAULT_ROUTER_ENTRY        nx_ipv6_default_router_table[NX_IPV6_DEFAULT_ROUTER_TABLE_SIZE];
    UINT                                        nx_ipv6_default_router_table_round_robin_index;
    NX_IPV6_PREFIX_ENTRY                nx_ipv6_prefix_list_table [NX_IPV6_PREFIX_LIST_TABLE_SIZE];
    NX_IPV6_PREFIX_ENTRY       *nx_ipv6_prefix_list_ptr;
    NX_IPV6_PREFIX_ENTRY       *nx_ipv6_prefix_entry_free_list;

    /* Define the IPv6 packet receive processing routine */
    void                                        (*nx_ipv6_packet_receive)(struct NX_IP_STRUCT *,
                                                                          NX_PACKET *);
    ULONG                                       nx_ipv6_retrans_timer_ticks;
    ULONG                                       nx_ipv6_reachable_timer;
    ULONG                                       nx_ipv6_hop_limit;
#endif /* FEATURE_NX_IPV6 */

#ifdef NX_IPSEC_ENABLE
    UINT                                        (*nx_ip_ipsec_authentication_header_receive)(struct
                                                   NX_IP_STRUCT *, NX_PACKET *, ULONG *, NX_PACKET **);
    UINT                                        (*nx_ip_ipsec_authentication_header_transmit)(struct
                                                   NX_IP_STRUCT *, NX_PACKET **, UINT, UINT);
    UINT                                        (*nx_ip_ipsec_encapsulating_security_payload_receive)
                                                   (struct NX_IP_STRUCT *, NX_PACKET *, ULONG *,
                                                            NX_PACKET **);
    UINT                                        (*nx_ip_ipsec_encapsulating_security_payload_transmit)
                                                   (struct NX_IP_STRUCT *, NX_PACKET **, UINT);
    UINT                                        (*nx_ip_packet_egress_sa_lookup)(struct NX_IP_STRUCT
                                                            *ip_ptr, NXD_ADDRESS *src_address,
                                                            NXD_ADDRESS *dst_address, UCHAR protocol,
                                                            ULONG src_port, ULONG dest_port,
                                                            ULONG *data_offset, VOID **sa_ptr, UINT
                                                            option);
    VOID                                        *nx_ip_ipsec_ingress_sa_ptr;
    VOID                                        *nx_ip_ipsec_egress_sa_ptr;
    VOID                                        *nx_ip_ipsec_ikev2_ptr;
    NX_PACKET                                   *nx_ip_hw_done_packet_header_ptr;
    NX_PACKET                                   *nx_ip_hw_done_packet_tail_ptr;
#endif /* NX_IPSEC_ENABLE */
    VOID                                        (*nx_ip_link_status_change_callback)(struct
                                                                           NX_IP_STRUCT *, UINT, UINT);
#ifdef NX_ENABLE_IP_PACKET_FILTER
    UINT                                        (*nx_ip_packet_filter)(VOID *, UINT);
#endif /* NX_ENABLE_IP_PACKET_FILTER */
}
```
## NX_IP_DRIVER

```c
typedef struct NX_IP_DRIVER_STRUCT
{
    UINT                                         nx_ip_driver_command;
    UINT                                         nx_ip_driver_status;
    ULONG                                        nx_ip_driver_physical_address_msw;
    ULONG                                        nx_ip_driver_physical_address_lsw;
    NX_PACKET                                    *nx_ip_driver_packet;
    ULONG                                        *nx_ip_driver_return_ptr;
    struct NX_IP_STRUCT                          *nx_ip_driver_ptr;
    NX_INTERFACE                                 *nx_ip_driver_interface;
}
```
## NX_IP_ROUTING_ENTRY

```c
typedef struct NX_IP_ROUTING_ENTRY_STRUCT
{
    ULONG                                        nx_ip_routing_dest_ip;
    ULONG                                        nx_ip_routing_net_mask;
    ULONG                                        nx_ip_routing_next_hop_address;
    NX_INTERFACE                                 *nx_ip_routing_entry_ip_interface;
}
```
## NX_IPV6_DEFAULT_ROUTER_ENTRY

```c
typedef struct NX_IPV6_DEFAULT_ROUTER_ENTRY_STRUCT
{
    UCHAR                                        nx_ipv6_default_router_entry_flag;
    UCHAR                                        nx_ipv6_default_router_entry_reserved;
    USHORT                                       nx_ipv6_default_router_entry_life_time;
    ULONG                                        nx_ipv6_default_router_entry_router_address[4];
    struct NX_INTERFACE_STRUCT                   *nx_ipv6_default_router_entry_interface_ptr;
    VOID                                         *nx_ipv6_default_router_entry_neighbor_cache_ptr;
} 
#endif /* FEATURE_NX_IPV6 */
```
## NX_IPV6_PREFIX_ENTRY

```c
typedef struct NX_IPV6_PREFIX_ENTRY_STRUCT
{
ULONG                                           nx_ipv6_prefix_entry_network_address[4];
ULONG                                           nx_ipv6_prefix_entry_prefix_length;
ULONG                                           nx_ipv6_prefix_entry_valid_lifetime;
struct NX_IPV6_PREFIX_ENTRY_STRUCT              * nx_ipv6_prefix_entry_prev;
struct NX_IPV6_PREFIX_ENTRY_STRUCT              * nx_ipv6_prefix_entry_next;
} 
```
## NX_PACKET

```c
typedef struct NX_PACKET_STRUCT
{
   struct NX_PACKET_POOL_STRUCT                 *nx_packet_pool_owner;
#ifndef NX_DISABLE_PACKET_CHAIN
struct NX_PACKET_STRUCT                         *nx_packet_next;
#endif /* NX_DISABLE_PACKET_CHAIN */
UCHAR                                           *nx_packet_prepend_ptr;
UCHAR                                           *nx_packet_append_ptr;
UCHAR                                           *nx_packet_data_start;
UCHAR                                           *nx_packet_data_end;
#ifndef NX_DISABLE_PACKET_CHAIN
struct NX_PACKET_STRUCT                         *nx_packet_last;
struct NX_PACKET_STRUCT                         *nx_packet_queue_next;
union
{
   struct NX_PACKET_STRUCT                      *nx_packet_tcp_queue_next;

#ifndef NX_DISABLE_FRAGMENTATION
        struct NX_PACKET_STRUCT                 *nx_packet_fragment_next;
#endif /* NX_DISABLE_FRAGMENTATION */
} nx_packet_union_next;
    ULONG                                       nx_packet_length;
#ifndef NX_DISABLE_FRAGMENTATION
    ULONG                                       nx_packet_reassembly_time;
#endif /* NX_DISABLE_FRAGMENTATION */
#ifdef FEATURE_NX_IPV6
    UCHAR                                       nx_packet_option_state;
    UCHAR                                       nx_packet_destination_header;
    USHORT                                      nx_packet_option_offset;
#endif /* FEATURE_NX_IPV6 */
    UCHAR                                       nx_packet_ip_version;
    UCHAR                                       nx_packet_identical_copy;
    UCHAR                                       nx_packet_reserved[2];
    union
    {
         struct NX_INTERFACE_STRUCT            *nx_packet_interface_ptr;
         struct NXD_IPV6_ADDRESS_STRUCT        *nx_packet_ipv6_address_ptr;
    } nx_packet_address;

    #define nx_packet_ip_interface nx_packet_address.nx_packet_interface_ptr
    
    UCHAR                                      *nx_packet_ip_header;

#ifdef NX_ENABLE_INTERFACE_CAPABILITY
    ULONG                                      nx_packet_interface_capability_flag;
#endif /* NX_ENABLE_INTERFACE_CAPABILITY */
#ifdef NX_IPSEC_ENABLE
    VOID                                       *nx_packet_ipsec_sa_ptr;
    USHORT                                     nx_packet_ipsec_op;
    USHORT                                     nx_packet_ipsec_state;
#endif /* NX_IPSEC_ENABLE */
#ifdef NX_ENABLE_PACKET_DEBUG_INFO
    CHAR                                       *nx_packet_debug_thread;
    CHAR                                       *nx_packet_debug_file;
    ULONG                                      nx_packet_debug_line;
#endif /* NX_ENABLE_PACKET_DEBUG_INFO */

#ifdef NX_PACKET_HEADER_PAD
    ULONG                                      nx_packet_packet_pad[NX_PACKET_HEADER_PAD_SIZE];
#endif
    struct NX_PACKET_POOL_STRUCT               *nx_packet_pool_owner;
    struct NX_PACKET_STRUCT                    *nx_packet_queue_next;
    struct NX_PACKET_STRUCT                    *nx_packet_tcp_queue_next;
    struct NX_PACKET_STRUCT                    *nx_packet_next;
    struct NX_PACKET_STRUCT                    *nx_packet_last;
    struct NX_PACKET_STRUCT                    *nx_packet_fragment_next;
    ULONG                                      nx_packet_length;
    struct NX_INTERFACE_STRUCT                 *nx_packet_ip_interface;
    ULONG                                      nx_packet_next_hop_address;
    UCHAR                                      *nx_packet_data_start;
    UCHAR                                      *nx_packet_data_end;
    UCHAR                                      *nx_packet_prepend_ptr;
    UCHAR                                      *nx_packet_append_ptr;
#ifdef NX_PACKET_HEADER_PAD
    ULONG                                      nx_packet_packet_pad;
#endif
    ULONG                                      nx_packet_reassembly_time;
    UCHAR                                      nx_packet_option_state;
    UCHAR                                      nx_packet_destination_header;
    USHORT                                     nx_packet_option_offset;
    ULONG                                      nx_packet_ip_version;
#ifdef FEATURE_NX_IPV6
    ULONG                                      nx_packet_ipv6_dest_addr[4];
    ULONG                                      nx_packet_ipv6_src_addr[4];
    struct                                     NXD_IPV6_ADDRESS_STRUCT *nx_packet_interface;
#endif /* FEATURE_NX_IPV6 */
    UCHAR                                      *nx_packet_ip_header;
}
```
## NX_PACKET_POOL

```c
typedef struct NX_PACKET_POOL_STRUCT
{
    ULONG                                      nx_packet_pool_id;
    CHAR                                       *nx_packet_pool_name;
    ULONG                                      nx_packet_pool_available;
    ULONG                                      nx_packet_pool_total;
    ULONG                                      nx_packet_pool_empty_requests;
    ULONG                                      nx_packet_pool_empty_suspensions;
    ULONG                                      nx_packet_pool_invalid_releases;
    struct NX_PACKET_STRUCT                    *nx_packet_pool_available_list;
    CHAR                                       *nx_packet_pool_start;
    ULONG                                      nx_packet_pool_size;
    ULONG                                      nx_packet_pool_payload_size;
    TX_THREAD                                  *nx_packet_pool_suspension_list;
    ULONG                                      nx_packet_pool_suspended_count;
    struct NX_PACKET_POOL_STRUCT               *nx_packet_pool_created_next,
                                               *nx_packet_pool_created_previous;
#ifdef NX_ENABLE_LOW_WATERMARK
    UINT                                       nx_packet_pool_low_watermark;
#endif /* NX_ENABLE_LOW_WATERMARK */
}
```
## NX_TCP_LISTEN

```c
typedef struct NX_TCP_LISTEN_STRUCT
{
    UINT                                        nx_tcp_listen_port;
    VOID                                        (*nx_tcp_listen_callback)(NX_TCP_SOCKET *socket_ptr,
                                                                          UINT port);
    NX_TCP_SOCKET                               *nx_tcp_listen_socket_ptr;
    ULONG                                       nx_tcp_listen_queue_maximum;
    ULONG                                       nx_tcp_listen_queue_current;
    NX_PACKET                                   *nx_tcp_listen_queue_head,
                                                *nx_tcp_listen_queue_tail;
    struct NX_TCP_LISTEN_STRUCT                 *nx_tcp_listen_next,
                                                *nx_tcp_listen_previous;
}
```
## NX_TCP_SOCKET

```c
typedef struct NX_TCP_SOCKET_STRUCT
{
    ULONG                                       nx_tcp_socket_id;
    CHAR                                        *nx_tcp_socket_name;
    UINT                                        nx_tcp_socket_client_type;
    UINT                                        nx_tcp_socket_port;
    ULONG                                       nx_tcp_socket_mss;
    NXD_ADDRESS                                 nx_tcp_socket_connect_ip;
    UINT                                        nx_tcp_socket_connect_port;
    ULONG                                       nx_tcp_socket_connect_mss;
    struct NX_INTERFACE_STRUCT                  *nx_tcp_socket_connect_interface;
    ULONG                                       nx_tcp_socket_next_hop_address;
    ULONG                                       nx_tcp_socket_connect_mss2;
    ULONG                                       nx_tcp_socket_tx_slow_start_threshold;
    UINT                                        nx_tcp_socket_state;
    ULONG                                       nx_tcp_socket_tx_sequence;
    ULONG                                       nx_tcp_socket_rx_sequence;
    ULONG                                       nx_tcp_socket_rx_sequence_acked;
    ULONG                                       nx_tcp_socket_delayed_ack_timeout;
    ULONG                                       nx_tcp_socket_fin_sequence;
    USHORT                                      nx_tcp_socket_fin_received;
    USHORT                                      nx_tcp_socket_fin_acked;
    ULONG                                       nx_tcp_socket_tx_window_advertised;
    ULONG                                       nx_tcp_socket_tx_window_congestion;
    ULONG                                       nx_tcp_socket_tx_outstanding_bytes;
    ULONG                                       nx_tcp_socket_tx_sequence_recover;
    ULONG                                       nx_tcp_socket_previous_highest_ack;
    ULONG                                       nx_tcp_socket_ack_n_packet_counter;
    UINT                                        nx_tcp_socket_duplicated_ack_received;
    ULONG                                       nx_tcp_socket_rx_window_default;
    ULONG                                       nx_tcp_socket_rx_window_current;
    ULONG                                       nx_tcp_socket_rx_window_last_sent;
    ULONG                                       nx_tcp_socket_packets_sent;
    ULONG                                       nx_tcp_socket_bytes_sent;
    ULONG                                       nx_tcp_socket_packets_received;
    ULONG                                       nx_tcp_socket_bytes_received;
    ULONG                                       nx_tcp_socket_retransmit_packets;
    ULONG                                       nx_tcp_socket_checksum_errors;
    ULONG                                       nx_tcp_socket_zero_window_probe_failure;
    ULONG                                       nx_tcp_socket_zero_window_probe_sequence;
    UCHAR                                       nx_tcp_socket_zero_window_probe_has_data;
    UCHAR                                       nx_tcp_socket_zero_window_probe_data;
    UCHAR                                       nx_tcp_socket_fast_recovery;
    UCHAR                                       nx_tcp_socket_reserved;
    struct NX_IP_STRUCT                         *nx_tcp_socket_ip_ptr;
    ULONG                                       nx_tcp_socket_type_of_service;
    UINT                                        nx_tcp_socket_time_to_live;
    ULONG                                       nx_tcp_socket_fragment_enable;
    ULONG                                       nx_tcp_socket_receive_queue_count;
    NX_PACKET                                   *nx_tcp_socket_receive_queue_head,
                                                *nx_tcp_socket_receive_queue_tail;
    ULONG                                       nx_tcp_socket_transmit_queue_maximum;
    ULONG                                       nx_tcp_socket_transmit_sent_count;
    NX_PACKET                                   *nx_tcp_socket_transmit_sent_head,
                                                nx_tcp_socket_transmit_sent_tail;
#ifdef NX_ENABLE_LOW_WATERMARK
    ULONG                                       nx_tcp_socket_receive_queue_maximum;
#endif /* NX_ENABLE_LOW_WATERMARK */
    ULONG                                       nx_tcp_socket_timeout;
    ULONG                                       nx_tcp_socket_timeout_rate;
    ULONG                                       nx_tcp_socket_timeout_retries;
    ULONG                                       nx_tcp_socket_timeout_max_retries;
    ULONG                                       nx_tcp_socket_timeout_shift;
#ifdef NX_ENABLE_TCP_WINDOW_SCALING
    ULONG                                       nx_tcp_socket_rx_window_maximum;
    ULONG                                       nx_tcp_rcv_win_scale_value;
    ULONG                                       nx_tcp_snd_win_scale_value;
#endif /* NX_ENABLE_TCP_WINDOW_SCALING */
    ULONG                                       nx_tcp_socket_keepalive_timeout;
    ULONG                                       nx_tcp_socket_keepalive_retries;
    struct NX_TCP_SOCKET_STRUCT                 *nx_tcp_socket_bound_next,
                                                *nx_tcp_socket_bound_previous;
    TX_THREAD                                   *nx_tcp_socket_bind_in_progress;
    TX_THREAD                                   *nx_tcp_socket_receive_suspension_list;
    ULONG                                       nx_tcp_socket_receive_suspended_count;
    TX_THREAD                                   *nx_tcp_socket_transmit_suspension_list;
    ULONG                                       nx_tcp_socket_transmit_suspended_count;
    TX_THREAD                                   *nx_tcp_socket_connect_suspended_thread;
    TX_THREAD                                   *nx_tcp_socket_disconnect_suspended_thread;
    TX_THREAD                                   *nx_tcp_socket_bind_suspension_list;
    ULONG                                       nx_tcp_socket_bind_suspended_count;
    struct NX_TCP_SOCKET_STRUCT                 *nx_tcp_socket_created_next,
                                                *nx_tcp_socket_created_previous;
    VOID                                        (*nx_tcp_urgent_data_callback)(struct
                                                                NX_TCP_SOCKET_STRUCT *socket_ptr);
#ifndef NX_DISABLE_EXTENDED_NOTIFY_SUPPORT
    UINT                                        (*nx_tcp_socket_syn_received_notify)(struct
                                                               NX_TCP_SOCKET_STRUCT *socket_ptr,
                                                               NX_PACKET *packet_ptr);
    VOID                                        (*nx_tcp_establish_notify)(struct NX_TCP_SOCKET_STRUCT
                                                               *socket_ptr);
    VOID                                        (*nx_tcp_disconnect_complete_notify)(struct
                                                               NX_TCP_SOCKET_STRUCT *socket_ptr);
    VOID                                        (*nx_tcp_timed_wait_callback)(struct
                                                               NX_TCP_SOCKET_STRUCT *socket_ptr);
#endif
    VOID                                        (*nx_tcp_disconnect_callback)(struct
                                                               NX_TCP_SOCKET_STRUCT *socket_ptr);
    VOID                                        (*nx_tcp_receive_callback)(struct NX_TCP_SOCKET_STRUCT
                                                               *socket_ptr);
    VOID                                        (*nx_tcp_socket_window_update_notify)(struct
                                                               NX_TCP_SOCKET_STRUCT *socket_ptr);
#ifdef NX_ENABLE_TCP_QUEUE_DEPTH_UPDATE_NOTIFY
    VOID                                        (*nx_tcp_socket_queue_depth_notify)(struct
                                                               NX_TCP_SOCKET_STRUCT *socket_ptr);
#endif
    void                                        *nx_tcp_socket_reserved_ptr;
    ULONG                                       nx_tcp_socket_transmit_queue_maximum_default;
    UINT                                        nx_tcp_socket_keepalive_enabled;
#ifdef FEATURE_NX_IPV6
    struct NXD_IPV6_ADDRESS_STRUCT              *nx_tcp_socket_ipv6_addr;
#endif /* FEATURE_NX_IPV6 */
#ifdef NX_IPSEC_ENABLE
    VOID                                        *nx_tcp_socket_egress_sa;
    UINT                                        nx_tcp_socket_egress_sa_data_offset;
#endif /* NX_IPSEC_ENABLE */
}
```
## NX_UDP_SOCKET

```c
typedef struct NX_UDP_SOCKET_STRUCT
{
    ULONG                                       nx_udp_socket_id;
    CHAR                                        *nx_udp_socket_name;
    UINT                                        nx_udp_socket_port;
    struct NX_IP_STRUCT                         *nx_udp_socket_ip_ptr;
    ULONG                                       nx_udp_socket_packets_sent;
    ULONG                                       nx_udp_socket_bytes_sent;
    ULONG                                       nx_udp_socket_packets_received;
    ULONG                                       nx_udp_socket_bytes_received;
    ULONG                                       nx_udp_socket_invalid_packets;
    ULONG                                       nx_udp_socket_packets_dropped;
    ULONG                                       nx_udp_socket_checksum_errors;
    ULONG                                       nx_udp_socket_type_of_service;
    UINT                                        nx_udp_socket_time_to_live;
    ULONG                                       nx_udp_socket_fragment_enable;
    UINT                                        nx_udp_socket_disable_checksum;
    ULONG                                       nx_udp_socket_receive_count;
    ULONG                                       nx_udp_socket_queue_maximum;
    NX_PACKET                                   *nx_udp_socket_receive_head,
                                                *nx_udp_socket_receive_tail;
    struct NX_UDP_SOCKET_STRUCT                 *nx_udp_socket_bound_next,
                                                *nx_udp_socket_bound_previous;
    TX_THREAD                                   *nx_udp_socket_bind_in_progress;
    TX_THREAD                                   *nx_udp_socket_receive_suspension_list;
    ULONG                                       nx_udp_socket_receive_suspended_count;
    TX_THREAD                                   *nx_udp_socket_bind_suspension_list;
    ULONG                                       nx_udp_socket_bind_suspended_count;
    struct NX_UDP_SOCKET_STRUCT                 *nx_udp_socket_created_next,
                                                *nx_udp_socket_created_previous;
    VOID                                        (*nx_udp_receive_callback)(struct NX_UDP_SOCKET_STRUCT
                                                                          *socket_ptr);
    void                                        *nx_udp_socket_reserved_ptr;
}
```
## NXD_IPV6_ADDRESS

```c
typedef struct NXD_IPV6_ADDRESS_STRUCT
{
    UCHAR                                       nxd_ipv6_address_valid;
    UCHAR                                       nxd_ipv6_address_type;
    UCHAR                                       nxd_ipv6_address_state;
    UCHAR                                       nxd_ipv6_address_prefix_length;
    struct NX_INTERFACE_STRUCT                  *nxd_ipv6_address_attached; 
    ULONG                                       nxd_ipv6_address[4];
    struct                                      NXD_IPV6_ADDRESS_STRUCT *nxd_ipv6_address_next;
    CHAR                                        nxd_ipv6_address_DupAddrDetectTransmit;
    CHAR                                        nxd_ipv6_address_ConfigurationMethod;
    UCHAR                                       nxd_ipv6_address_index;
    UCHAR                                       reserved;
} 
```
## NXD_ADDRESS

```c
typedef struct NXD_ADDRESS_STRUCT
{
    ULONG                                       nxd_ip_version;
    union
    {
         ULONG                                  v4;
#ifdef FEATURE_NX_IPV6
         ULONG                                  v6[4];
#endif
    } nxd_ip_address;
} 
```