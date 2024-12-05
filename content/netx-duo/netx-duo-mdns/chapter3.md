---
title: Chapter 3 - Description of internal service cache
description: 'NetX Duo mDNS module manages two internal services caches: the local service cache, and the peer service cache.'
---


NetX Duo mDNS module manages two internal services caches: the local service cache, and the peer service cache. The local service cache stores Resource Records related to services offered by applications running on the node. For incoming queries, if the question matches the service offered, mDNS responses with answers stored in the local service cache. Applications register services by calling the API *nx_mdns_service_add()*. To remove services, applications use the API *nx_mdns_service_delete()*, which will in turn send "goodbye" messages before removing the corresponding entries in the local service cache.

When a service is added, mDNS maintains at least 3 Resource Records in the local service cache: SRV, PTR, and TXT. Additional PTR Resource Record may be added if the service type includes subtype. For example, an application registers a service:

```
*name*._*subtype*._sub._*type*._tcp.local,
```

two PTR Resource Records are added to the local service cache, one for

```
"*_subtype._*sub*._type._*tcp.local *PTR name.type._*tcp*.*local"
```

and the other one for

```
*"_type._*tcp*.*local *PTR name.type._*tcp*.*local".
```

The peer service cache contains mDNS Resource Records received from neighboring nodes. mDNS module collects Resource Records advertised by other nodes on the network, and stores the received information in the peer service cache. When application queries for information such as host IPv4 or IPv6 addresses, mDNS searches the peer service cache for locally cached responses. When application queries for services offered by peers, mDNS searches through the cache for related PTR, SRV, TXT, and IPv4/IPv6 address records. The peer service cache also stores queries sent to the node. For example, an application may request a particular service by calling *nx_mdns_service_one_shot_query.* If the service is not found in the peer service cache, mDNS creates query questions (PTR, SRV, and TXT) in the peer service cache. These query questions will be sent to the network periodically till the service is resolved, or times out. Similar, the application may use the API *nx_mdns_service_continuous_query()* to request a particular service over a long period of time (application cancels a previously issued continuous query by using the API *nx_mdns_service_query_stop()* ). To search for a particular service in the peer service cache without sending queries to the network, applications can use the API *nx_mdns_service_lookup().* This API only searches the resource records in the peer service cache.

Each Resource Record is stored in a data structure *NX_MDNS_RR* in the service caches. Strings in Resource Records are variable length, therefore are not stored in the *NX_MDNS_RR* structure. The Resource Record contains a pointer to the actual memory location where the string is stored. The string table and the Resource Records share the service cache. Resource Records are stored from the beginning of the service cache, and grow towards the end of the cache. The string table starts from the end of the service cache and grows towards the beginning of the cache. Each string in the string table has a length field and a counter field. When a string is added to the string table, if the same string is already present in the table, the counter value is incremented and no memory is allocated for the string. The service cache is considered full if no more resource records or new strings can be added to the service cache.

There are two ways for an application to find services offered on the local network. It can either issue a specific service look up through a one-shot query, or it can initiate a continuous query to "monitor" the activities on the network. In the one-short query scenario, the application must specify the service type. mDNS searches through the local service cache and the peer service cache. If a service instances is located, the one-shot query returns with the information found in the Resource Records. If there are no records in the local service cache or peer service cache, mDNS sends out query messages. If the instance name is specified, an *ANY* type (query the SRV and TXT type) with the specific instance name, in the form of "name.type.local", is sent to the local network. If the instance name is not specified, a PTR type of query is sent to the local network. The first complete service received is returned to the caller.

Continuous query works differently. The typical use case for a continuous query is to monitor the local network for a specific service (for example to constantly look for printing services on the local network). In this case the application issues a search query (via the API *nx_mdns_service_continuous_*query) for certain type of services. The caller typically does not wait for a particular response. For queries submitted as continuous query, the mDNS module transmits the queries periodically with exponentially increasing intervals. To stop the query, application must use the API *nx_mdns_service_query_stop* to stop the internal timer in these queries. The query type can be NULL, in which case the query type is set to special PTR type "_services._dns-sd._udp.local". This service type is defined by mDNS as a way to discover all services available on the local network. If the instance name is supplied, an ANY type (query the SRV and TXT type) with the specific instance name "name.type.local" is sent to the local network. If the instance name is NULL, a PTR type of query is sent to the local network,

All responses, including responses from unsolicited queries, are recorded in the peer service cache. At a later time, the application uses the API *nx_mdns_service_lookup* to retrieve specific service from the peer service cache.

Special note on fragmentation: Each *NX_MDNS_RR* structure is fixed in size, and therefore is not subject to fragmentation as mDNS adds and deletes RRs. However strings are variable length. After a string is deleted, the space becomes available and could be used for a new string if the new string is able to fit in. But this operation causes the memory to fragment. As services are added and removed, the string table may be fragmented to the point that no new strings can be added even though the service cache contains enough unused bytes for the string. Local applications tend to be more stable and predictable. Therefore the local service cache is less likely to suffer from fragmentation. The peer service cache, on the other hand, constantly add RRs as the services become available, or remove RRs as services are deleted by the nodes on the network. Services on the network come and go, causing more frequent allocate/delete operations on the string table. Over time it is possible the string table becomes fragmented. When this situation happens, a simple solution is to flush the entire peer service cache. This can be done by calling the API *nx_mdns_peer_cache_clear().* Note that this API automatically terminates any outstanding continuous queries.
