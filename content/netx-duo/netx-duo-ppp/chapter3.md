---
title: Chapter 3 - Description of NetX Duo Point-to-Point Protocol (PPP) services
description: This chapter contains a description of all NetX Duo PPP services (listed below) in alphabetic order.
---


This chapter contains a description of all NetX Duo PPP services (listed below) in alphabetic order.

In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_ppp_byte_receive

Receive a byte from serial ISR

### Prototype

```c
UINT nx_ppp_byte_receive(
    NX_PPP *ppp_ptr,
    UCHAR byte);
```

### Description

This service is typically called from the application's serial driver Interrupt Service Routine (ISR) to transfer a received byte to PPP. When called, this routine places the received byte into a circular byte buffer and notifies the appropriate PPP thread for processing.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *byte*: Byte received from serial device

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP byte receive.
- **NX_PPP_BUFFER_FULL**: (0xB1) PPP serial buffer is already full.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Threads, ISRs

### Example

```c
/* Notify "my_ppp" of a received byte. */
status =  nx_ppp_byte_receive(&my_ppp, new_byte);

/* If status is NX_SUCCESS the received byte was successfully
   buffered. */
```

## nx_ppp_chap_challenge

Generate a CHAP challenge

### Prototype

```c
UINT nx_ppp_chap_challenge(NX_PPP *ppp_ptr);
```

### Description

This service initiates a CHAP challenge after the PPP connection is already up and running. This gives the application the ability to verify the authenticity of the connection on a periodic basis. If the challenge is unsuccessful, the PPP link is closed.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP challenge initiated.
- **NX_PPP_FAILURE**: (0xB0) Invalid PPP challenge, CHAP was enabled only for response.
- **NX_NOT_IMPLEMENTED**: (0x80) CHAP logic was disabled via NX_PPP_DISABLE_CHAP.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Initiate a PPP challenge for instance "my_ppp". */
status =  nx_ppp_chap_challenge(&my_ppp);

/* If status is NX_SUCCESS a CHAP challenge "my_ppp" was successfully 
initiated. */
```

## nx_ppp_chap_enable

Enable CHAP authentication

### Prototype

```c
UINT nx_ppp_chap_enable(
    NX_PPP *ppp_ptr, 
    UINT (*get_challenge_values)(
        CHAR *rand_value,
        CHAR *id,
        CHAR *name),
    UINT (*get_responder_values)(
        CHAR *system,
        CHAR *name,
        CHAR *secret),
    UINT (*get_verification_values)(
        CHAR *system,
        CHAR *name,
        CHAR *secret)); 
```

### Description

This service enables the Challenge-Handshake Authentication Protocol (CHAP) for the specified PPP instance.

If the "***get_challenge_values***" and "***get_verification_values***" function pointers are specified, CHAP is required by this PPP instance. Otherwise, CHAP only responds to the peer's challenge requests.

There are several data items referenced below in the required callback functions. The data items *secret*, *name*, and *system* are expected to be NULL-terminated strings with a maximum size of NX_PPP_NAME_SIZE-1. The data item *rand_value* is expected to be a NULL-terminated string with a maximum size of NX_PPP_VALUE_SIZE-1. The data item *id* is a simple unsigned character type.

> **Note:** This function must be called after *nx_ppp_create* but before nx_ip_create or *nx_ip_interface_attach*.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *get_challenge_values*: Pointer to application function to retrieve values used for the challenge. Note that the *rand_value*, *id*, and *secret* values must be copied into the supplied destinations.
- *get_responder_values*: Pointer to application function that retrieves values used to respond to a challenge. Note that the *system*, *name*, and *secret* values must be copied into the supplied destinations.
- *get_verification_values*: Pointer to application function that retrieves values used to verify the challenge response. Note that the *system*,*name*, and *secret* values must be copied into the supplied destinations.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP CHAP enable
- **NX_NOT_IMPLEMENTED**: (0x80) CHAP logic was disabled via NX_PPP_DISABLE_CHAP.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer or callback function pointer. Note that if *get_challenge_values* is specified, then the *get_verification_values* function must also be supplied.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Initialization, threads

### Example

```c
CHAR    name_string[] = "username";
CHAR    rand_value_string[] = "123456";
CHAR    system_string[] = "system";
CHAR    secret_string[] = "secret";

/* Enable CHAP in both directions (CHAP challenger and CHAP responder) for 
"my_ppp". */
status =  nx_ppp_chap_enable(&my_ppp,	get_challenge_values, 
                              get_responder_values,
                              get_verification_values);


/* If status is NX_SUCCESS, "my_ppp" has CHAP enabled. */
/* Define the CHAP enable routines.  */
UINT  get_challenge_values(CHAR *rand_value, CHAR *id, CHAR *name)
{
   UINT    i;
   for (i = 0; i< (NX_PPP_NAME_SIZE-1); i++)
   {
      name[i] = name_string[i];
   }
   name[i] =  0;
   
   *id =  '1';  /* One byte  */
   for (i = 0; i< (NX_PPP_VALUE_SIZE-1); i++)
   {
      rand_value[i] =  rand_value_string[i];
   }
   rand_value[i] =  0;
   
   return(NX_SUCCESS);  
}


UINT  get_responder_values(CHAR *system, CHAR *name, CHAR *secret)
{
   UINT    i;
   
   for (i = 0; i< (NX_PPP_NAME_SIZE-1); i++)
   {
      name[i] = name_string[i];
   }
   name[i] =  0;

   for (i = 0; i< (NX_PPP_NAME_SIZE-1); i++)
   {
      system[i] =  system_string[i];
   }
   system[i] =  0;
   
   for (i = 0; i< (NX_PPP_NAME_SIZE-1); i++)
   {
      secret[i] =  secret_string[i];
   }
   secret[i] =  0;
   
   return(NX_SUCCESS);  
}

UINT  get_verification_values(CHAR *system, CHAR *name, CHAR *secret)
{
   UINT    i;
   
   for (i = 0; i< (NX_PPP_NAME_SIZE-1); i++)
   {
      name[i] = name_string[i];
   }
   name[i] =  0;
   
   for (i = 0; i< (NX_PPP_NAME_SIZE-1); i++)
   {
      system[i] =  system_string[i];
   }
   system[i] =  0;
   
   for (i = 0; i< (NX_PPP_NAME_SIZE-1); i++)
   {
      secret[i] =  secret_string[i];
   }
   secret[i] =  0;
   
   return(NX_SUCCESS);  
}

```
## nx_ppp_create

Create a PPP instance

### Prototype

```c
UINT  nx_ppp_create(
    NX_PPP *ppp_ptr,
    CHAR *name,
    NX_IP *ip_ptr, 
    VOID *stack_memory_ptr,
    ULONG stack_size, 
    UINT thread_priority,
    NX_PACKET_POOL *pool_ptr,
    void (*ppp_invalid_packet_handler)(NX_PACKET *packet_ptr),
    void (*ppp_byte_send)(UCHAR byte));
```

### Description

This service creates a PPP instance for the specified NetX Duo IP instance. This function must be called prior to creating the NetX Duo IP instance.

> **Note:** It is generally a good idea to create the NetX Duo IP thread at a higher priority than the PPP thread priority. Please refer to the *nx_ip_create* service for more information on specifying the IP thread priority.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *name*: Name of this PPP instance.
- *ip_ptr*: Pointer to control block for not-yet-created IP instance.
- *stack_memory_ptr*: Pointer to start of PPP thread's stack area.
- *stack_size*: Size in bytes in the thread's stack.
- *pool_ptr*: Pointer to default packet pool.
- *thread_priority*: Priority of internal PPP threads (1-31).
- *ppp_invalid_packet_handler*: Function pointer to application's handler for all non-PPP packets. The NetX Duo PPP typically calls this routine during initialization. This is where the application can respond to modem commands or in the case of Windows XP, the NetX Duo PPP application can initiate PPP by responding with "CLIENT SERVER" to the initial "CLIENT" sent by Windows XP.
- *ppp_byte_send*: Function pointer to application's serial byte output routine.


### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP create.
- NX_PTR_ERROR: (0x07) Invalid PPP, IP, or byte output function pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Initialization, threads

### Example

```c
/* Create "my_ppp" for IP instance "my_ip". */
status =  nx_ppp_create(&my_ppp, "my PPP", &my_ip, stack_start, 1024, 2, 
                        &my_pool, my_invalid_packet_handler, my_out_byte);

/* If status is NX_SUCCESS the PPP instance was successfully
   created. */
```

## nx_ppp_delete

Delete a PPP instance

### Prototype

```c
UINT nx_ppp_delete(NX_PPP *ppp_ptr);
```

### Description

This service deletes the previously created PPP instance.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP deletion.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Delete PPP instance "my_ppp". */
status =  nx_ppp_delete(&my_ppp);

/* If status is NX_SUCCESS the "my_ppp" was successfully deleted. */
```

## nx_ppp_dns_address_get

Get DNS Server IP address

### Prototype

```c
UINT nx_ppp_dns_address_get(
    NX_PPP *ppp_ptr,
    ULONG *dns_address_ptr);
```

### Description

This service retrieves the DNS IP address supplied by the peer in the IPCP handshake. If no IP address was supplied by the peer, an IP address of 0 is returned.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *dns_address_ptr*: Destination for DNS server address

### Return Values

- **NX_SUCCESS**: (0x00) Successful DNS address get.
- **NX_PPP_NOT_ESTABLISHED**: (0xB5) PPP has not completed negotiation with peer.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Initialization, threads, timers, ISRs

### Example

```c

ULONG  my_dns_address;

/* Get DNS Server address supplied by peer. */
status =  nx_ppp_dns_address_get(&my_ppp, &my_dns_address);

/* If status is NX_SUCCESS the "my_dns_address" contains the DNS IP address – 
   if the peer supplied one. */
```

## nx_ppp_secondary_dns_address_get

Get Secondary DNS Server IP address

### Prototype

```c
UINT nx_ppp_secondary_dns_address_get(
    NX_PPP *ppp_ptr,
    ULONG *dns_address_ptr);
```

### Description

This service retrieves the secondary DNS IP address supplied by the peer in the IPCP handshake. If no IP address was supplied by the peer, an IP address of 0 is returned.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *dns_address_ptr*: Destination for Secondary DNS server address

### Return Values

- **NX_SUCCESS**: (0x00) Successful DNS address get.
- **NX_PPP_NOT_ESTABLISHED**: (0xB5) PPP has not completed negotiation with peer.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Initialization, threads, timers, ISRs

### Example

```c
ULONG  my_dns_address;

/* Get secondary DNS Server address supplied by peer. */
status =  nx_ppp_secondary_dns_address_get(&my_ppp, &my_dns_address);

/* If status is NX_SUCCESS the "my_dns_address" contains the secondary DNS Server address – if the peer supplied one. */
```

## nx_ppp_dns_address_set

Set primary DNS Server IP address

### Prototype

```c
UINT nx_ppp_dns_address_set(
    NX_PPP *ppp_ptr,
    ULONG dns_address);
```

### Description

This service sets the DNS Server IP address. If the peer sends a DNS Server option request in the IPCP state, this host will provide the information.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *dns_address*: DNS server address

### Return Values

- **NX_SUCCESS**: (0x00) Successful DNS address set.
- **NX_PPP_NOT_ESTABLISHED**: (0xB5) PPP has not completed negotiation with peer.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Initialization, threads

### Example

```c

ULONG  my_dns_address = IP_ADDRESS(1,2,3,1);

/* Set DNS Server address. */
status =  nx_ppp_dns_address_set(&my_ppp, my_dns_address);

/* If status is NX_SUCCESS the "my_dns_address" will be the DNS Server address provided if the peer requests one. */

```

## nx_ppp_secondary_dns_address_set

Set secondary DNS Server IP address

### Prototype

```c
UINT nx_ppp_secondary_dns_address_set(
    NX_PPP *ppp_ptr,
    ULONG dns_address);
```

### Description

This service sets the secondary DNS Server IP address. If the peer sends a secondary DNS Server option request in the IPCP state, this host will provide the information.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *dns_address*: Secondary DNS server address

### Return Values

- **NX_SUCCESS**: (0x00) Successful DNS address set. 
- **NX_PPP_NOT_ESTABLISHED**: (0xB5) PPP has not completed negotiation with peer.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Initialization, threads

### Example

```c
ULONG  my_dns_address = IP_ADDRESS(1,2,3,1);

/* Set DNS Server address. */
status =  nx_ppp_secondary_dns_address_set(&my_ppp, my_dns_address);

/* If status is NX_SUCCESS the "my_dns_address" will be the secondary DNS Server address provided if the peer requests one. */

```
## nx_ppp_interface_index_get

Get IP interface index

### Prototype

```c
UINT nx_ppp_interface_index_get(
    NX_PPP *ppp_ptr,
    UINT *index_ptr);
```

### Description

This service retrieves the IP interface index associated with this PPP instance. This is only useful when the PPP instance is not the primary interface of an IP instance.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *index_ptr*: Destination for interface index

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP index get.
- **NX_IN_PROGRESS**: (0x37) PPP has not completed initialization.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Initialization, threads

### Example

```c
ULONG  my_index;

/* Get the interface index for this PPP instance. */
status =  nx_ppp_interface_index_get(&my_ppp, &my_index);

/* If status is NX_SUCCESS the "my_index" contains the IP interface index for
   this PPP instance. */

```
## nx_ppp_ip_address_assign

Assign IP addresses for IPCP

### Prototype

```c
UINT nx_ppp_ip_address_assign(
    NX_PPP *ppp_ptr,
    ULONG local_ip_address, 
    ULONG peer_ip_address);
```

### Description

This service sets up the local and peer IP addresses for use in the Internet Protocol Control Protocol (IPCP. It should be called for the PPP instance that has valid IP addresses for itself and the other peer.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *local_ip_address*: Local IP address.
- *peer_ip_address*: Peer's IP address.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP address assignment.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Initialization, threads

### Example

```c
/* Set IP addresses for "my_ppp". */
status =  nx_ppp_ip_address_assign(&my_ppp, IP_ADDRESS(256,2,2,187), 
IP_ADDRESS(256,2,2,188));


/* If status is NX_SUCCESS the "my_ppp" has the IP addresses. */
```

## nx_ppp_link_down_notify

Notify application on link down

### Prototype

```c
UINT nx_ppp_link_down_notify(
    NX_PPP *ppp_ptr, 
    VOID (*link_down_callback)(NX_PPP *ppp_ptr));
```

### Description

This service registers the application's link down notification callback with the specified PPP instance. If non-NULL, the application's link down callback function is called whenever the link goes down.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *link_down_callback*: Application's link down notification function pointer. If NULL, link down notification is disabled.

### Return Values

- **NX_SUCCESS**: (0x00) Successful link down notification callback registration.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Initialization, threads, timers, ISRs

### Example

```c
/* Register "my_link_down_callback" to be called whenever the PPP
   link goes down. */
status =  nx_ppp_link_down_notify(&my_ppp, my_link_down_callback);


/* If status is NX_SUCCESS the function "my_link_down_callback" has been
   registered with this PPP instance. */

VOID my_link_down_callback(NX_PPP *ppp_ptr)
{

/* On link down, simply restart PPP.  */
    nx_ppp_restart(ppp_ptr);
} 
```
## nx_ppp_link_up_notify

Notify application on link up

### Prototype

```c
UINT nx_ppp_link_up_notify(
    NX_PPP *ppp_ptr, 
    VOID (*link_up_callback)(NX_PPP *ppp_ptr));
```
### Description

This service registers the application's link up notification callback with the specified PPP instance. If non-NULL, the application's link up callback function is called whenever the link comes up.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *link_up_callback*: Application's link up notification function pointer. If NULL, link up notification is disabled.**

### Return Values

- **NX_SUCCESS**: (0x00) Successful link up notification callback registration.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Initialization, threads, timers, ISRs

### Example

```c
/* Register "my_link_up_callback" to be called whenever the PPP
   link comes up. */
status =  nx_ppp_link_up_notify(&my_ppp, my_link_up_callback);


/* If status is NX_SUCCESS the function "my_link_up_callback" has been
   registered with this PPP instance. */

VOID my_link_up_callback(NX_PPP *ppp_ptr)
{
    /* On link up, the application my want to start sending/receiving
       UPD/TCP data.  */
}
```

## nx_ppp_nak_authentication_notify

Notify application if authentication NAK received

### Prototype

```c
UINT    nx_ppp_nak_authentication_notify(
    NX_PPP *ppp_ptr, 
    void (*nak_authentication_notify)(void));
```

### Description

This service registers the application's authentication nak notification callback with the specified PPP instance. If non-NULL, this callback function is called whenever the PPP instance receives a NAK during authentication.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *nak_authentication_notify*: Pointer to function called when the PPP instance receives an authentication NAK. If NULL, the notification is disabled.

### Return Values

- **NX_SUCCESS**: (0x00) Successful notification callback registration.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Initialization, threads, timers, ISRs

### Example

```c
/* Register "my_nak_auth_callback" to be called whenever the PPP
   receives a NAK during authentication. */
status =  nx_ppp_nak_authentication_notify(&my_ppp, my_nak_auth_callback);

/* If status is NX_SUCCESS the function "my_nak_auth_callback" has been
   registered with this PPP instance. */

VOID my_nak_auth_callback(NX_PPP *ppp_ptr)
{
   /* Handle the situation of receiving an authentication NAK */
}

```

## nx_ppp_pap_enable

Enable PAP Authentication

### Prototype

```c

UINT  nx_ppp_pap_enable(
    NX_PPP *ppp_ptr, 
    UINT (*generate_login)(CHAR *name, CHAR *password),
    UINT (*verify_login)(CHAR *name, CHAR *password));
```

### Description

This service enables the Password Authentication Protocol (PAP) for the specified PPP instance. If the "***verify_login***" function pointer is specified, PAP is required by this PPP instance. Otherwise, PAP only responds to the peer's PAP requirements as specified during LCP negotiation.

There are several data items referenced below in the required callback functions. The data item *name* is expected to be NULL-terminated string with a maximum size of NX_PPP_NAME_SIZE-1. The data item *password* is also expected to be a NULL-terminated string with a maximum size of NX_PPP_PASSWORD_SIZE-1.

> **Note:** This function must be called after *nx_ppp_create* but before *nx_ip_create* or *nx_ip_interface_attach*.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *generate_login*: Pointer to application function that produces a *name* and *password* for authentication by the peer. Note that the *name* and *password* values must be copied into the supplied destinations.
- *verify_login*: Pointer to application function that verifies the *name* and *password* supplied by the peer. This routine must compare the supplied *name* and *password*. If this routine returns NX_SUCCESS, the name and password are correct and PPP can proceed to the next step. Otherwise, this routine returns NX_PPP_ERROR and PPP simply waits for another name and password.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP PAP enable.
- **NX_NOT_IMPLEMENTED**: (0x80) PAP logic was disabled via NX_PPP_DISABLE_PAP.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer or application function pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Initialization, threads

### Example

```c
CHAR    name_string[] = "username";
CHAR    password_string[] =  "password";

/* Enable PAP for PPP instance "my_ppp". */
status =  nx_ppp_pap_enable(&my_ppp, my_generate_login, my_verify_login);

/* If status is NX_SUCCESS the "my_ppp" now has PAP enabled. */

/* Define callback routines for PAP enable.  */

UINT  generate_login(CHAR *name, CHAR *password)
{

UINT    i;

for (i = 0; i< (NX_PPP_NAME_SIZE-1); i++)
name[i] = name_string[i];
name[i] =  0;

for (i = 0; i< (NX_PPP_PASSWORD_SIZE-1); i++)
password[i] = password_string[i];
password_string[i] =  0;

return(NX_SUCCESS);  
}

UINT  verify_login(CHAR *name, CHAR *password)
{

/* Assume name and password are correct. Normally, 
a comparison would be made here!  */
printf("Name: %s, Password: %s\n", name, password);

return(NX_SUCCESS);  
}
```

## nx_ppp_ping_request

Send an LCP ping request

### Prototype

```c
UINT  nx_ppp_ping_request(
    NX_PPP *ppp_ptr,
    CHAR *data, 
    UINT data_size,
    ULONG wait_option);
```

### Description

This service sends an LCP request and sets a flag that the PPP device is waiting for an echo response. The wait option is primarily for the *nx_packet_allocate* call. The service returns as soon as the request is sent. It does not wait for a response. 

When a matching echo response is received, the PPP thread task will clear the flag. The PPP device must have completed the LCP part of the PPP negotiation.

This service is useful for PPP set ups where polling the hardware for link status may not be readily possible.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *data*: Pointer to data to send in echo request.
- *data_size*: Size of data to send wait_option Time to wait to send the LCP echo message.

### Return Values

- **NX_SUCCESS**: (0x00) Successful sent echo request.
- **NX_PPP_NOT_ESTABLISHED**: (0xB5) PPP connection not established.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer or application function pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Application threads

### Example

```c

CHAR    buffer[] = "username";
UINT    buffer_length =  sizeof("username ") - 1;

/* Send an LCP ping request". */
status =  nx_ppp_ping_request(&my_ppp, &buffer[0], buffer_length, 200);

/* If status is NX_SUCCESS the LCP echo request was successfully sent. Now wait to 
   receive a response. */

while(my_ppp.nx_ppp_lcp_echo_reply_id > 0)
{
	tx_thread_sleep(100);
}

/* Got a valid reply! */
```

## nx_ppp_raw_string_send

Send a raw ASCII string

### Prototype

```c
UINT  nx_ppp_raw_sting_send(
    NX_PPP *ppp_ptr,
    CHAR *string_ptr);
```

### Description

This service sends a non-PPP ASCII string directly out the PPP interface. It is typically used after PPP receives an non-PPP packet that contains modem control information.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *string_ptr*: Pointer to string to send.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP raw string send.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer or string pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c

/* Send "CLIENTSERVER" to "CLIENT" sent by Windows 98 before PPP is
initiated.  */
status =  nx_ppp_raw_string_send(&my_ppp, "CLIENTSERVER");

/* If status is NX_SUCCESS the raw string was successfully Sent via PPP. */
```
## nx_ppp_restart

Restart PPP processing

### Prototype

```c
UINT  nx_ppp_restart(NX_PPP *ppp_ptr);
```

### Description

This service restarts the PPP processing. It is typically called when the link needs to be re-established either from a link down callback or by a non-PPP modem message indicating communication was lost.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP restart initiated.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Restart the PPP instance "my_ppp".  */
status =  nx_ppp_restart(&my_ppp);

/* If status is NX_SUCCESS the PPP instance has been restarted. */
```

## nx_ppp_start

Start PPP processing

### Prototype

```c
UINT  nx_ppp_start(NX_PPP *ppp_ptr);
```

### Description

This service starts the PPP processing. It is typically called after nx_ppp_stop() called.

> **Note:** PPP automatically starts the PPP processing when the link is enabled.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP start initiated. 
- **NX_PPP_ALREADY_STARTED**: (0xb9) PPP already started.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Start the PPP instance "my_ppp".  */
status =  nx_ppp_start(&my_ppp);

/* If status is NX_SUCCESS the PPP instance has been started. */
```

## nx_ppp_status_get

Get current PPP status

### Prototype

```c
UINT  nx_ppp_status_get(
    NX_PPP *ppp_ptr,
    UINT *status_ptr);
```
### Description

This service gets the current status of the specified PPP instance.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *status_ptr*: Destination for the PPP status, the following are possible status values:
    - **NX_PPP_STATUS_ESTABLISHED**
    - **NX_PPP_STATUS_LCP_IN_PROGRESS**
    - **NX_PPP_STATUS_LCP_FAILED**
    - **NX_PPP_STATUS_PAP_IN_PROGRESS**
    - **NX_PPP_STATUS_PAP_FAILED**
    - **NX_PPP_STATUS_CHAP_IN_PROGRESS**
    - **NX_PPP_STATUS_CHAP_FAILED**
    - **NX_PPP_STATUS_IPCP_IN_PROGRESS**
    - **NX_PPP_STATUS_IPCP_FAILED**

> **Note:** The status is only valid if the API returns NX_SUCCESS. In addition, if any of the *_FAILED status values are returned, PPP processing is effectively stopped until it is restarted again by the application.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP status request.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Initialization, threads, timers, ISRs

### Example

```c
UINT ppp_status;
UINT status;


/* Get the current status of PPP instance "my_ppp".  */
status =  nx_ppp_status_get(&my_ppp, &ppp_status);

/* If status is NX_SUCCESS the current internal PPP status is contained in
   "ppp_status". */
```
## nx_ppp_stop

Start PPP processing

### Prototype

```c
UINT  nx_ppp_stop(NX_PPP *ppp_ptr);
```

### Description

This service stops the PPP processing. User also can calls nx_ppp_start() to start the PPP processing if needed.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP start initiated. 
- **NX_PPP_ALREADY_STOPPED**: (0xb8) PPP already stopped.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Stop the PPP instance "my_ppp".  */
status =  nx_ppp_stop(&my_ppp);

/* If status is NX_SUCCESS the PPP instance has been stopped. */
```
## nx_ppp_packet_receive

Receive PPP packet

### Prototype

```c
UINT  nx_ppp_packet_receive(
    NX_PPP *ppp_ptr,
    NX_PACKET *packet_ptr);

```

### Description

This service receives PPP packet.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *packet_ptr*: Pointer to PPP packet.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP status request.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Initialization, threads

### Example

```c
/* Receive the PPP packet of PPP instance "my_ppp".  */
status =  nx_ppp_packet_receive(&my_ppp, packet_ptr);

/* If status is NX_SUCCESS the PPP packet has received. */


```
## nx_ppp_packet_send_set

Set the PPP packet send function

### Prototype

```c
UINT  nx_ppp_packet_send_set(
    NX_PPP *ppp_ptr, 
    VOID (*nx_ppp_packet_send)(NX_PACKET *packet_ptr));

```

### Description

This service sets the PPP packet send function.

### Input Parameters

- *ppp_ptr*: Pointer to PPP control block.
- *nx_ppp_packet_send*: Routine to send PPP packet.

### Return Values

- **NX_SUCCESS**: (0x00) Successful PPP status request.
- NX_PTR_ERROR: (0x07) Invalid PPP pointer.

### Allowed From

Initialization, threads

### Example

```c
/* Set the PPP packet send function of PPP instance "my_ppp".  */
status =  nx_ppp_packet_send_set(&my_ppp, nx_ppp_packet_send);

/* If status is NX_SUCCESS the PPP packet send function has set. */


```
