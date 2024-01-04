---
title: Chapter 2 - Installation and use of NetX Duo DHCPv6 server
description: This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo DHCPv6 Server.
---

# Chapter 2 - Installation and use of NetX Duo DHCPv6 server

This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo DHCPv6 Server.

## Product Distribution

NetX Duo can be obtained from our public source code repository at [https://github.com/azure-rtos/netxduo/](https://github.com/azure-rtos/netxduo/).

**nxd_dhcpv6_server.h** NetX DuoDHCPv6Server header file

**nxd_dhcpv6_server.c** NetX DuoDHCPv6Server source file

**demo_netxduo_dhcpv6.c** NetX Duo DHCPv6 Server demo file

**nxd_dhcpv6_server.pdf** NetX Duo DHCPv6Server User Guide

## NetX Duo DHCPv6 Server Installation

In order to use the NetX Duo DHCPv6Server API, the entire distribution mentioned previously should be copied to the same directory where NetX Duo is installed. For example, if NetX Duo is installed in the directory "*\threadx\arm7\green*" then the *nxd_dhcpv6_server.h* and *nx_dhpcv6_server.c* files should be copied into this directory.

## Using NetX Duo DHCPv6 Server

Using the NetX Duo DHCPv6Server API is easy. Basically, the application code must include *nx_dhcpv6-server.h* after it includes *tx_api.h* and *nx_api.h*, in order to use ThreadX and NetX Duo, respectively. The application must also include *nxd_dhcpv6_server.c* in the build process. This file must be compiled in the same manner as other application files and its object form must be linked along with the files of the application. This is all that is required to use NetX Duo DHCPv6 Server.

Note that since DHCPv6is based on the IPv6 protocol, IPv6 must be enabled on the IP instance using *nxd_ipv6_enable*. NetX Duo UDP and ICMPv6 services are also utilized. UDP is enabled by calling *nx_udp_enable* and ICMPv6is enabled by calling *nxd_icmp_enable* prior to starting the NetX Duo DHCPv6 Server thread task.

## Small Example System

An example of how easy it is to use the NetX Duo DHCPv6 Server is described in the small example below using a DHCPv6 Client and Server running over a virtual "RAM" driver. This demo assumes a single homed host using the NetX Duo environment.

*tx_application_define* creates packet pool for sending DHCPv6 message, a thread and an IP instance for both the Client and Server, and enables UDP (DHCP runs over UDP), IPv6, ICMP and ICMPv6 for both Client and Server IP tasks in lines 116-157.

The DHCPv6 Server is created in line 456. It does not define the optional address decline or option request handlers. In the Server thread entry function, the Server IP is set up with a link local address services in lines 435-453.

Before starting the DHCPv6 Server, the host application creates a Server DUID in line 498 and sets the local network DNS server on line 483. It then creates a table of assignable IP addresses in lines 521. See the **Advanced Example System** in Appendix D for how to store and retrieve Server tables from memory.

Then the DHCPv6 Server is ready to start on line 530.

For details on creating and running the NetX Duo DHCPv6 Client see the *nxd_dhcpv6_client.pdf* file distributed on with the DHCPv6 Server.

```

1 /* This is a small demo of the NetX Duo DHCPv6 Client and Server for the 
2 high-performance NetX Duo stack. */
3
4 #include <stdio.h>
5 #include "tx_api.h"
6 #include "nx_api.h"
7 #include "nxd_dhcpv6_client.h"
8 #include "nxd_dhcpv6_server.h"
9
10 #ifdef FEATURE_NX_IPV6
11
12 #define DEMO_STACK_SIZE                 2048
13 #define NX_DHCPV6_THREAD_STACK_SIZE     2048
14
15 /* Define the ThreadX and NetX object control blocks... */
16
17 NX_PACKET_POOL     pool_0;
18 TX_THREAD          thread_client;
19 NX_IP              client_ip;
20 TX_THREAD          thread_server;
21 NX_IP              server_ip;
22
23 /* Define the Client and Server instances. */
24
25 NX_DHCPV6         dhcp_client;
26 NX_DHCPV6_SERVER  dhcp_server;
27
28 /* Define the error counter used in the demo application... */
29 ULONG             error_counter;
30 CHAR              *pointer;
31 
32 NXD_ADDRESS       server_address;
33 NXD_ADDRESS       dns_ipv6_address;
34 NXD_ADDRESS       start_ipv6_address;
35 NXD_ADDRESS       end_ipv6_address;
36
37
38 /* Define thread prototypes. */
39
40 void thread_client_entry(ULONG thread_input);
41 void thread_server_entry(ULONG thread_input);
42
43 /***** Substitute your ethernet driver entry function here *********/
44 VOID _nx_ram_network_driver(NX_IP_DRIVER *driver_req_ptr);
45
46
47 /* Define some DHCPv6 parameters. */
48
49 #define DHCPV6_IANA_ID                  0xC0DEDBAD
50 #define DHCPV6_T1                       NX_DHCPV6_INFINITE_LEASE
51 #define DHCPV6_T2                       NX_DHCPV6_INFINITE_LEASE
52 #define NX_DHCPV6_REFERRED_LIFETIME     NX_DHCPV6_INFINITE_LEASE
53 #define NX_DHCPV6_VALID_LIFETIME        NX_DHCPV6_INFINITE_LEASE
54
55
56 /* Define main entry point. */
57
58 int main()
59 {
60
61     /* Enter the ThreadX kernel. */
62     tx_kernel_enter();
63 }
64
65
66 /* Define what the initial system looks like. */
67
68 void     tx_application_define(void *first_unused_memory)
69 {
70
71 UINT     status;
72
73     /* Setup the working pointer. */
74     pointer = (CHAR *) first_unused_memory;
75
76     /* Initialize the NetX system. */
77     nx_system_initialize();
78
79     /* Create a packet pool. */
80     status = nx_packet_pool_create(&pool_0, "NetX Main Packet Pool", 1024, pointer,
                NX_DHCPV6_PACKET_POOL_SIZE);
81    pointer = pointer + NX_DHCPV6_PACKET_POOL_SIZE;
82
83     /* Check for pool creation error. */
84     if (status)
85         error_counter++;
86
87     /* Create a Client IP instance. */
88     status = nx_ip_create(&client_ip, "Client IP", IP_ADDRESS(0, 0, 0, 0),
89              0xFFFFFF00UL, &pool_0, _nx_ram_network_driver,
90              pointer, 2048, 1);
91
92     pointer = pointer + 2048;
93
94     /* Check for IP create errors. */
95     if (status)
96     {
97         error_counter++;
98         return;
99     }
100
101     /* Create a Server IP instance. */
102     status = nx_ip_create(&server_ip, "Server IP", IP_ADDRESS(1, 2, 3, 4),
103              0xFFFFFF00UL, &pool_0, _nx_ram_network_driver,
104              pointer, 2048, 1);
105
106     pointer = pointer + 2048;
107
108     /* Check for IP create errors. */
109     if (status)
110     {
111         error_counter++;
112         return;
113     }
114
115     /* Enable UDP traffic for sending DHCPv6 messages. */
116     status = nx_udp_enable(&client_ip);
117     status += nx_udp_enable(&server_ip);
118
119     /* Check for UDP enable errors. */
120     if (status)
121     {
122         error_counter++;
123         return;
124     }
125
126     /* Enable ICMP. */
127     status = nx_icmp_enable(&client_ip);
128     status += nx_icmp_enable(&server_ip);
129
130     /* Check for ICMP enable errors. */
131     if (status)
132     {
133         error_counter++;
134         return;
135     }
136
137     /* Enable the IPv6 services. */
138     status = nxd_ipv6_enable(&client_ip);
139     status += nxd_ipv6_enable(&server_ip);
140
141     /* Check for IPv6 enable errors. */
142     if (status)
143     {
144         error_counter++;
145         return;
146     }
147
148     /* Enable the ICMPv6 services. */
149     status = nxd_icmp_enable(&client_ip);
150     status += nxd_icmp_enable(&server_ip);
151
152     /* Check for ICMP enable errors. */
153     if (status)
154     {
155         error_counter++;
156         return;
157     }
158
159     /* Create the Client thread. */
160     status = tx_thread_create(&thread_client, "Client thread", thread_client_entry, 0,
161              pointer, DEMO_STACK_SIZE,
162              8, 8, TX_NO_TIME_SLICE, TX_AUTO_START);
163     /* Check for IP create errors. */
164     if (status)
165     {
166         error_counter++;
167         return;
168     }
169
170     pointer = pointer + DEMO_STACK_SIZE;
171
172     /* Create the Server thread. */
173     status = tx_thread_create(&thread_server, "Server thread", thread_server_entry, 0,
174              pointer, DEMO_STACK_SIZE,
175              4, 4, TX_NO_TIME_SLICE, TX_AUTO_START);
176     /* Check for IP create errors. */
177     if (status)
178     {
179         error_counter++;
180         return;
181     }
182
183     pointer = pointer + DEMO_STACK_SIZE;
184
185     /* Yield control to DHCPv6 threads and ThreadX. */
186     return;
187     }
188
189 /* Define the Client host application thread. */
190
191 void     thread_client_entry(ULONG thread_input)
192 {
193
194 UINT         status;
195
196 #ifdef GET_ONE_SPECIFIC_ADDRESS
197 NXD_ADDRESS ia_ipv6_address;
198 #endif
199
200 NXD_ADDRESS ipv6_address;
201 NXD_ADDRESS dns_address;
202 ULONG        T1, T2, preferred_lifetime, valid_lifetime;
203 UINT         address_count;
204 UINT         address_index;
205 UINT         dns_index;
206 NX_PACKET    *my_packet;
207
208
209     /* Establish the link local address for the host. The RAM driver creates
210     a virtual MAC address of 0x1122334456. */
211     status = nxd_ipv6_address_set(&client_ip, 0, NX_NULL, 10, NULL);
212
213     /* Let NetX Duo and the network driver get initialized. Also give the server time  to get set up. */
214     tx_thread_sleep(300);
215
216
217     /* Create the DHCPv6 Client. */
218     status = nx_dhcpv6_client_create(&dhcp_client, &client_ip, "DHCPv6 Client",     
                 &pool_0, pointer, NX_DHCPV6_THREAD_STACK_SIZE,
219              NX_NULL, NX_NULL);
220
221     /* Check for errors. */
222     if (status)
223     {
224         error_counter++;
225         return;
226     }
227
228     /* Update the stack pointer because we need it again. */
229     pointer = pointer + NX_DHCPV6_THREAD_STACK_SIZE;
230
231     /* Create a Link Layer Plus Time DUID for the DHCPv6 Client. Set time ID field
232     to NULL; the DHCPv6 Client API will supply one. */
233     status = nx_dhcpv6_create_client_duid(&dhcp_client, NX_DHCPV6_DUID_TYPE_LINK_TIME,
234     NX_DHCPV6_HW_TYPE_IEEE_802, 0);
235
236     if (status != NX_SUCCESS)
237     {
238         error_counter++;
239         return;
240     }
241
242     /* Create the DHCPv6 client's Identity Association (IA-NA) now.
243
244     Note that if this host had already been assigned in IPv6 lease, it
245     would have to use the assigned T1 and T2 values in loading the DHCPv6
246     client with an IANA block.
247     */
248     status = nx_dhcpv6_create_client_iana(&dhcp_client, DHCPV6_IANA_ID, DHCPV6_T1,     DHCPV6_T2);
249
250     if (status != NX_SUCCESS)
251     {
252         error_counter++;
253         return;
254     }
255
256     /* Starting up the NetX DHCPv6 Client. */
257     status = nx_dhcpv6_start(&dhcp_client);
258
259     /* Check for errors. */
260     if (status != NX_SUCCESS)
261     {
262
263         return;
264     }
265
266     /* Let DHCPv6 Server start. */
267     tx_thread_sleep(500);
268
269 #ifdef GET_ONE_SPECIFIC_ADDRESS
270
271     /* Create an IA address option.
272
273         The client includes IA options for any IAs to which it wants the server 
            to assign addresses.
274     */
275
276     memset(&ia_ipv6_address,0x0, sizeof(NXD_ADDRESS));
277     ia_ipv6_address.nxd_ip_version = NX_IP_VERSION_V6 ;
278     ia_ipv6_address.nxd_ip_address.v6[0] = 0x20010db8;
279     ia_ipv6_address.nxd_ip_address.v6[1] = 0x00000f101;
280     ia_ipv6_address.nxd_ip_address.v6[2] = 0x0;
281     ia_ipv6_address.nxd_ip_address.v6[3] = 0x00000115;
282
283     status = nx_dhcpv6_create_client_ia(&dhcp_client, &ia_ipv6_address                     
                 NX_DHCPV6_REFERRED_LIFETIME,
284              NX_DHCPV6_VALID_LIFETIME);
285
286     if (status != NX_SUCCESS)
287     {
288         error_counter++;
289         return;
290     }
291
292 #endif
293
294     /* If the host also want to get the option message, set the list of 
        desired options to enabled. */
295     nx_dhcpv6_request_option_timezone(&dhcp_client, NX_TRUE);
296     nx_dhcpv6_request_option_DNS_server(&dhcp_client, NX_TRUE);
297     nx_dhcpv6_request_option_time_server(&dhcp_client, NX_TRUE);
298     nx_dhcpv6_request_option_domain_name(&dhcp_client, NX_TRUE);
299
300     /* Now, the host send the solicit message to get the IPv6 address and 
        other options from the DHCPv6 server. */
301     status = nx_dhcpv6_request_solicit(&dhcp_client);
302
303     /* Check status. */
304     if (status != NX_SUCCESS)
305     {
306
307         error_counter++;
308         return;
309     }
310
311     /* Waiting for get the IPv6 address and do the duplicate address detection. */
312     /* 
313        Note, if the host detect another host withe the same address, the DHCPv6 
           Client can automatically
314        declient the address. At time T1 for an IPv6 address, the DHCPv6 Client can     automatically renew the address.
315        At time T2 for an IPv6 address, the DHCPv6 Client can automatically rebind the address.
316        At time valid lifetime for an IPv6 address, the DHCPv6 Client can automatically delete the IPv6 address.
317     */
318     tx_thread_sleep(500);
319
320
321     /* Get the T1 and T2 value of IANA option. */
322     status = nx_dhcpv6_get_iana_lease_time(&dhcp_client, &T1, &T2);
323
324     /* Check status. */
325     if (status != NX_SUCCESS)
326     {
327         error_counter++;
328     }
329
330     /* Get the valid IPv6 address count which the DHCPv6 server assigned . */
331     status = nx_dhcpv6_get_valid_ip_address_count(&dhcp_client, &address_count);
332
333     /* Check status. */
334     if (status != NX_SUCCESS)
335     {
336         error_counter++;
337     }
338
339     /* Get the IPv6 address, preferred lifetime and valid lifetime according to the address index. */
340     address_index = 0;
341     status = nx_dhcpv6_get_valid_ip_address_lease_time(&dhcp_client, address_index, 
&ipv6_address, &preferred_lifetime, &valid_lifetime);
342
343     /* Check status. */
344     if (status != NX_SUCCESS)
345     {
346         error_counter++;
347     }
348
349     /* Get the IPv6 address.
350        Note, This API only applies to one IA. */
351     status = nx_dhcpv6_get_IP_address(&dhcp_client, &ipv6_address);
352
353     /* Check status. */
354     if (status != NX_SUCCESS)
355     {
356         error_counter++;
357     }
358
359     /* Get IP address lease time.
360        Note, This API only applies to one IA. */
361     status = nx_dhcpv6_get_lease_time_data(&dhcp_client, &T1, &T2, &preferred_lifetime, 
&valid_lifetime);
362
363     /* Check status. */
364     if (status != NX_SUCCESS)
365     {
366         error_counter++;
367     }
368
369     /* Get the DNS Server address lease time. */
370     dns_index = 0;
371     status = nx_dhcpv6_get_DNS_server_address(&dhcp_client, dns_index, &dns_address);
372
373     /* Check status. */
374     if (status != NX_SUCCESS)
375     {
376         error_counter++;
377     }
378
379     /**************************************************/
380     /* Ping the DHCPv6 Server, Test the IPv6 address. */
381     /**************************************************/
382
383     /* Ping an unknown IP address. This will timeout after 100 ticks. */
384     status = nxd_icmp_ping(&client_ip, &server_address, "ABCDEFGHIJKLMNOPQRSTUVWXYZ", 
                 28, &my_packet, 100);
385
386     /* Determine if the timeout error occurred. */
387     if ((status != NX_SUCCESS) || (my_packet == NX_NULL))
388     {
389         error_counter++;
390     }
391
392     /* If we want to release the address, we can send release message to
393        the server we are releasing the assigned address. */
394     status = nx_dhcpv6_request_release(&dhcp_client);
395
396     /* Check status. */
397     if (status != NX_SUCCESS)
398     {
399
400         error_counter++;
401         return;
402     }
403
404     /* Stopping the Client task. */
405     status = nx_dhcpv6_stop(&dhcp_client);
406
407     /* Check status. */
408     if (status != NX_SUCCESS)
409     {
410
411         error_counter++;
412         return;
413     }
414
415     /* Now delete the DHCPv6 client and release ThreadX and NetX resources back to
416        the system. */
417     nx_dhcpv6_client_delete(&dhcp_client);
418
419     return;
420
421 }
422
423 /* Define the test server thread. */
424 void     thread_server_entry(ULONG thread_input)
425 {
426
427 UINT     status;
428 ULONG    duid_time;
429 UINT     addresses_added;
430
431
432     /* Wait till the IP task thread has had a chance to set the device MAC address. */
433     tx_thread_sleep(100);
434
435     memset(&server_address,0x0, sizeof(NXD_ADDRESS));
436
437     server_address.nxd_ip_version = NX_IP_VERSION_V6 ;
438     server_address.nxd_ip_address.v6[0] = 0x20010db8;
439     server_address.nxd_ip_address.v6[1] = 0xf101;
440     server_address.nxd_ip_address.v6[2] = 0x00000000;
441     server_address.nxd_ip_address.v6[3] = 0x00000101;
442
443     /* Set the link local and global addresses. */
444     status = nxd_ipv6_address_set(&server_ip, 0, NX_NULL, 10, NULL);
445     status += nxd_ipv6_address_set(&server_ip, 0, &server_address, 64, NULL);
446
447     /* Check for errors. */
448     if (status != NX_SUCCESS)
449     {
450
451         error_counter++;
452         return;
453     }
454
455     /* Create the DHCPv6 Server. */
456     status = nx_dhcpv6_server_create(&dhcp_server, &server_ip, "DHCPv6 Server", &pool_0, pointer, NX_DHCPV6_SERVER_THREAD_STACK_SIZE, NX_NULL, NX_NULL);
457
458     /* Check for errors. */
459     if (status != NX_SUCCESS)
460     {
461         error_counter++;
462     }
463
464     /* Update the stack pointer in case we need it again. */
465     pointer = pointer + NX_DHCPV6_SERVER_THREAD_STACK_SIZE;
466
467     /* Note this example assumes a single global IP address on the primary interface. If otherwise
468        the host should call the service to set the network interface and global IP address index.
469
470     UINT _nx_dhcpv6_server_interface_set(NX_DHCPV6_SERVER *dhcpv6_server_ptr, UINT interface_index, UINT address_index)
471     */
472
473     /* Validate the link local and global addresses. */
474     tx_thread_sleep(500);
475
476     /* Set up the DNS IPv6 server address. */
477     dns_ipv6_address.nxd_ip_version = NX_IP_VERSION_V6 ;
478     dns_ipv6_address.nxd_ip_address.v6[0] = 0x20010db8;
479     dns_ipv6_address.nxd_ip_address.v6[1] = 0x0000f101;
480     dns_ipv6_address.nxd_ip_address.v6[2] = 0x00000000;
481     dns_ipv6_address.nxd_ip_address.v6[3] = 0x00000107;
482
483     status = nx_dhcpv6_create_dns_address(&dhcp_server, &dns_ipv6_address);
484
485     /* Check for errors. */
486     if (status != NX_SUCCESS)
487     {
488
489         error_counter++;
490         return;
491     }
492
493     /* Note: For DUID types that do not require time, the 'duid_time' input can be 
left at zero.
494        The DUID_TYPE and HW_TYPE are configurable options that are user defined in nx_dhcpv6_server.h. */
495
496     /* Set the DUID time as the start of the millennium. */
497     duid_time = SECONDS_SINCE_JAN_1_2000_MOD_32;
498     status = nx_dhcpv6_set_server_duid(&dhcp_server,
499              NX_DHCPV6_SERVER_DUID_TYPE, NX_DHCPV6_SERVER_HW_TYPE,
500              dhcp_server.nx_dhcpv6_ip_ptr -> nx_ip_arp_physical_address_msw,
501              dhcp_server.nx_dhcpv6_ip_ptr -> nx_ip_arp_physical_address_lsw,
502              duid_time);
503     if (status != NX_SUCCESS)
504     {
505         error_counter++ ;
506         return;
507     }
508
509     start_ipv6_address.nxd_ip_version = NX_IP_VERSION_V6 ;
510     start_ipv6_address.nxd_ip_address.v6[0] = 0x20010db8;
511     start_ipv6_address.nxd_ip_address.v6[1] = 0x00000f101;
512     start_ipv6_address.nxd_ip_address.v6[2] = 0x0;
513     start_ipv6_address.nxd_ip_address.v6[3] = 0x00000110;
514
515     end_ipv6_address.nxd_ip_version = NX_IP_VERSION_V6 ;
516     end_ipv6_address.nxd_ip_address.v6[0] = 0x20010db8;
517     end_ipv6_address.nxd_ip_address.v6[1] = 0x0000f101;
518     end_ipv6_address.nxd_ip_address.v6[2] = 0x00000000;
519     end_ipv6_address.nxd_ip_address.v6[3] = 0x00000120;
520
521     status = nx_dhcpv6_create_ip_address_range(&dhcp_server, &start_ipv6_address, &end_ipv6_address, &addresses_added);
522
523     if (status != NX_SUCCESS)
524     {
525         error_counter++ ;
526         return;
527     }
528
529     /* Start the NetX DHCPv6 server! */
530     status = nx_dhcpv6_server_start(&dhcp_server);
531
532     /* Check for errors. */
533     if (status != NX_SUCCESS)
534     {
535         error_counter++;
536     }
537
538     return;
539 }
540 #endif /* FEATURE_NX_IPV6 */
```

**Figure 6. Example of the NetX Duo DHCPv6 Server**