---
title: Appendix A - Description of the Restore state feature for NetX Duo DHCP Client services
description: This chapter contains a description of the Restore state feature for NetX Duo DHCP Client services.
---

# Appendix A : Description of the Restore state feature for NetX Duo DHCP Client services

The NetX Duo DHCP Client configuration option, NX_DHCP_CLIENT_RESTORE_STATE, allows a system to restore a previously created DHCP Client Record in a Bound state between system reboots.

When this option is enabled, the application can suspend and resume the DHCP Client thread. There is also a service to update the DHCP Client with the elapsed time between suspending and resuming the thread.

## Restoring the DHCP Client between Reboots

Before restoring a DHCP Client after rebooting, a previously created DHCP Client that must reach the Bound state and be is assigned an IP address from the DHCP server. Before it powers down, the DHCP application must then save the current DHCP Client record to non-volatile memory. There must also be an independent 'time keeper' elsewhere in the system to keep track of the time elapsed during this powered down state. On powering up, the application creates a new DHCP Client instance, and then updates it with the previously created DHCP Client record. The elapsed time is obtained from the "time keeper" and then applied to the time remaining on the DHCP Client lease. Note that this may cause the DHCP Client to change states e.g. from BOUND to RENEWING. At this point, the application can resume the DHCP Client.

If the time elapsed during power down puts the DHCP Client state in either a RENEW or REBIND state, the DHCP Client will automatically initiate DHCP messages requesting to renew or rebind the IP address lease. If the IP address is expired, the DHCP Client will automatically clear the IP address on the IP instance and begin the DHCP process from the INIT state, requesting a new IP address.

In this manner the DHCP Client can operate between reboots as if uninterrupted.

Below is an illustration of this feature. This assumes DHCP Client is running only on the primary interface.

```c
/* On the power up, create an IP instance, DHCP Client, enable ICMP and UDP
   and other resources (not shown) for the DHCP Client/application
   in tx_application_define().  */
 
/* Define the DHCP application thread. */     
void    thread_dhcp_client_entry(ULONG thread_input)
{

UINT        status;
UINT        time_elapsed = 0;
NX_DHCP_CLIENT_RECORD client_nv_record;


if (/* The application checks if there is a previously saved DHCP Client record. */)
{
    /* No previously saved Client record. Start the DHCP Client in the INIT state.  */
    status =  nx_dhcp_start(&dhcp_0);

    if (status !=NX_SUCCESS)
        return;

    do
    {
        /* Wait for DHCP to assign the IP address.  */
    } while (status != NX_SUCCESS);

    /* We have a valid IP address. */
    /* At some point decide we power down the system. */
    /* Save the Client state data which we will subsequently need to restore the DHCP    
       Client. */
    status = nx_dhcp_client_get_record(&dhcp_0, &client_nv_record);               

    /* Copy this memory to non-volatile memory (not shown). */
    /* Delete the IP and DHCP Client instances before powering down. */
    nx_dhcp_delete(&dhcp_0);

    nx_ip_delete(&ip_0);
    /* Ready to power down, having released other resources as necessary. */

}
else
{

    /* The application has determined there is a previously saved record. We will 
        restore it to the current DHCP Client instance.  */

    /* Get the previous Client state data from non-volatile memory. */

    /* Apply the record to the current Client instance. This will also 
        update the IP instance with IP address, mask etc. */
    status = nx_dhcp_client_restore_record(&dhcp_0, &client_nv_record, time_elapsed);   

    if (status != NX_SUCCESS)
        return;

    /* We are ready to resume the DHCP Client thread and use the assigned IP address. */
    status = nx_dhcp_resume(&dhcp_0);

    if (status != NX_SUCCESS)
        return;

}
```
## Resuming the DHCP Client Thread after Suspension 

To suspend a DHCP Client thread without powering down, the application calls *nx_dhcp_suspend* on a DHCP Client which has achieved the BOUND state and which has a valid IP address. When it is ready to resume the DHCP Client it first calls *nx_dhcp_client_update_time_remaining* to update the time remaining on the DHCP address lease (obtaining the time elapsed from an independent time keeper). Then it calls the *nx_dhcp_resume* to resume the DHCP Client thread.

If the time elapsed puts the DHCP Client state in either a RENEW or REBIND state, the DHCP Client will automatically initiate DHCP messages requesting to renew or rebind the IP address lease. If the IP address is expired, the DHCP Client will automatically clear the IP address and begin the DHCP process from the INIT state, requesting a new IP address.

Below is an illustration of using this feature.

```c
/* Create an IP instance, DHCP Client, enable ICMP and UDP
   and other resources (not shown) typically in tx_application_define().  */
 
/* Define the DHCP application thread. */     
void    thread_dhcp_client_entry(ULONG thread_input)
{

  /* Start the DHCP Client.  */
  status =  nx_dhcp_start(&dhcp_0);

  if (status !=NX_SUCCESS)
    return;

  while(1)
  {
   /* Wait for DHCP to obtain an IP address.  */
  }

  /* Do tasks with the IP address e.g. send pings to another host on the network...  */
  status =  nx_icmp_ping(…);

  if (status !=NX_SUCCESS)
          printf("Failed %d byte Ping!\n", length);

  /* At some later time, suspend the DHCP Client e.g. the device is going to low 
   power mode (sleep) so we do not want any threads to wake it up. */

  nx_dhcp_suspend(&dhcp_0);  

  /* During this suspended state, an independent timer is keeping track of the elapsed      
     time. */


  /* At some point, we are ready to resume the DHCP Client thread. */

  /* Update the DHCP Client lease time remaining with the time elapsed. */
  status = nx_dhcp_client_update_time_remaining(&dhcp_0, time_elapsed);   

  if (status != NX_SUCCESS)
       return;

  /* We now can resume the DHCP Client thread. */
  status = nx_dhcp_resume(&dhcp_0);

  if (status != NX_SUCCESS)
       return;

  /* Resume tasks e.g. ping another host. */
  status =  nx_icmp_ping(…);

}

```
Below is a list of services for restoring a DHCP Client state from memory and for suspending and resuming the DHCP Client.

## nx_dhcp_client_get_record

Create a record of the current DHCP Client state

### Prototype

```c
ULONG nx_dhcp_ client_get_record(NX_DHCP *dhcp_ptr, NX_DHCP_CLIENT_RECORD *record_ptr);
```

### Description

This service saves the DHCP Client running on the first interface enabled for DHCP found on the DHCP Client instance to the record pointed to by record_ptr. This allows the DHCP Client application restore its DHCP Client state after, for example, a power down and reboot.

To save a DHCP Client record on a specific interface if more than one interface is enabled for DHCP, use the *nx_dhcp_interface_client_get_record* service.

### Input Parameters

- **dhcp_ptr**: Pointer to DHCP Client
- **record_ptr**: Pointer to DHCP Client record

### Return Values

- **NX_SUCCESS (0x0)**: Client record created
- **NX_DHCP_NOT_BOUND**: (0x94) Client not in Bound states
- **NX_DHCP_NO_INTERFACES_ENABLED**: (0xA5) No interfaces enabled for DHCP
- NX_PTR_ERROR: (0x16) Invalid pointer input

### Allowed From

Threads

### Example

```c
NX_DHCP_CLIENT_RECORD dhcp_record;

/* Obtain a record of the current client state. */
status=  nx_dhcp_client_get_record(dhcp_ptr, &dhcp_record);

/* If status is NX_SUCCESS dhcp_record contains the current DHCP client record. */
```

## nx_dhcp_interface_client_get_record

Create a record of the current DHCP Client state on the specified interface

### Prototype

```c
ULONG nx_dhcp_interface_client_get_record(NX_DHCP *dhcp_ptr, UINT interface_index,
                                          NX_DHCP_CLIENT_RECORD *record_ptr);
```

### Description

This service saves the DHCP Client running on the specified interface to the record pointed to by record_ptr. This allows the DHCP Client application restore its DHCP Client state after, for example, a power down and reboot.

### Input Parameters

- **dhcp_ptr**: Pointer to DHCP Client
- **interface_index**: Index on which to get record
- **record_ptr**: Pointer to DHCP Client record

### Return Values

- **NX_SUCCESS (0x0)**: Client record created
- **NX_DHCP_NOT_BOUND**: (0x94) **Client not in Bound state**
- **NX_DHCP_BAD_INTERFACE_INDEX_ERROR**: (0x9A) Invalid interface index
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
NX_DHCP_CLIENT_RECORD dhcp_record;
/* Obtain a record of the current client state on interface 1. */
status=  nx_dhcp_interface_client_get_record(dhcp_ptr, 1, &dhcp_record);

/* If status is NX_SUCCESS dhcp_record contains the current DHCP client record. */
```

## nx_dhcp_ client_restore_record

Restore DHCP Client from a previously saved record

### Prototype

```c
ULONG nx_dhcp_client_restore_record(NX_DHCP *dhcp_ptr, 
                                    NX_DHCP_CLIENT_RECORD *record_ptr, 
                                    ULONG time_elapsed);

```

### Description

This service enables an application to restore its DHCP Client from a previous session using the DHCP Client record pointed to by record_ptr. The time_elapsed input is applied to the time remaining on DHCP Client lease.

This requires that the DHCP Client application created a record of the DHCP Client before powering down, and saved that record to nonvolatile memory.

If more than one interface is enabled for DHCP Client, this service is applied to the first valid interface found in the DHCP Client instance.

### Input Parameters

- **dhcp_ptr**: Pointer to DHCP Client
- **record_ptr**: Pointer to DHCP Client record 
- **time_elapsed**: Time to subtract from the lease time remaining in the input client record

### Return Values

- **NX_SUCCESS**: (0x0) Client record restored
- **NX_DHCP_NO_INTERFACES_ENABLED**: (0xA5) No interfaces running DHCP
- NX_PTR_ERROR: (0x16) Invalid pointer Input

### Allowed From

Threads

### Example

```c
NX_DHCP_CLIENT_RECORD dhcp_record;
ULONG 		time_elapsed;

/* Obtain time (timer ticks) elapsed from independent time keeper. */
Time_elapsed = /* to be determined by application */ 1000; 

/* Obtain a record of the current client state. */
status=  nx_dhcp_client_restore_record(client_ptr, &dhcp_record, time_elapsed);

/* If status is NX_SUCCESS the current DHCP Client pointed to by dhcp_ptr  contains the current client record updated for time elapsed during power down. */
```

## nx_dhcp_interface_client_restore_record

Restore DHCP Client from a previously saved record on specified interface

### Prototype

```c
ULONG nx_dhcp_interface_client_restore_record(NX_DHCP *dhcp_ptr, 
                                              NX_DHCP_CLIENT_RECORD *record_ptr, 
                                              ULONG time_elapsed);
```

### Description

This service enables an application to restore its DHCP Client on the specified interface using the DHCP Client record pointed to by record_ptr. The time_elapsed input is applied to the time remaining on DHCP Client lease.

This requires that the DHCP Client application created a record of the DHCP Client before powering down, and saved that record to nonvolatile memory.

If more than one interface is enabled for DHCP Client, this service is applied to the first valid interface found in the DHCP Client instance.

### Input Parameters

- **dhcp_ptr**: Pointer to DHCP Client
- **record_ptr**: Pointer to DHCP Client record 
- **time_elapsed**: Time to subtract from the lease time remaining in the input client record

### Return Values

- **NX_SUCCESS**: (0x0) Client record restored
- **NX_DHCP_NOT_BOUND**: (0x94) Client not bound to IP address
- **NX_DHCP_BAD_INTERFACE_INDEX_ERROR**: (0x9A) Invalid interface index
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
NX_DHCP_CLIENT_RECORD dhcp_record;
ULONG 		time_elapsed;

/* Obtain time (timer ticks) elapsed from independent time keeper. */
Time_elapsed = /* to be determined by application */ 1000; 


/* Obtain a record of the current client state on the primary interface. */
status=  nx_dhcp_interface_client_restore_record(client_ptr, 0, &dhcp_record, time_elapsed);

/* If status is NX_SUCCESS the current DHCP Client pointed to by dhcp_ptr  contains the current client record updated for time elapsed during power down. */
```

## nx_dhcp_ client_update_time_remaining

Update the time remaining on DHCP Client lease

### Prototype

```c
ULONG nx_dhcp_client_update_time_remaining(NX_DHCP *dhcp_ptr, ULONG time_elapsed);
```

### Description

This service updates the time remaining on the DHCP Client IP address lease with the time_elapsed input on the first interface enabled for DHCP found on the DHCP Client instance. The application must suspend the DHCP Client thread before using this service using *nx_dhcp_suspend*. After calling this service, the application can resume the DHCP Client thread by calling *nx_dhcp_resume*.

This is intended for DHCP Client applications that need to suspend the DHCP Client thread for a period of time, and then update the IP address lease time remaining.

> **Note:** This service is not intended to be used with *nx_dhcp_client_get_record* and *nx_dhcp_client_restore_record* described previously). These services are previously described in this section.

### Input Parameters

- **dhcp_ptr**: Pointer to DHCP Client 
- **time_elapsed**: Time to subtract from the time remaining on the IP address lease

### Return Values

- **NX_SUCCESS**: (0x0) Client IP lease updated
- **NX_DHCP_NO_INTERFACES_ENABLED**: (0xA5) No interfaces enabled for DHCP
- NX_PTR_ERROR: (0x16) Invalid Pointer Input

### Allowed From

Threads

### Example

```c
ULONG 		time_elapsed;

/* Obtain time (timer ticks) elapsed from independent time keeper. */
time_elapsed = /* to be determined by application */ 1000; 


/* Apply the elapsed time to the DHCP Client address lease. */
status=  nx_dhcp_client_update_time_remaining(client_ptr, time_elapsed);

/* If status is NX_SUCCESS the DHCP Client is updated for time elapsed. */
```

## nx_dhcp_interface_client_update_time_remaining

Update the time remaining on DHCP Client lease on the specified interface

### Prototype

```c
ULONG nx_dhcp_interface_client_update_time_remaining(NX_DHCP *dhcp_ptr,
       						                         UINT interface_index,
                                                     ULONG time_elapsed);
```

### Description

This service updates the time remaining on the DHCP Client IP address lease with the time_elapsed input on the specified interface if that interface is enabled for DHCP. The application must suspend the DHCP Client thread before using this service using *nx_dhcp_suspend*. After calling this service, the application can resume the DHCP Client thread by calling *nx_dhcp_resume*. Note suspending and resuming the DHCP Client thread applies to all interfaces enabled for DHCP.

This is intended for DHCP Client applications that need to suspend the DHCP Client thread for a period of time, and then update the IP address lease time remaining.

> **Note:** This service is not intended to be used with *nx_dhcp_client_get_record* and *nx_dhcp_client_restore_record* described previously). These services are previously described in this section.

### Input Parameters

- **dhcp_ptr**: Pointer to DHCP Client
- **interface_index**: Index to interface to apply elapsed time to
- **time_elapsed**: Time to subtract from the time remaining on the IP address lease

### Return Values

- **NX_SUCCESS**: (0x0) Client IP lease updated
- **NX_DHCP_BAD_INTERFACE_INDEX_ERROR**: (0x9A) Invalid interface index
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
ULONG 		time_elapsed;

/* Obtain time (timer ticks) elapsed from independent time keeper. */
time_elapsed = /* to be determined by application */ 1000; 


/* Apply the elapsed time to the DHCP Client address lease on interface 1. */
status=  nx_dhcp_interface_client_update_time_remaining(client_ptr, 1, time_elapsed);

/* If status is NX_SUCCESS the DHCP Client is updated for time elapsed. */

```

## nx_dhcp_suspend

Suspend the DHCP Client thread

### Prototype

```c
ULONG nx_dhcp_suspend(NX_DHCP *dhcp_ptr);
```

### Description

This service suspends the current DHCP Client thread. Note that unlike *nx_dhcp_stop*, there is no change to the DHCP Client state when this service is called.

This service suspends DHCP running on all interfaces enabled for DHCP.

To update the DHCP Client state with elapsed time while the DHCP Client is suspended, see the *nx_dhcp_client_update_time_remaining* described previously. To resume a suspended DHCP Client thread, the application should call *nx_dhcp_resume*.

### Input Parameters

- **dhcp_ptr**: Pointer to DHCP Client

### Return Values

- **NX_SUCCESS**: (0x0) Client thread is suspended
- NX_PTR_ERROR: (0x16) Invalid pointer Input

### Allowed From

Threads

### Example

```c
/* Pause the DHCP client thread. */
status=  nx_dhcp_suspend(client_ptr);

/* If status is NX_SUCCESS the current DHCP Client thread is paused. */
```

## nx_dhcp_resume

Resume a suspended DHCP Client thread

### Prototype

```c
ULONG nx_dhcp_resume(NX_DHCP *dhcp_ptr);
```

### Description

This service resumes a suspended DHCP Client thread. Note that there is no change to the actual DHCP Client state after resuming the Client thread. To update the time remaining on the DHCP Client IP address lease with elapsed time before calling *nx_dhcp_resume*, see the *nx_dhcp_client_update_time_remaining* described previously.

This service resumes DHCP running on all interfaces enabled for DHCP.

### Input Parameters

- **dhcp_ptr**: Pointer to DHCP Client

### Return Values

- **NX_SUCCESS**: (0x0) Client thread is resumed
- NX_PTR_ERROR: (0x16) Invalid pointer Input

### Allowed From

Threads

### Example

```c
/* Resume the DHCP client thread. */
status=  nx_dhcp_resume(client_ptr);

/* If status is NX_SUCCESS the current DHCP Client thread is resumed. */
```