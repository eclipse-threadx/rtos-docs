---
title: Appendix A - Description of the Restore State Feature for NetX Duo DHCPv6 Client
description: "The NetX Duo DHDPv6 Client configuration option, NX_DHCPV6_CLIENT_RESTORE_STATE, allows a system to restore a previously created DHCP Client in a Bound state between system reboots."
---

# Appendix A - Description of the Restore State Feature for NetX Duo DHCPv6 Client

The NetX Duo DHDPv6 Client configuration option, NX_DHCPV6_CLIENT_RESTORE_STATE, allows a system to restore a previously created DHCP Client in a Bound state between system reboots.

This option also allows an application to suspend the DHCPv6 Client thread and resume it, updated with the elapsed time between suspending and resuming the thread without powering down.

## Restoring the DHCPv6 Client between Reboots

To restore a DHCPv6 Client between reboots, the DHCPv6 application creates an instance of the DHCPv6 Client, and then obtains an IP address lease using the normal DHCPv6 protocol and calling *nx_dhcpv6_start*. Then the DHCPv6 application waits for the protocol to complete. If all goes well, the device achieves the BOUND state with an assigned valid IP address from its DHCPv6 Server. Before it powers down, the DHCPv6 Client application saves the current DHCPv6 Client instance to a DHCPv6 Client record which is then stored in non-volatile memory. An independent 'time keeper' elsewhere in the system keeps track of the time elapsed during this powered down state. On powering up, the application creates a new DHCPv6 Client instance, and then updates it with the previously created DHCPv6 Client record. The elapsed time is obtained from the "time keeper" and then applied to the time remaining on the DHCP Clientv6 lease. At this point, the application can resume the DHCPv6 Client.

If the time elapsed during power down puts the DHCPv6 Client state in either a RENEW or REBIND state, the DHCPv6 Client will automatically initiate DHCPv6 messages requesting to renew or rebind the IP address lease. If the IP address is expired, the DHCPv6 Client will automatically clear the IP address on the IP instance and begin the DHCPv6 process from the INIT state, requesting a new IP address.

In this manner the DHCPv6 Client can operate between reboots as if uninterrupted.

Below is an illustration of this feature.

```C
/* On the power up, create an IP instance, DHCPv6 Client, enable ICMPv6 and UDP
   and other resources (not shown) for the DHCPv6 Client/application
   in tx_application_define(). */
 
/* Define the DHCPv6 Client application thread. */     
void    thread_dhcpv6_client_entry(ULONG thread_input)
{

UINT        status;
UINT        time_elapsed = 0;
NX_DHCPV6_CLIENT_RECORD client_my_record;


    /* No previously saved Client record. Start the DHCPv6 Client in the INIT state. */
    status =  nx_dhcpv6_start(&dhcp_0);

    if (status !=NX_SUCCESS)
        return;

    while(1)	
    {
    
        /* Wait for DHCPv6 Client to get the IP address. */
    }

    /* At some point decide we power down the system. */

    /* Save the Client state data which we will subsequently need to restore the DHCPv6    
       Client. */
    status = nx_dhcpv6_client_get_record(&dhcp_0, &client_my_record);               

    /* Copy this memory to non-volatile memory (not shown). */

    /* Delete the IP and DHCPv6 Client instances before powering down. */
    nx_dhcpv6_client_delete(&dhcp_0);

    nx_ip_delete(&ip_0);

    /* Ready to power down, having released other resources as necessary. */

    /* The application has determined there is a previously saved record. We will 
       restore it to the current DHCPv6 Client instance. */

/* Create the IP and DHCPv6 Client instances, enable ICMPv6 and UDP after powering up. */

/* Calculate the time elapsed during power down */

    /* Get the previous Client state data from non-volatile memory. */

    /* Apply the record to the current Client instance. This will also 
       update the IP instance with IP address, mask etc. */
    status = nx_dhcpv6_client_restore_record(&dhcp_0, &client_my_record, time_elapsed);   

     if (status != NX_SUCCESS)
          return;

     /* We are ready to resume the DHCPv6 Client thread and use the assigned IP address. */
     status = nx_dhcpv6_resume(&dhcp_0);

     if (status != NX_SUCCESS)
          return;

}
```

## nx_dhcpv6_client_get_record

Create a record of the current DHCPv6 Client state

### Prototype

```C
ULONG nx_dhcpv6_client_get_record(NX_DHCPV6 *dhcpv6_ptr, 
								  NX_DHCPV6_CLIENT_RECORD *record_ptr);
```

### Description

This service saves the DHCPv6 Client to the record pointed to by record_ptr. This allows the DHCPv6 Client application restore its DHCPv6 Client state after, for example, a power down and reboot.

### Input Parameters

- **dhcpv6_ptr** Pointer to DHCPv6 Client

- **record_ptr** Pointer to DHCPv6 Client record

### Return Values

- **NX_SUCCESS (0x0)** Valid Client record created

- **NX_DHCPV6_NOT_BOUND** (0xE94) Client not in bound state, therefore not assigned valid IP address

- **NX_PTR_ERROR** (0x16) Invalid pointer input

### Allowed From

Threads

### Example

```C
NX_DHCPV6_CLIENT_RECORD dhcpv6_record;


/* Obtain a record of the current client state. */
status=  nx_dhcpv6_client_get_record(&dhcpv6_ptr, &dhcpv6_record);

/* If status is NX_SUCCESS dhcpv6_record contains the current DHCPv6 client record. */
```

### See Also

- nx_dhcpv6_resume
- nx_dhcpv6_suspend
- nx_dhcpv6_client_restore_record

## nx_dhcpv6_client_restore_record

Restore DHCPv6 Client state from saved record

### Prototype

```C
ULONG nx_dhcpv6_client_restore_record(NX_DHCPV6 *dhcpv6_ptr, 
									  NX_DHCPV6_CLIENT_RECORD       
									  *record_ptr, ULONG time_elapsed);
```

### Description

This service enables a DHCPv6 application to recreate its DHCPv6 Client state from a previous session by updating the DHCPv6 Client with the DHCPv6 Client record pointed to by record_ptr, and updates the time remaining on DHCPv6 Client lease with the time_elapsed input. This allows the DHCPv6 Client application to recreate its DHCPv6 Client, for example, after powering down. This requires that the DHCPv6 Client application created a record of the DHCPv6 Client before powering down, and saved that record to non-volatile memory.

### Input Parameters

- **dhcpv6_ptr** Pointer to DHCPv6 Client

- **record_ptr** Pointer to DHCPv6 Client record

- **time_elapsed** Time to subtract from the lease time remaining in the input client record

### Return Values

- **NX_SUCCESS (0x0)** Client record restored

- NX_PTR_ERROR (0x16) Invalid Pointer Input

### Allowed From

Threads

### Example

```C
NX_DHCPV6_CLIENT_RECORD dhcpv6_record;
ULONG 		time_elapsed;

/* Obtain time (timer ticks) elapsed from independent time keeper. */
time_elapsed = /* to be determined by application */ 1000; 

/* Obtain a record of the current client state. */
status=  nx_dhcpv6_client_restore_record(&dhcpv6_ptr, &dhcpv6_record, time_elapsed);

/* If status is NX_SUCCESS the current DHCPv6 Client pointed to by dhcpv6_ptr contains the current client record updated for time elapsed during power down. */
```

### See Also

- nx_dhcpv6_client_get_record