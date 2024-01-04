---
title: Appendix D - NetX Duo Advanced DHCPv6 server example
description: This chapter contains a NetX Duo Advanced DHCPv6 server example.
---

# Appendix D -  NetX Duo Advanced DHCPv6 server example

This is an advanced DHCPv6 Server which demonstrates saving and retrieving the Server's IP address lease table and Client record tables from non volatile memory, as required by the RFC 3315.

In this example, the include file *nxd_dhcpv6_server.h* is brought in at line 7. Next, the NetX Duo DHCPv6 Server application thread is created at line 81 in the example code below. Note that the DHCPv6 control block "*dhcp_server_0*" was defined as a global variable at line 19 previously.

Before creating the NetX Duo DHCPv6 Server instance, the demo creates packet pool for sending DHCPv6 messages in line 84, creates an IP thread interface in line 102, and enables UDP in NetX Duo in line 116.

The successful creation of NetX Duo DHCPv6 Server in line 136 includes the the two optional callbacks function described in Chapter 1.It enables IPv6 and ICMPv6 necessary for NetX Duo to process IPv6 and DHCPv6 operations in line 162-163. Before the DHCPv6 Server thread is ready to run, the DHCPv6 server must validate its IPv6 address(167-180),and define its DHCPv6 interface in lines 208-209. The *nx_dhcpv6_set_server_duid* service is called to create the Server if no Server DUID has been previously created in line 266. The Server sets up an IP address range for creating a list of assignable addresses. If data is saved from a previous session, it retrieves Client records and IPv6 lease data from memory in lines 283-318. It also creates its Server DUID, or if one was previously created, retrieves the DUID data from user specified storage. This is necessary to reproduce a consistent Server DUID across reboots. Optionally the host application defines a DNS server for Clients requesting DNS server configuration.

Next, the host starts the DHCPv6Server in line 329. This creates the DHCPv6 Server UPD socket and activates NetX Duo DHCPv6 Server timers. Then the Server waits to receive Client requests. While it can service many Clients it can only process a single Client request at a time.

The remainder of the example contains host implementations for saving and retrieving Server tables of its assignable IPv6 address pool and Client records to and from non volatile memory respectively. It also contains an option handler for options requested by DHCPv6 Clients that are not supported directly by the NetX Duo DHCPv6 Server (only the DNS server option is currently supported). Lastly there is a code for demonstrating how to save and retrieve 'non volatile time' by which the Server keeps track of assigned IP lease expiration.

```
1 /* This is a small demo of the NetX Duo DHCPv6 Server for the high-performance
2 NetX Duo TCP/IP stack. */
3
4 #include     <stdio.h>
5 #include     "tx_api.h"
6 #include     "nx_api.h"
7 #include     "nxd_dhcpv6_server.h"
8
9
10 #define     DEMO_STACK_SIZE 2048
11
12
13
14 /* Define the ThreadX and NetX Duo object control blocks... */
15
16 TX_THREAD             thread_0;
17 NX_PACKET_POOL        pool_0;
18 NX_IP                 ip_0;
19 NX_DHCPV6_SERVER      dhcp_server_0;
20
21
22 /* Define the counters used in the demo application... */
23
24 ULONG                 thread_0_counter;
25 ULONG                 state_changes;
26 ULONG                 error_counter;
27
28 #define               SERVER_PRIMARY_ADDRESS IP_ADDRESS(192,2,2,66)
29
30 /* Define thread prototypes. */
31
32 void     thread_0_entry(ULONG thread_input);
33
34 /***** Substitute your ethernet driver entry function here *********/
35 void     nx_etherDriver_mcf5485(struct NX_IP_DRIVER_STRUCT *driver_req);
36
37
38 /* Define some DHCPv6 parameters. */
39
40 #define DHCPV6_IANA_ID 0xC0DEDBAD
41 #define DHCPV6_T1      NX_DHCPV6_INFINITE_LEASE
42 #define DHCPV6_T2      NX_DHCPV6_INFINITE_LEASE
43
44
45 /* Declare NetX Duo DHCPv6 Server callbacks. */
46
47 VOID dhcpv6_decline_handler(struct NX_DHCPV6_SERVER_STRUCT *dhcpv6_server_ptr,
                              NX_DHCPV6_CLIENT *dhcpv6_client_ptr, UINT message_type);
48 VOID dhcpv6_option_request_handler(NX_DHCPV6_SERVER *dhcpv6_server_ptr, 
                                     UINT option_request, UCHAR *buffer_ptr, UINT *index);
49
50 /* Declare helper functions for the DHCPv6 Server host application. */
51 VOID dhcpv6_get_time_handler(ULONG *realtime);
52 UINT dhcpv6_create_ip_address_range(NX_DHCPV6_SERVER *dhcpv6_server_ptr, 
                                      UINT *addresses_added);
53 UINT dhcpv6_restore_ip_lease_table(NX_DHCPV6_SERVER *dhcpv6_server_ptr);
54 UINT dhcpv6_restore_client_records(NX_DHCPV6_SERVER *dhcpv6_server_ptr);
55 UINT dhcpv6_retrieve_ip_address_lease(NX_DHCPV6_SERVER *dhcpv6_server_ptr);
56 UINT dhcpv6_retrieve_client_records(NX_DHCPV6_SERVER *dhcpv6_server_ptr);
57
58
59 /* Define main entry point. */
60
61 intmain()
62 {
63
64     /* Enter the ThreadX kernel. */
65     tx_kernel_enter();
66 }
67
68
69 /* Define what the initial system looks like. */
70
71 void    tx_application_define(void *first_unused_memory)
72 {
73
74 CHAR    *pointer;
75 UINT    status;
76
77     /* Setup the working pointer. */
78     pointer = (CHAR *) first_unused_memory;
79
80     /* Create the main thread. */
81     tx_thread_create(&thread_0, "thread 0", thread_0_entry, 0,
82         pointer, DEMO_STACK_SIZE,
83         1, 1, TX_NO_TIME_SLICE, TX_AUTO_START);
84
85     pointer = pointer + DEMO_STACK_SIZE;
86
87     /* Initialize the NetX Duo system. */
88     nx_system_initialize();
89
90     /* Create a packet pool. */
91     status = nx_packet_pool_create(&pool_0, "NetX Main Packet Pool", NX_DHCPV6_PACKET_SIZE,
                                     pointer, NX_DHCPV6_PACKET_POOL_SIZE);
92                                   pointer = pointer + NX_DHCPV6_PACKET_POOL_SIZE;
93
94     /* Check for pool creation error. */
95     if (status != NX_SUCCESS)
96     {
97         error_counter++ ;
98         return;
99     }
100
101     /* Create an IP instance. */
102     status = nx_ip_create(&ip_0, "NetX IP Instance 0", SERVER_PRIMARY_ADDRESS,
103                          0xFFFFFF00UL, &pool_0, nx_etherDriver_mcf5485,
104                          pointer, 2048, 1);
105
106     pointer = pointer + 2048;
107
108     /* Check for IP create errors. */
109     if (status != NX_SUCCESS)
110     {
111         error_counter++ ;
112         return;
113     }
114
115     /* Enable UDP traffic for sending DHCPv6 messages. */
116     status = nx_udp_enable(&ip_0);
117
118     /* Check for UDP enable errors. */
119     if (status != NX_SUCCESS)
120     {
121         error_counter++;
122         return;
123     }
124
125     /* Enable ICMP. */
126     status = nx_icmp_enable(&ip_0);
127
128     /* Check for ICMP enable errors. */
129     if (status != NX_SUCCESS)
130     {
131         error_counter++ ;
132         return;
133     }
134
135     /* Create the DHCPv6 Server. */
136     status = nx_dhcpv6_server_create(&dhcp_server_0, &ip_0, "DHCPv6 Server", &pool_0,
                                        pointer, 2048, dhcpv6_decline_handler, dhcpv6_option_request_handler);
137
138     /* Check for errors. */
139     if (status != NX_SUCCESS)
140     {
141         error_counter++;
142     }
143
144     /* Yield control to DHCPv6 threads and ThreadX. */
145     return;
146 }
147
148
149 /* Define the Server host application thread. */
150
151 void     thread_0_entry(ULONG thread_input)
152 {
153
154 UINT         status;
155 NXD_ADDRESS ipv6_address_primary, dns_ipv6_address;
156 ULONGduid_time;
157 UINTiface_index, ga_address_index;
158 UINTaddresses_added;
159
160
161     /* Make the DHCPv6 Server IPv6 and ICMPv6 enabled. */
162     nxd_ipv6_enable(&ip_0);
163     nxd_icmp_enable(&ip_0);
164
165     memset(&ipv6_address_primary,0x0, sizeof(NXD_ADDRESS));
166
167     ipv6_address_primary.nxd_ip_version = NX_IP_VERSION_V6 ;
168     ipv6_address_primary.nxd_ip_address.v6[0] = 0x20010db8;
169     ipv6_address_primary.nxd_ip_address.v6[1] = 0xf101;
170     ipv6_address_primary.nxd_ip_address.v6[2] = 0x00000000;
171     ipv6_address_primary.nxd_ip_address.v6[3] = 0x00000101;
172
173
174
175
176     /* Wait till the IP task thread has had a chance to set the device MAC address. */177
178
179     tx_thread_sleep(10);
180
181
182
183
184
185 /* Set the primary interface link local address (address index 0). This
186 will use the host MAC address to build the link local address. */
187
188     nxd_ipv6_linklocal_address_set(&ip_0, NULL);
189
196 /* Set the single homed host global IP address. */
197
198     nxd_ipv6_global_address_set(&ip_0, &ipv6_address_primary, 64);
199
200
201     tx_thread_sleep(500);
202
203
204     /* Set the server interface for DHCP communications. */
205     iface_index = 0;
206     ga_address_index = 1;
207
208     /* Set the DHCPv6 server interface to the primary interface and global address index. */
209     status = nx_dhcpv6_server_interface_set(&dhcp_server_0, iface_index, ga_address_index);
210
211     /* Wait for DHCP to assign the IP address. */
212     do
213     {
214
215         /* Check for address resolution. */
216         status = nx_ip_status_check(&ip_0, NX_IP_ADDRESS_RESOLVED, (ULONG *)
                                        &status, 100000);
217
218         /* Check status. */
219         if (status)
220         {
221
222             tx_thread_sleep(20);
223         }
224
225     } while (status != NX_SUCCESS);
226
227     dns_ipv6_address.nxd_ip_version = NX_IP_VERSION_V6 ;
228     dns_ipv6_address.nxd_ip_address.v6[0] = 0x20010db8;
229     dns_ipv6_address.nxd_ip_address.v6[1] = 0x0000f101;
230     dns_ipv6_address.nxd_ip_address.v6[2] = 0x00000000;
231     dns_ipv6_address.nxd_ip_address.v6[3] = 0x00000107;
232
233     status = nx_dhcpv6_create_dns_address(&dhcp_server_0, &dns_ipv6_address);
234
235     /* Check for errors. */
236     if (status != NX_SUCCESS)
237     {
238
239         error_counter++;
240         return;
241     }
242
243     /* Set the server DUID before starting the DHCPv6 server. You will also need to set the
244        Server DUID if you are restoring Client data from non volatile memory.
245
246        This demo will create a server DUID of the link layer time DUID type.
247
248        Note #1: The RFC 3315 Sect 9.2 recommends link layer time DUID type over link layer
249        DUID type to minimize the chances of 'collisions' or identical DUIDs between hosts,
           particularly clients.
250
251        Note #2: If the client or server host is rebooting, RFC 3315 Sect 9.2 requires the
252        host retrieve its previously created DUID data rather than create one from new data.
253
254        For a Link layer time DUID, retrieve a time value. If the DHCPv6 server has not
255        created a server DUID previously, this function should provide a new value;        
           otherwise this function must retrieve the time data used in the previously created server DUID. For link layer and enterprise type DUIDs, the 'duid_time' data is not
           necessary. */
259        dhcpv6_get_time_handler(&duid_time);
260
261
262     /* For DUID types that do not require time, the 'duid_time' input can be left at zero.
263        The DUID_TYPE and HW_TYPE are configurable options that are user defined in
           nxd_dhcpv6_server.h. */
264
265
266     status = nx_dhcpv6_set_server_duid(&dhcp_server_0,
267                                       NX_DHCPV6_SERVER_DUID_TYPE, NX_DHCPV6_SERVER_HW_TYPE,
268                                       dhcp_server_0.nx_dhcpv6_ip_ptr >>
                                          nx_ip_arp_physical_address_msw,
269                                       dhcp_server_0.nx_dhcpv6_ip_ptr >>
                                          nx_ip_arp_physical_address_lsw,
270                                       duid_time);
271     if (status != NX_SUCCESS)
272     {
273         error_counter++ ;
274         return;
275     }
276
277
278     /* The next step is to set up the server IP lease and Client record tables. If no
279        previous data exists, the host application only needs to create an IP address range
280        of assignable IP addresses, and set the size of the tables, NX_DHCPV6_MAX_CLIENTS
281        and NX_DHCPV6_MAX_LEASES in nxd_dhcpv6_server.h. */
282
283 #ifndef RESTORE_SERVER_DATA
284
285     /* Create the ip address table on the primary server network interface. */
286     status = dhcpv6_create_ip_address_range(&dhcp_server_0, &addresses_added);
287
288     if (status != NX_SUCCESS)
289     {
290
291         error_counter++;
292         return;
293     }
294
295 #else
296
297     /* RFC 3315 requires that DHCPv6 servers be able to store and retrieve lease data to 
           and from non-volatile memory so that DHCPv6 server may remain uninterrupted across server reboots. */
299     status = dhcpv6_restore_ip_lease_table(&dhcp_server_0);
300
301     if (status != NX_SUCCESS)
302     {
303
304         error_counter++;
305         return;
306     }
307
308     status = dhcpv6_restore_client_records(&dhcp_server_0);
309
310     if (status != NX_SUCCESS)
311     {
312
313         error_counter++;
314         return;
315     }
316
317
318 #endif /* RESTORE_SERVER_DATA */
319
320     /*Check for error. */
321     if (status != NX_SUCCESS)
322     {
323
324         error_counter++;
325         return;
326     }
327
328     /* Start the NetX Duo DHCPv6 server! */
329     status = nx_dhcpv6_server_start(&dhcp_server_0);
330
331     /* Check for errors. */
332     if (status != NX_SUCCESS)
333     {
334         error_counter++;
335     }
336
337     return;
338 }
339
340 /* Simulate a handler with access to a real time clock and non volatile memory storage. 
       This service is required for a link layer time DUID to create a time value as part of
341    the DUID. A default value is provided below. The time value serves
342    no actual function, but increases the chances of a unique host DUID.
343
344    It is the host's responsibility to save the 'time' data created for the server DUID to
       memory. The DHCPv6 server should always use a previously created its server DUID as per
345    RFC 3315 Sect. 9.2. */
346 VOID dhcpv6_get_time_handler(ULONG *realtime)
347 {
348
349
350     /* Check if the DHCPv6 server has previously created a DUID. If so
351     return this time value to the host application. */
352     /*********** insert your application logic here **************/
353
354     /* Otherwise create time data. One can use a random number incremented
355     to the number of seconds since JAN 1, 2000 to
356     create a unique time value. */
357     *realtime = SECONDS_SINCE_JAN_1_2000_MOD_32;
358
359     return;
360 }
361
362
363 /* Create an IP address lease table based on from a range of available addresses. */
364 UINT dhcpv6_create_ip_address_range(NX_DHCPV6_SERVER *dhcpv6_server_ptr,
*UINT *addresses_added)
365 {
366
367 UINT status;
368 NXD_ADDRESS start_ipv6_address;
369 NXD_ADDRESS end_ipv6_address;
370
371
372     start_ipv6_address.nxd_ip_version = NX_IP_VERSION_V6 ;
373     start_ipv6_address.nxd_ip_address.v6[0] = 0x20010db8;
374     start_ipv6_address.nxd_ip_address.v6[1] = 0x00000f101;
375     start_ipv6_address.nxd_ip_address.v6[2] = 0x0;
376     start_ipv6_address.nxd_ip_address.v6[3] = 0x00000110;
377
378     end_ipv6_address.nxd_ip_version = NX_IP_VERSION_V6 ;
379     end_ipv6_address.nxd_ip_address.v6[0] = 0x20010db8;
380     end_ipv6_address.nxd_ip_address.v6[1] = 0x0000f101;
381     end_ipv6_address.nxd_ip_address.v6[2] = 0x00000000;
382     end_ipv6_address.nxd_ip_address.v6[3] = 0x00000120;
383
384     status = nx_dhcpv6_create_ip_address_range(dhcpv6_server_ptr, &start_ipv6_address,
                                                  &end_ipv6_address, addresses_added);
385
386     return status;
387
388 }
389
390 /* Demonstrate how to use NetX Duo DHCPv6 Server API to upload data from memory
391 to the DHCPv6 server's IP lease tables. */
392 UINT dhcpv6_restore_ip_lease_table(NX_DHCPV6_SERVER *dhcpv6_server_ptr)
393 {
394
395 NXD_ADDRESS  next_ipv6_address;
396 UINTi;
397 UINT         status;
398
399
400     /* Set the starting IP address. */
401     next_ipv6_address.nxd_ip_version = 6;
402     next_ipv6_address.nxd_ip_address.v6[0] = 0x20010db8;
403     next_ipv6_address.nxd_ip_address.v6[1] = 0x00000f101;
404     next_ipv6_address.nxd_ip_address.v6[2] = 0x0;
405     next_ipv6_address.nxd_ip_address.v6[3] = 0x00000110;
406
407
408     /* Copy the 'lease data' to the server table. */
409     for (i = 0; i< NX_DHCPV6_MAX_LEASES; i++)
410     {
411
412         /* These are assigned address leases. */
413
414         status = nx_dhcpv6_add_ip_address_lease(dhcpv6_server_ptr, i, &next_ipv6_address,
                     NX_DHCPV6_DEFAULT_T1_TIME, NX_DHCPV6_DEFAULT_T2_TIME,
415                  X_DHCPV6_DEFAULT_PREFERRED_TIME, NX_DHCPV6_DEFAULT_VALID_TIME);
416
417     if (status != NX_SUCCESS)
418         return status;
419
420         /* Simulate the next IP address in the table. */
421         next_ipv6_address.nxd_ip_address.v6[3]++;
422     }
423
424     return NX_SUCCESS;
425 }
426
427 /* Demonstrate how to use NetX Duo DHCPv6 Server API to download data to local memory and
428 eventually nonvolatile memory from the DHCPv6 server's IP lease tables. This might be
called after the a certain duration of operation and after stopping Server task (e.g.
before rebooting or for making a backup).*/
429 UINT     dhcpv6_retrieve_ip_address_lease(NX_DHCPV6_SERVER *dhcpv6_server_ptr)
430 {
431
432 NXD_ADDRESS   next_ipv6_address;
433 ULONG         T1, T2, valid_lifetime, preferred_lifetime;
434 UINTi;
435 UINT          status;
436
437
438
439
440
442     for (i = 0; i< NX_DHCPV6_MAX_LEASES; i++)
443     {
444
445         T1 = 0;
446         T2 = 0;
447         valid_lifetime = 0;
448         preferred_lifetime = 0;
449         memset(&next_ipv6_address, 0, sizeof(NXD_ADDRESS));
450
451         /* Get the next lease from the table. */
452         status = nx_dhcpv6_retrieve_ip_address_lease(dhcpv6_server_ptr, i,
                     &next_ipv6_address, &T1, &T2, &preferred_lifetime, &valid_lifetime);
453
454         if (status != NX_SUCCESS)
455             return status;
456
457         /* At this point the host application would store this record to NV memory. */
458     }
459
460     return NX_SUCCESS;
461 }
462
463 /* Demonstrate how to use NetX Duo DHCPv6 Server API to upload data from memory
464 to the DHCPv6 server's client record tables. */
465 UINT dhcpv6_restore_client_records(NX_DHCPV6_SERVER *dhcpv6_server_ptr)
466 {
467
468 UINTi;
469 UINT     status;
470
471 /* Create data to simulate client records stored in memory. */
472 NXD_ADDRESS client_ipv6_address;
473 UINTduid_type = 1;
474 UINTduid_hardware = NX_DHCPV6_HW_TYPE_IEEE_802;
475 ULONGmessage_xid = 0xabcd;
476 UINTduid_time = 0x1234567;
477 ULONGphysical_address_msw = 0x01;
478 ULONGphysical_address_lsw = 0x02030405;
479 ULONGIP_lifetime_time_accrued = 200000; /* lease time accrued (ticks) */
480 ULONGvalid_lifetime = 300000; /* expiration on the lease (ticks) */
481 ULONGenterprise_number = 0xaaaa;
482 UCHARprivate_id[8];
483 UINT     length;
484
485
486     /* Set the Client IP address. */
487     client_ipv6_address.nxd_ip_version = 6;
488     client_ipv6_address.nxd_ip_address.v6[0] = 0x20010db8;
489     client_ipv6_address.nxd_ip_address.v6[1] = 0x00000f101;
490     client_ipv6_address.nxd_ip_address.v6[2] = 0x0;
491     client_ipv6_address.nxd_ip_address.v6[3] = 0x00000110;
492
493
494     /* Copy the 'lease data' to the server table. */
495     for (i = 0; i< 10; i++)
496     {
497         /* Simulate a Client record with a vendor assigned DUID. */
498         if (i == 0)
499         {
500             duid_type = NX_DHCPV6_SERVER_DUID_TYPE_VENDOR_ASSIGNED;
501
502             memcpy(&private_id[0], "Corp_XYZ", sizeof("Corp_XYZ"));
503             length = sizeof("Corp_XYZ") + 4;
504             status = nx_dhcpv6_add_client_record(dhcpv6_server_ptr, i, message_xid,
                         &client_ipv6_address, NX_DHCPV6_STATE_BOUND, IP_lifetime_time_accrued,
                         valid_lifetime, duid_type, duid_hardware, physical_address_msw,
                         physical_address_lsw, duid_time, enterprise_number,
506                      &private_id[0], length);
507         }
508         /* Simulate client record with a link layer DUID. */
509         else
510         {
511                 status = nx_dhcpv6_add_client_record(dhcpv6_server_ptr, i, message_xid,
                             &client_ipv6_address, NX_DHCPV6_STATE_BOUND, IP_lifetime_time_accrued,
512                          valid_lifetime, duid_type, duid_hardware, physical_address_msw,
                             physical_address_lsw, duid_time, 0, NX_NULL, 0);
513         }
515
516         /* Check for error. */
517         if (status != NX_SUCCESS)
518         {
519
520             /* Check if the Client address is found in the IP lease table. */
521             if (status == NX_DHCPV6_ADDRESS_NOT_FOUND)
522             {
523
524                 /* It is not. Client state should be set to unbound/init.*/
525             }
526             else
527             {
528
529                 /* Either the table is full or the index exceeds the bounds of the table. */
530                 return status;
531             }
532         }
533
534         /* Simulate the Client IP address in the table. Leave all other client 'data' the
            same for the next record we'll 'restore'. */
535         client_ipv6_address.nxd_ip_address.v6[3]++;
536         physical_address_lsw++;
537         message_xid++;
538     }
539
540     return NX_SUCCESS;
541 }
542
543 /* Demonstrate how to use NetX Duo DHCPv6 Server API to download data to local memory and
544 eventually nonvolatile memory from the DHCPv6 server's client record tables. */
545
546 UINT dhcpv6_retrieve_client_records(NX_DHCPV6_SERVER *dhcpv6_server_ptr)
547 {
548
549 UINTi;
550 UINT         status;
551 NXD_ADDRESS  client_ipv6_address;
552 UINTduid_type;
553 UINTduid_hardware;
554 ULONGmessage_xid;
555 ULONGduid_time;
556 ULONGphysical_address_msw;
557 ULONGphysical_address_lsw;
558 ULONGIP_lifetime_time_accrued; /* lease time accrued (ticks) */
559 ULONGvalid_lifetime; /* expiration on the lease (ticks) */
560 ULONGduid_vendor_number;
561 UCHARprivate_id[8];
562 UINT          length;
563 UINTclient_state;
564
565
566     for (i = 0; i< 100; i++)
567     {
568
569         memset(&client_ipv6_address, 0,sizeof(NXD_ADDRESS));
570
571         status = nx_dhcpv6_retrieve_client_record(dhcpv6_server_ptr, i, &message_xid,
                     &client_ipv6_address, &client_state, &IP_lifetime_time_accrued,
572                  &valid_lifetime, &duid_type, &duid_hardware, &physical_address_msw,
573                  &physical_address_lsw, &duid_time, &duid_vendor_number, &private_id[0], 
                     &length);
574
575         if (status != NX_SUCCESS)
576         {
577             /* The host application should handle error status returns depending on
578             the specific error code. See the user guide for error returns for
579             this service. */
580         }
581
582     }
583
584     return NX_SUCCESS;
586 }
587
589 /* This is an optional callback for NetX DHCPv6 server to notify the host application
590 that it has received either a DECLINE or RELEASE address from a Client. */
591
592 VOID dhcpv6_decline_handler(NX_DHCPV6_SERVER *dhcpv6_server_ptr, NX_DHCPV6_CLIENT
                               *dhcpv6_client_ptr, UINT message_type)
593 {
594
595     switch (message_type)
596     {
597         case NX_DHCPV6_MESSAGE_TYPE_DECLINE:
598
599
600             /* Host application handles a declined address. The Server will
601             mark the address as assigned elsewhere. Any other processing
602             is left to the host application. */
603
604         break;
605
606         case NX_DHCPV6_MESSAGE_TYPE_RELEASE:
607
608             /* Host application handles a released address. The Server will
609             mark the released IP address as available for lease to other
610             clients. Any other processing is left to the host application. */
611
612         break;
613
614         default:
615
616             /* Unhandled message type */
617             error_counter++;
618         break;
619     }
620
621     return;
622 }
623
624 /* This is an optional DHCPv6 server callback to handle client option request options. */
625 VOID dhcpv6_option_request_handler(NX_DHCPV6_SERVER *dhcpv6_server_ptr, UINT
                                      option_request, UCHAR *buffer_ptr, UINT *index)
626 {
627
628 UCHARoption_length = 10;
629 UCHARoption_code = 24;
630 ULONGmessage_word;
631
632
633     if (option_request == 24)
634     {
635
636         message_word = option_code<< 16;
637         message_word |= option_length;
638
639         /* Adjust for endianness. */
640         NX_CHANGE_ULONG_ENDIAN(message_word);
641
642         /* Copy the option request option header to the packet. */
643         memcpy(buffer_ptr + *index, &message_word, sizeof(ULONG));
644         *index += sizeof(ULONG);
645
646         /* Copy the code for domain search list. */
647         *(buffer_ptr + *index) = 0x04;
648         (*index)++;
649
650         /* Adjusting for endianness is an exercise left for the reader. */
651         memcpy(buffer_ptr + *index, "abc.com", sizeof("abc.com"));
652         (*index) += sizeof("abc.com");
653     }
654     /* else unknown option; just return; no need to adjust buffer pointers. */
655
656     return;
657 }
```

**Figure 6. Advanced NetX Duo DHCPv6 Server Application**