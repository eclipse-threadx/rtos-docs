---
title: Chapter 3 - Description of NetX Duo SNMP agent services
description: This chapter contains a description of all NetX Duo SNMP Agent services (listed below) in alphabetical order.
---


# Chapter 3 - Description of NetX Duo SNMP agent services

This chapter contains a description of all NetX Duo SNMP agent services (listed below) in alphabetical order.

In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_snmp_agent_auth_trap_key_use

Specify authentication key for trap messages

### Prototype

```c
UINT nx_snmp_agent_auth_trap_key_use(
    NX_SNMP_AGENT *agent_ptr, 
    NX_SNMP_SECURITY_KEY *key);
```
### Description

This service specifies the key to be used for setting authentication parameters in the SNMPv3 security header in trap messages. Supplying a NX_NULL value for the key disables authentication.

> **Note:** The key must be previously created. See nx_snmp_agent_md5_key_create or nx_snmp_agent_sha_key_create.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *key* Pointer to a previously created MD5 or SHA key.

### Return Values

- **NX_SUCCESS** (0x00) Successful authentication key set.
- **NX_NOT_ENABLED** (0x14) SNMP Security disabled 
- **NX_PTR_ERROR** (0x07) Invalid SNMP Agent pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Use previously created "my_key" for SNMPv3 trap message authentication.  */
status =  nx_snmp_agent_auth_trap_key_use(&my_agent, &my_key);


/* If status is NX_SUCCESS the SNMP Agent will use "my_key" for
   for authentication parameters in trap messages.  */
```

## nx_snmp_agent_authenticate_key_use
Specify authentication key for response messages

### Prototype

```c
UINT nx_snmp_agent_authenticate_key_use(
    NX_SNMP_AGENT *agent_ptr, 
    NX_SNMP_SECURITY_KEY *key);
```
### Description

This service specifies the key to be used for authentication parameters in the SNMPv3 security parameter for all requests made after it is set. Supplying a NX_NULL value for the key disables authentication.

> **Note:** The key must be previously created. See nx_snmp_agent_md5_key_create or nx_snmp_agent_sha_key_create.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *key* Pointer to a previously created MD5 or SHA key.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP key set.
- **NX_NOT_ENABLED** (0x14) SNMP Security disabled
- **NX_PTR_ERROR** (0x07) Invalid SNMP Agent pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Use previously created "my_key" for SNMPv3 authentication.  */
status =  nx_snmp_agent_authenticate_key_use(&my_agent, &my_key);


/* If status is NX_SUCCESS the SNMP Agent will use "my_key" for
   for setting the authentication parameters of SNMPv3 requests.  */
```

## nx_snmp_agent_community_get
Retrieve community name

### Prototype

```c
UINT nx_snmp_agent_community_get(
    NX_SNMP_AGENT *agent_ptr, 
    UCHAR **community_string_ptr);
```

### Description

This service retrieves the community name from the most recent SNMP request received by the SNMP Agent.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *community_string_ptr* Pointer to a string pointer to return the SNMP Agent community string.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP community get.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
UCHAR *string_ptr;

/* Pickup the community string pointer for my_agent.  */
status =  nx_snmp_agent_community_get(&my_agent, &string_ptr);


/* If status is NX_SUCCESS the pointer "string_ptr" points to the
   last community name supplied to the SNMP agent.  */
```

## nx_snmp_agent_request_get_type_test
Indicate if last SNMP request is GET or SET type

### Prototype

```c
UINT nx_snmp_agent_request_get_type_test(
    NX_SNMP_AGENT *agent_ptr,
    UINT *is_get_type);
```

### Description

This service indicates if the most recent request from the SNMP Manager
is a GET (GET, GETNEXT, or GETBULK) or SET type. It is intended for use
with the username callback where the SNMPv1 or SNMPv2 application will
want to compare the received community string to the SNMP Agent public
string if the request is a GET type, or to the SNMP Agent private string
if the request is a SET type.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *is_get_type* Pointer to request type status:  
NX_TRUE if GET type  
NX_FALSE if SET type

### Return Values

- **NX_SUCCESS** (0x00) Successfully returned type
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
UINT is_get_type;

/* Determine if the current SNMP request is a GET or SET type.  */
status =  nx_snmp_agent_request_get_type_test(&my_agent, &is_get_type);


/* If status is NX_SUCCESS, is_get_type will indicate the request type.  */
```

## nx_snmp_agent_context_engine_set
Set context engine (SNMP v3 only)

### Prototype

```c
UINT nx_snmp_agent_context_engine_set(
    NX_SNMP_AGENT *agent_ptr, 
    UCHAR *context_engine, 
    UINT context_engine_size);
```
### Description

This service sets the context engine of the SNMP Agent. It is only applicable for SNMPv3 processing. This should be called before creating security keys if the application is using authentication and encryption, since the context engine ID is used in the key creation process. If not, NetX Duo SNMP provides a default context engine id at the top of *nxd_snmp.c:*

```c
UCHAR _nx_snmp_default_context_engine[NX_SNMP_MAX_CONTEXT_STRING] =  
    {0x80, 0x00, 0x03, 0x10, 0x01, 0xc0, 0xa8, 0x64, 0xaf};

```

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *context_engine* Pointer to the context engine string.
- *context_engine_size* Size of context engine string. Note that the maximum number of bytes in a context engine is defined by NX_SNMP_MAX_CONTEXT_STRING.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP context engine set.
- **NX_NOT_ENABLED** (0x14) SNMPv3 is not enabled
- **NX_SNMP_ERROR** (0x100) Context engine size error.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
UCHAR my_engine[] = {0x80, 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07};

/* Set the context engine for my_agent.  */
status =  nx_snmp_agent_context_engine_set(&my_agent, my_engine, 9);


/* If status is NX_SUCCESS the context engine has been set.  */
```
## nx_snmp_agent_context_name_set
Set context name (SNMP v3 only)

### Prototype

```c
UINT nx_snmp_agent_context_name_set(
    NX_SNMP_AGENT *agent_ptr, 
    UCHAR *context_name, 
    UINT context_name_size);
```

### Description

This service sets the context name of the SNMP Agent. It is only applicable for SNMPv3 processing. If not called, NetX Duo SNMP Agent will leave the context name blank.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *context_name* Pointer to the context name string.
- *context_name_size* Size of context name string. Note that the maximum number of bytes in a context name is defined by NX_SNMP_MAX_CONTEXT_STRING.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP context name set.
- **NX_SNMP_ERROR** (0x100) Context name size error.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Set the context name for my_agent.  */
status =  nx_snmp_agent_context_name_set(&my_agent, "my_context_name", 15);


/* If status is NX_SUCCESS the context name has been set.  */
```

## nx_snmp_agent_create
Create SNMP agent

### Prototype

```c
UINT nx_snmp_agent_create(
    NX_SNMP_AGENT *agent_ptr, 
    CHAR *snmp_agent_name,
    NX_IP *ip_ptr,
    VOID *stack_ptr, 
    ULONG stack_size,
    NX_PACKET_POOL *pool_ptr,
    UINT (*snmp_agent_username_process)(
        struct NX_SNMP_AGENT_STRUCT *agent_ptr,
        UCHAR *username),
    UINT (*snmp_agent_get_process)(
        struct NX_SNMP_AGENT_STRUCT *agent_ptr,
        UCHAR *object_requested, 
        NX_SNMP_OBJECT_DATA *object_data),
    UINT (*snmp_agent_getnext_process)(
        struct NX_SNMP_AGENT_STRUCT *agent_ptr,
        UCHAR *object_requested, 
        NX_SNMP_OBJECT_DATA *object_data),
    UINT (*snmp_agent_set_process)(
        struct NX_SNMP_AGENT_STRUCT 
        *agent_ptr, UCHAR *object_requested, 
        NX_SNMP_OBJECT_DATA *object_data));
```
### Description

This service creates a SNMP Agent on the specified IP instance.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *snmp_agent_name* Pointer to the SNMP Agent name string.
- *ip_ptr* Pointer to IP instance.
- *stack_ptr* Pointer to SNMP Agent thread stack pointer.
- *stack_size* Stack size in bytes.
- *pool_ptr* Pointer the default packet pool for this SNMP Agent.
- *snmp_agent_username_process* Function pointer to application's username handling routine.
- *snmp_agent_get_process* Function pointer to application's GET request handling routine.
- *snmp_agent_getnext_process* Function pointer to application's GETNEXT request handling routine.
- *snmp_agent_set_process* Function pointer to application's SET request handling routine.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP Agent create.
- **NX_SNMP_ERROR** (0x100) SNMP Agent create error.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
NX_SNMP_AGENT my_agent;

/* Create the SNMP Agent "my_agent."  */
status =  nx_snmp_agent_create(&my_agent, "My SNMP Agent", &ip_0, stack_start_ptr,
                4096, &pool_0, my_username_processing, my_get_processing, 
                my_getnext_processing, my_set_processing);


/* If status is NX_SUCCESS the SNMP Agent "my_agent" has been created.  */
```

## nx_snmp_agent_current_version_get
Get the SNMP packet version

### Prototype

```c
UINT nx_snmp_agent_current_version_get(
    NX_SNMP_AGENT *agent_ptr, 
    UINT *version);
```

### Description

This service retrieves the SNMP version parsed from the most recent SNMP packet received.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *version* Pointer to the SNMP version parsed from received SNMP packet

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP version get
- **NX_PTR_ERROR** (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
UINT          snmp_version;
NX_SNMP_AGENT my_agent;

/* Get the version of the last received SNMP packet. */
status =  nx_snmp_agent_current_version_get (&my_agent, &snmp_version);


/* If status is NX_SUCCESS, snmp_version contains 
   the received packet SNMP version.  */
```

## nx_snmp_agent_private_string_test
Verify private string matches Agent private string

### Prototype

```c
UINT nx_snmp_agent_private_string_test(
    NX_SNMP_AGENT *agent_ptr, 
    UCHAR *community_string,          
    UINT *is_private);
```

### Description

This service compares the null terminated input community string with the SNMP agent private string and indicates if they match.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *community_string* Pointer to string to compare
- *is_private* Pointer to result of comparison  
NX_TRUE - string matches  
NX_FALSE - string does not match

### Return Values

- **NX_SUCCESS** (0x00) Successful comparison
- **NX_PTR_ERROR** (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
UINT    	is_private;
UCHAR   	*community_string_ptr;
NX_SNMP_AGENT 	my_agent;

/* Determine if the community string matches the agent private string */
status =  nx_snmp_agent_private_string_test(&my_agent, community_string_ptr,  
                                            &is_private);


/* If status is NX_SUCCESS, is_private will indicate if there is a match. 
   If is_private is NX_TRUE, they match.  */
```

## nx_snmp_agent_public_string_test
Verify received public string matches Agent's public string

### Prototype

```c
UINT nx_snmp_agent_public_string_test(
    NX_SNMP_AGENT *agent_ptr, 
    UCHAR *community_string,          
    UINT *is_public);
```
### Description

This service compares a null terminated input community string with the SNMP agent public string and indicates if they match.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *community_string* Pointer to string to compare
- *is_public* Pointer to result of comparison  
NX_TRUE - string matches  
NX_FALSE - string does not match

### Return Values

- **NX_SUCCESS** (0x00) Successful comparison
- **NX_PTR_ERROR** (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
UINT    	is_public;
UCHAR   	*community_string_ptr;
NX_SNMP_AGENT 	my_agent;


/* Determine if the community string matches the agent public string */
status =  nx_snmp_agent_public_string_test(&my_agent, community_string_ptr,  
                                           &is_public);


/* If status is NX_SUCCESS, is_public will indicate if there is a match. 
   If is_public is true they match.  */
```

## nx_snmp_agent_version_set
Set the SNMP agent status for each SNMP version

### Prototype

```c
UINT nx_snmp_agent_version_set(
    NX_SNMP_AGENT *agent_ptr, 
    UINT enabled_v1,
    UINT enable_v2, 
    UINT enable_v3);
```

### Description

This service sets the status (enabled/disabled) for each of the SNMP versions, V1, V2 and V3 on the SNMP agent. Note that the user configurable options, NX_SNMP_DISABLE_V1, NX_SNMP_DISABLE_V2, and NX_SNMP_DISABLE_V3, will override these run time settings. By default, the SNMP agent is enabled for all three versions.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *enabled_v1* Sets enabled status for SNMP V1 to on/off.
- *enabled_v2* Sets enabled status for SNMP V2 to on/off.
- *enabled_v3* Sets enabled status for SNMP V3 to on/off.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP version set
- **NX_PTR_ERROR** (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
UINT          v1_on = NX_TRUE;
UINT          v2_on = NX_TRUE;
UINT          v3_on = NX_FALSE;

NX_SNMP_AGENT my_agent;

/* Enable/Disable each SNMP protocol (version) for the SNMP Agent) */
status =  nx_snmp_agent_version_set(&my_agent, v1_on, v2_on, v3_on);


/* If status is NX_SUCCESS, my_agent is enabled only for V1 and V2 assuming 
   NX_SNMP_DISABLE_V1 and NX_SNMP_DISABLE_V2 are not defined. */
```

## nx_snmp_agent_private_string_set
Set the SNMP agent private string

### Prototype

```c
UINT nx_snmp_agent_private_string_set(
    NX_SNMP_AGENT *agent_ptr, 
    UCHAR *community_string);
```

### Description

This service sets the SNMP agent private community string with the input
null terminated string. The default value is NULL (no private string
set), such that any SNMP packet received with a "private" community
string will not be accepted by the SNMP agent for read/write access. The
input string must be less than or equal to the user configurable
NX_SNMP_MAX_USER_NAME-1 (to allow room for null termination) size.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *community_string* Pointer to the private string to assign

### Return Values

- **NX_SUCCESS** (0x00) Successfully set private string
- **NX_SNMP_ERROR_TOOBIG** (0x01) String size too large 
- **NX_PTR_ERROR** (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
NX_SNMP_AGENT my_agent;

/* Set the SNMP agent's private community string */
status =  nx_snmp_agent_private_string_set(&my_agent, "private"));


/* If status is NX_SUCCESS, the SNMP agent private string is set.  */
```

## nx_snmp_agent_public_string_set
Set the SNMP agent public string

### Prototype

```c
UINT nx_snmp_agent_public_string_set(
    NX_SNMP_AGENT *agent_ptr, 
    UCHAR *community_string);
```

### Description

This service sets the SNMP agent public community string with the input
null terminated string. The community string must be less than or equal
to the user configurable NX_SNMP_MAX_USER_NAME-1 (to allow room for
null termination) size.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *community_string* Pointer to the public string to assign

### Return Values

- **NX_SUCCESS** (0x00) Successfully set public string
- **NX_SNMP_ERROR_TOOBIG** (0x01) String size too large
- **NX_PTR_ERROR** (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
NX_SNMP_AGENT my_agent;

/* Set the SNMP agent's public string. */
nx_snmp_agent_public_string_set(&my_agent, "my_public"));


/* If status is NX_SUCCESS, the SNMP agent public string is set.  */
```

## nx_snmp_agent_delete
Delete SNMP agent

### Prototype

```C
UINT nx_snmp_agent_delete(NX_SNMP_AGENT *agent_ptr);
```
### Description

This service deletes a previously created SNMP Agent.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP Agent delete.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Delete the SNMP Agent "my_agent."  */
status =  nx_snmp_agent_delete(&my_agent);


/* If status is NX_SUCCESS the SNMP Agent "my_agent" has been deleted.  */
```

## nx_snmp_agent_set_interface
Set the SNMP agent network interface

### Prototype

```c
UINT nx_snmp_agent_set_interface(
    NX_SNMP_AGENT *agent_ptr,  
    UINT if_index);
```

### Description

This service sets the SNMP network interface for the SNMP Agent as
specified by the input interface index. This is only useful for SNMP
host applications with NetX Duo 5.6 or higher which support multiple
network interfaces. The default value if not specified by the host is
zero, for the primary interface.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *If_index* Index specifying the SNMP interface.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP interface set.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Set the SNMP Agent "my_agent" to the secondary interface.  */
if_index = 1;
status =  nx_snmp_agent_set_interface(&my_agent, if_index);


/* If status is NX_SUCCESS the SNMP agent interface is set.  */
```

## nx_snmp_agent_md5_key_create
Create md5 key (SNMP v3 only)

### Prototype

```c
UINT nx_snmp_agent_md5_key_create(
    NX_SNMP_AGENT *agent_ptr, 
    UCHAR *password, 
    NX_SNMP_SECURITY_KEY *destination_key);
```
### Description

This service creates a MD5 key that can be used for authentication and
encryption.

This service is deprecated. Developers are encouraged to migrate to
*nx_snmp_agent_md5_key_create_extended*.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *password* Pointer to password string.
- *destination_key* Pointer to SNMP key data structure.

### Return Values

- **NX_SUCCESS** (0x00) Successful key create.
- **NX_NOT_ENABLED** (0x14) Security not enabled.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
NX_SNMP_SECURITY_KEY my_key;

/* Create the MD5 key for "my_agent."   */
status =  nx_snmp_agent_md5_key_create(&my_agent, "authpw", &my_key);


/* If status is NX_SUCCESS an MD5 key has been created.  */
```

## nx_snmp_agent_md5_key_create_extended
Create md5 key (SNMP v3 only)

### Prototype

```c
UINT nx_snmp_agent_md5_key_create_extended(
    NX_SNMP_AGENT *agent_ptr, 
    UCHAR *password, 
    UINT password_length,
    NX_SNMP_SECURITY_KEY *destination_key);
```
### Description

This service creates a MD5 key that can be used for authentication and
encryption.

This service replaces *nx_snmp_agent_md5_key_create.* This
version requires caller to supply password length.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *password* Pointer to password string.
- *password_length* Length of password string.
- *destination_key* Pointer to SNMP key data structure.

### Return Values

- **NX_SUCCESS** (0x00) Successful key create.
- **NX_NOT_ENABLED** (0x14) Security not enabled.
- **NX_SNMP_FAILED** (0x102) SNMP failed.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
NX_SNMP_SECURITY_KEY my_key;

/* Create the MD5 key for "my_agent."   */
status =  nx_snmp_agent_md5_key_create_extended(&my_agent, "authpw", 6, &my_key);


/* If status is NX_SUCCESS the key for the password "authpw" has been created.  */
```

## nx_snmp_agent_priv_trap_key_use
Specify encryption key for trap messages

### Prototype

```c
UINT nx_snmp_agent_priv_trap_key_use(
    NX_SNMP_AGENT *agent_ptr, 
    NX_SNMP_SECURITY_KEY *key);
```

### Description

This service specifies that a previously created privacy key is to be
used for encryption and decryption of SNMPv3 trap messages.

> **Note:** *An authentication key must be previously created. SNMP v3 privacy (encryption) requires authentication. See nx_snmp_agent_auth_trap_key_use for details.*

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *key* Pointer to previously create key.

### Return Values

- **NX_SUCCESS** (0x00) Successful privacy key set.
- **NX_NOT_ENABLED** (0x14) Security not enabled.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Use the "my_privacy_key" for the SNMP Agent "my_agent" trap messages.   */
status =  nx_snmp_agent_priv_trap_key_use(&my_agent, &my_privacy_key);


/* If status is NX_SUCCESS the privacy key is registered with the SNMP agent.  */
```

## nx_snmp_agent_privacy_key_use
Specify encryption key for response messages

### Prototype

```c
UINT nx_snmp_agent_privacy_key_use(
    NX_SNMP_AGENT *agent_ptr, 
    NX_SNMP_SECURITY_KEY *key);
```
### Description

This service specifies that the previously created key is to be used for
encryption and decryption of SNMPv3 response messages.

> **Note:** *An authentication key must have previously been specified. SNMP v3 encryption requires creation of an authentication key as well. See nx_snmp_agent_authentication_key_use for details.*

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *key* Pointer to previously create key.

### Return Values

- **NX_SUCCESS** (0x00) Successful privacy key set.
- **NX_NOT_ENABLED** (0x14) Security not enabled.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Use the "my_privacy_key" for the SNMP Agent "my_agent."   */
status =  nx_snmp_agent_privacy_key_use(&my_agent, &my_privacy_key);


/* If status is NX_SUCCESS the privacy key is registered with the SNMP agent.  */
```

## nx_snmp_agent_sha_key_create
Create SHA key (SNMP v3 only)

### Prototype

```c
UINT nx_snmp_agent_sha_key_create(
    NX_SNMP_AGENT *agent_ptr, 
    UCHAR *password, 
    NX_SNMP_SECURITY_KEY *destination_key);
```
### Description

This service creates a SHA key that can be used for authentication and
encryption.

This service is deprecated. Developers are encouraged to migrate to
*nx_snmp_agent_sha_key_create_extended*.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *password* Pointer to password string.
- *destination_key* Pointer to SNMP key data structure.

### Return Values

- **NX_SUCCESS** (0x00) Successful key create.
- **NX_SNMP_ERROR** (0x100) Key create error.
- **NX_PTR_ERROR** (0x07) Invalid SNMP Agent or key pointer.

### Allowed From

Initialization, Threads

### Example

```c
NX_SNMP_SECURITY_KEY my_key;

/* Create the SHA key for "my_agent."   */
status =  nx_snmp_agent_sha_key_create(&my_agent, "authpw", &my_key);


/* If status is NX_SUCCESS the key for the password "authpw" has been created.  */
```

## nx_snmp_agent_sha_key_create_extended
Create SHA key (SNMP v3 only)

### Prototype

```c
UINT nx_snmp_agent_sha_key_create_extended(
    NX_SNMP_AGENT *agent_ptr, 
    UCHAR *password, 
    UINT password_length,
    NX_SNMP_SECURITY_KEY *destination_key);
```
### Description

This service creates a SHA key that can be used for authentication and encryption.

This service replaces *nx_snmp_agent_sha_key_create.* This version requires caller to supply password length.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *password* Pointer to password string.
- *password_length* Length of password string.
- *destination_key* Pointer to SNMP key data structure.

### Return Values

- **NX_SUCCESS** (0x00) Successful key create.
- **NX_SNMP_ERROR** (0x100) Key create error.
- **NX_SNMP_FAILED** (0x102) Key create failed.
- **NX_PTR_ERROR** (0x07) Invalid SNMP Agent or key pointer.

### Allowed From

Initialization, Threads

### Example

```c
NX_SNMP_SECURITY_KEY my_key;

/* Create the SHA key for "my_agent."   */
status =  nx_snmp_agent_sha_key_create_extended(&my_agent, "authpw", 6, &my_key);


/* If status is NX_SUCCESS the key for the password "authpw" has been created.  */
```

## nx_snmp_agent_start

Start SNMP agent

### Prototype

```c
UINT nx_snmp_agent_start(NX_SNMP_AGENT *agent_ptr);
```

### Description

This service binds the UDP socket to the SNMP port 161 and starts the SNMP Agent thread task.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.

### Return Values

- **NX_SUCCESS** (0x00) Successful start of SNMP Agent.
- **NX_SNMP_ERROR** (0x100) SNMP Agent start error.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Start the previously created SNMP Agent "my_agent."   */
status =  nx_snmp_agent_start(&my_agent);


/* If status is NX_SUCCESS the SNMP Agent "my_agent" has been started.  */
```

## nx_snmp_agent_stop

Stop SNMP agent

### Prototype

```c
UINT nx_snmp_agent_stop(NX_SNMP_AGENT *agent_ptr);
```
### Description

This service stops the SNMP Agent thread task and unbinds the UDP socket to the SNMP port.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.

### Return Values

- **NX_SUCCESS** (0x00) Successful stop of SNMP Agent.
- **NX_PTR_ERROR** (0x07) Invalid SNMP Agent pointer.

### Allowed From

Threads

### Example

```c
/* Stop the previously created and started SNMP Agent "my_agent."   */
status =  nx_snmp_agent_stop(&my_agent);


/* If status is NX_SUCCESS the SNMP Agent "my_agent" has been stopped.  */
```
## nx_snmp_agent_trap_send
Send SNMPv1 trap *(IPv4 only)*

### Prototype

```c
UINT nx_snmp_agent_trap_send(
    NX_SNMP_AGENT *agent_ptr, 
    ULONG ip_address,
    UCHAR *enterprise, 
    UINT trap_type,
    UINT trap_code, 
    ULONG elapsed_time, 
    NX_SNMP_TRAP_OBJECT *object_list_ptr);
```

### Description

This service sends an SNMP trap to the SNMP Manager at the specified IPv4 address. The preferred method for sending an SNMP trap in NetX Duo is to use the *nxd_snmp_agent_trap_send* service. *nx_snmp_agent_trap_send* is included in NetX Duo to support existing NetX SNMP Agent applications.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *ip_address* IPv4 address of the SNMP Manager.
- *enterprise* Enterprise object ID string (sysObjectID).
- *trap_type* Type of trap requested, as follows:  
   - NX_SNMP_TRAP_COLDSTART (0)  
   - NX_SNMP_TRAP_WARMSTART (1)  
   - NX_SNMP_TRAP_LINKDOWN (2)  
   - NX_SNMP_TRAP_LINKUP (3)  
   - NX_SNMP_TRAP_AUTHENTICATE_FAILURE (4)  
   - NX_SNMP_TRAP_EGPNEIGHBORLOSS (5)  
- *trap_code* Specific trap code.
- *elapsed_time* Time system has been up (sysUpTime).
- *object_list_ptr* Array of objects and their associated values to be included in the SNMP trap. The list is NX_NULL terminated.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP trap send.
- **NX_SNMP_ERROR** (0x100) Error sending SNMP trap.
- **NX_NOT_ENABLED** (0x14) SNMPv1 not enabled.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Threads

### Example

```c
NX_SNMP_TRAP_OBJECT trap_list[5];

ULONG dest_ip_address = IP_ADDRESS(1,2,3,4);

/* Build list of objects to supply in the trap.  */
trap_list[0].nx_snmp_object_string_ptr =  "1.3.6.1.2.1.2.2.1.1.0";
trap_list[0].nx_snmp_object_data.nx_snmp_object_data_type =  NX_SNMP_INTEGER;
trap_list[0].nx_snmp_object_data.nx_snmp_object_data_msw =   counter;
trap_list[1].nx_snmp_object_string_ptr =  "1.3.6.1.2.1.1.3.0";
trap_list[1].nx_snmp_object_data.nx_snmp_object_data_type =  NX_SNMP_TIME_TICS;
trap_list[1].nx_snmp_object_data.nx_snmp_object_data_msw =   tx_time_get();
trap_list[2].nx_snmp_object_string_ptr =  NX_NULL; /* Terminate list!  */

/* Send trap to SNMP manager at 193.2.2.61.  */
status =  nx_snmp_agent_trap_send(&my_agent,dest_ip_address,
 		                           "1.3.6.7.7.7", NX_SNMP_TRAP_LINKUP, counter++, 
                                  tx_time_get(), trap_list);

/* If status is NX_SUCCESS the SNMP trap has been sent.  */
```
## nxd_snmp_agent_trap_send
Send SNMPv1 trap *(IPv4 and IPv6)*

### Prototype

```c
UINT nxd_snmp_agent_trap_send(
    NX_SNMP_AGENT *agent_ptr, 
    ULONG ip_address,
    UCHAR *enterprise, UINT trap_type, 
    UINT trap_code,
    ULONG elapsed_time, 
    NX_SNMP_TRAP_OBJECT *object_list_ptr);
```
### Description

This service sends an SNMP trap to the SNMP Manager at the specified IP address. The equivalent method for sending an SNMP trap in NetX is the *nxd_snmp_agent_trap_send* service.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *ip_address* IPv4 or IPv6 address of the SNMP Manager.
- *enterprise* Enterprise object ID string (sysObjectID).
- *trap_type* Type of trap requested, as follows:  
   - NX_SNMP_TRAP_COLDSTART (0)
   - NX_SNMP_TRAP_WARMSTART (1)
   - NX_SNMP_TRAP_LINKDOWN (2)
   - NX_SNMP_TRAP_LINKUP (3)
   - NX_SNMP_TRAP_AUTHENTICATE_FAILURE (4)
   - NX_SNMP_TRAP_EGPNEIGHBORLOSS (5)
- *trap_code* Specific trap code.
- *elapsed_time* Time system has been up (sysUpTime).
- *object_list_ptr* Array of objects and their associated values to be included in the SNMP trap. The list is NX_NULL terminated.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP trap send.
- **NX_SNMP_ERROR** (0x100) Error sending SNMP trap.
- **NX_NOT_ENABLED** (0x14) SNMPv1 not enabled.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Threads

### Example

```c
NX_SNMP_TRAP_OBJECT trap_list[5];

NXD_ADDRESS dest_ip_address;

    dest_ip_address.nxd_ip_version = NX_IP_VERSION_V6 ;
    dest_ip_address.nxd_ip_address.v6[0] = 0x20010db8;
    dest_ip_address.nxd_ip_address.v6[1] = 0xf101;
    dest_ip_address.nxd_ip_address.v6[2] = 0x00000000;
    dest_ip_address.nxd_ip_address.v6[3] = 0x00000101;

/* Build list of objects to supply in the trap.  */
trap_list[0].nx_snmp_object_string_ptr =  "1.3.6.1.2.1.2.2.1.1.0";
trap_list[0].nx_snmp_object_data.nx_snmp_object_data_type =  NX_SNMP_INTEGER;
trap_list[0].nx_snmp_object_data.nx_snmp_object_data_msw =   counter;
trap_list[1].nx_snmp_object_string_ptr =  "1.3.6.1.2.1.1.3.0";
trap_list[1].nx_snmp_object_data.nx_snmp_object_data_type =  NX_SNMP_TIME_TICS;
trap_list[1].nx_snmp_object_data.nx_snmp_object_data_msw =   tx_time_get();
trap_list[2].nx_snmp_object_string_ptr =  NX_NULL; /* Terminate list!  */

/* Send trap to SNMP manager at 193.2.2.61.  */
status =  nxd_snmp_agent_trap_send(&my_agent,&dest_ip_address,
 		         "1.3.6.7.7.7", NX_SNMP_TRAP_LINKUP, counter++, tx_time_get(), 
               trap_list);

/* If status is NX_SUCCESS the SNMP trap has been sent.  */
```

## nx_snmp_agent_trapv2_send
Send SNMPv2 trap (IPv4 only)

### Prototype

```c
UINT nx_snmp_agent_trapv2_send(
    NX_SNMP_AGENT *agent_ptr, 
    NXD_ADDRESS *ip_address,
    UCHAR *community,
    UINT trap_type, 
    ULONG elapsed_time,
    NX_SNMP_TRAP_OBJECT *object_list_ptr);
```
### Description

This service sends an SNMPv2 trap to the SNMP Manager at the specified IPv4 address. The preferred method for sending an SNMP trap in NetX Duo is to use the *nxd_snmp_agent_trapv2_send* service. *nx_snmp_agent_trapv2_send* is included in NetX Duo to support existing NetX SNMP Agent applications.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *ip_address* IPv4 address of the SNMP Manager.
- *community* Community name (username).
- *trap_type*  
   Type of trap requested. The standard events are:  
   - NX_SNMP_TRAP_COLDSTART (0)
   - NX_SNMP_TRAP_WARMSTART (1)
   - NX_SNMP_TRAP_LINKDOWN (2)
   - NX_SNMP_TRAP_LINKUP (3)
   - NX_SNMP_TRAP_AUTHENTICATE_FAILURE (4)
   - NX_SNMP_TRAP_EGPNEIGHBORLOSS (5)
   For proprietary data:  
   - NX_SNMP_TRAP_CUSTOM (0xFFFFFFFF) (defined in *nxd_snmp.h*)
- *elapsed_time* Time system has been up (sysUpTime).
- *object_list_ptr* Array of objects and their associated values to be included in the SNMP trap. The list is NX_NULL terminated.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP trap send.
- **NX_SNMP_ERROR** (0x100) Error sending SNMP trap.
- **NX_NOT_ENABLED** (0x14) SNMPv2 not enabled.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Threads

### Example

```c
NX_SNMP_TRAP_OBJECT trap_list[5];
ULONG  dest_ip_address = IP_ADDRESS(1,2,3,4);

/* Build an empty object list, since it is not needed for the 
   NX_SNMP_TRAP_COLDSTART trap.  */
trap_list[0].nx_snmp_object_string_ptr =  NX_NULL;

/* Send trap to SNMP manager at 193.2.2.61.  */
Status =  nx_snmp_agent_trapv2_send(&my_agent,dest_ip_address, "public",
               NX_SNMP_TRAP_COLDSTART, tx_time_get(), trap_list);

/* If status is NX_SUCCESS the SNMP trap has been sent.  */
```

## nx_snmp_agent_trapv2_oid_send
Send SNMPv2 trap specifying OID directly 

### Prototype

```c
UINT nx_snmp_agent_trapv2_oid_send(
    NX_SNMP_AGENT *agent_ptr, 
    ULONG ip_address,
    UCHAR *community,        
    UCHAR *oid,
    ULONG elapsed_time,   
    NX_SNMP_TRAP_OBJECT *object_list_ptr);
```
### Description

This service sends an SNMPv2 trap to the SNMP Manager at the specified IP address (IPv4 only) and allows the caller to specify the OID directly. The preferred method for sending an SNMP trap with specified OID in NetX Duo is to use the *nxd_snmp_agent_trapv2_oid_send* service. *nx_snmp_agent_trapv2_oid_ send* is included in NetX Duo to support existing NetX SNMP Agent applications.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *ip_address* IP address of SNMP Manager.
- *community* Community name (username).
- *oid* Pointer to buffer containing OID.
- *elapsed_time* Time system has been up (sysUpTime).
- *object_list_ptr* Array of objects and their associated values to be included in the SNMP trap. The list is NX_NULL terminated.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP trap send.
- **NX_SNMP_ERROR** (0x100) Error sending SNMP trap.
- **NX_PTR_ERROR** (0x16) Invalid SNMP Agent or parameter pointer.
- **NX_IP_ADDRESS_ERROR** (0x21) Invalid destination IP address.
- **NX_OPTION_ERROR** (0x0a) Invalid parameter.

### Allowed From

Threads

### Example

```c
NX_SNMP_TRAP_OBJECT trap_list[5];

/* Build an empty object list */
trap_list[0].nx_snmp_object_string_ptr =  NX_NULL;

/* Send trap to SNMP manager at 193.2.2.61.  */
Status =  nx_snmp_agent_trapv2_oid_send(&my_agent, IP_ADDRESS(193,2,2,61), 
                                       "public", (UCHAR *)"0.9.9.9.9.9.9.9.9.9",  
                                       (tx_time_get()), trap_list);

/* If status is NX_SUCCESS the SNMP trap has been sent.  */
```

## nxd_snmp_agent_trapv2_send
Send SNMPv2 trap (IPv4 and IPv6)

### Prototype

```c
UINT nxd_snmp_agent_trapv2_send(
    NX_SNMP_AGENT *agent_ptr, 
    NXD_ADDRESS *ip_address, 
    UCHAR *community, UINT trap_type, 
    ULONG elapsed_time, 
    NX_SNMP_TRAP_OBJECT *object_list_ptr);
```

### Description

This service sends an SNMP V2 trap to the SNMP Manager at the specified IP address.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *ip_address* IP (IPv4 or IPv6) address of the SNMP Manager.
- *community* Community name (username).
- *trap_type*  
   Type of trap requested. The standard events are:  
   - NX_SNMP_TRAP_COLDSTART (0)
   - NX_SNMP_TRAP_WARMSTART (1)
   - NX_SNMP_TRAP_LINKDOWN (2)
   - NX_SNMP_TRAP_LINKUP (3)
   - NX_SNMP_TRAP_AUTHENTICATE_FAILURE (4)
   - NX_SNMP_TRAP_EGPNEIGHBORLOSS (5)
   For proprietary data:
   - NX_SNMP_TRAP_CUSTOM (0xFFFFFFFF) (defined in *nxd_snmp.h*)
- *elapsed_time* Time system has been up (sysUpTime).
- *object_list_ptr* Array of objects and their associated values to be included in the SNMP trap. The list is NX_NULL terminated.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP trap send.
- **NX_SNMP_ERROR** (0x100) Error sending SNMP trap.
- **NX_NOT_ENABLED** (0x14) SNMPv2 not enabled.
- **NX_SNMP_INVALID_IP_PROTOCOL_ERROR** (0x104) Unsupported IP version
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Threads

### Example

```c
NX_SNMP_TRAP_OBJECT trap_list[5];
NXD_ADDRESS dest_ip_address;

    dest_ip_address.nxd_ip_version = NX_IP_VERSION_V6 ;
    dest_ip_address.nxd_ip_address.v6[0] = 0x20010db8;
    dest_ip_address.nxd_ip_address.v6[1] = 0xf101;
    dest_ip_address.nxd_ip_address.v6[2] = 0x00000000;
    dest_ip_address.nxd_ip_address.v6[3] = 0x00000101;

/* Build an empty object list, since it is not needed for the 
   NX_SNMP_TRAP_COLDSTART trap.  */
trap_list[0].nx_snmp_object_string_ptr =  NX_NULL;

/* Send a standard event trap to SNMP manager at 193.2.2.61.  */
Status =  nxd_snmp_agent_trapv2_send(&my_agent,&dest_ip_address, "public",
                                    NX_SNMP_TRAP_COLDSTART, tx_time_get(),  
                                    trap_list);

/* If status is NX_SUCCESS the SNMP trap has been sent.  */
```

## nxd_snmp_agent_trapv2_oid_send
Send SNMPv2 trap specifying OID directly 

### Prototype

```c
UINT nxd_snmp_agent_trapv2_oid_send(
    NX_SNMP_AGENT *agent_ptr, 
    NXD_ADDRESS *ip_address, 
    UCHAR *community,        
    UCHAR *oid, ULONG elapsed_time,   
    NX_SNMP_TRAP_OBJECT *object_list_ptr);
```
### Description

This service sends an SNMP v2 trap to the SNMP Manager at the specified IP address (IPv4/IPv6) and allows the caller to specify the OID directly.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *ip_address* IP address of SNMP Manager (IPv4/IPv6).
- *community* Community name (username).
- *oid* Pointer to buffer containing OID.
- *elapsed_time* Time system has been up (sysUpTime).
- *object_list_ptr* Array of objects and their associated values to be included in the SNMP trap. The list is NX_NULL terminated.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP trap send.
- **NX_SNMP_ERROR** (0x100) Error sending SNMP trap.
- **NX_PTR_ERROR** (0x16) Invalid SNMP Agent or parameter pointer.
- **NX_IP_ADDRESS_ERROR** (0x21) Invalid destination IP address.
- **NX_OPTION_ERROR** (0x0a) Invalid parameter.

### Allowed From

Threads

### Example

```c
NX_SNMP_TRAP_OBJECT trap_list[5];
NXD_ADDRESS         address;


/* Build an empty object list */
trap_list[0].nx_snmp_object_string_ptr =  NX_NULL;

address.nxd_ip_version = NX_IP_VERSION_V4;
address.nxd_ip_address.v4 = IP_ADDRESS(193,2,2,61)

/* Send trap to SNMP manager at 193.2.2.61.  */
Status =  nxd_snmp_agent_trapv2_oid_send(&my_agent, &address, 
                                       "public", (UCHAR *)"0.9.9.9.9.9.9.9.9.9",  
                                       tx_time_get(), trap_list);

/* If status is NX_SUCCESS the SNMP trap has been sent.  */
```
## nx_snmp_agent_trapv3_send
Send SNMPv3 trap (IPv4 only)

### Prototype

```c
UINT nx_snmp_agent_trapv3_send(
    NX_SNMP_AGENT *agent_ptr, 
    ULONG ip_address,
    UCHAR *username,
    UINT trap_type, 
    ULONG elapsed_time,
    NX_SNMP_TRAP_OBJECT *object_list_ptr);
```

### Description

This service sends an SNMPv3 trap to the SNMP Manager at the specified IPv4 address. The preferred method for sending an SNMP trap in NetX Duo is to use the *nxd_snmp_agent_trapv3_send* service. *nx_snmp_agent_trapv3_send* is included in NetX Duo to support existing NetX SNMP Agent applications.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *ip_address* IPv4 address of the SNMP Manager.
- *username* Community name (username).
- *trap_type*  
   Type of trap requested. The standard events are:  
   - NX_SNMP_TRAP_COLDSTART (0)
   - NX_SNMP_TRAP_WARMSTART (1)
   - NX_SNMP_TRAP_LINKDOWN (2)
   - NX_SNMP_TRAP_LINKUP (3)
   - NX_SNMP_TRAP_AUTHENTICATE_FAILURE (4)
   - NX_SNMP_TRAP_EGPNEIGHBORLOSS (5)
   For proprietary data:
   - NX_SNMP_TRAP_CUSTOM (0xFFFFFFFF) (defined in *nxd_snmp.h*)
- *elapsed_time* Time system has been up (sysUpTime).
- *object_list_ptr* Array of objects and their associated values to be included in the SNMP trap. The list is NX_NULL terminated.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP trap send.
- **NX_SNMP_ERROR** (0x100) Error sending SNMP trap.
- **NX_NOT_ENABLED** (0x14) SNMPv3 not enabled.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
NX_SNMP_TRAP_OBJECT trap_list[5];
ULONG dest_ip_address = IP_ADDRESS(1,2,3,4);

/* Build an empty object list, since it is not needed for the 
   NX_SNMP_TRAP_COLDSTART trap.  */
trap_list[0].nx_snmp_object_string_ptr =  NX_NULL;

/* Send trap to SNMP manager at 193.2.2.61.  */
Status =  nx_snmp_agent_trapv3_send(&my_agent, dest_ip_address, "public",
               NX_SNMP_TRAP_COLDSTART, tx_time_get(), trap_list);

/* If status is NX_SUCCESS the SNMP trap has been sent.  */
```

## nx_snmp_agent_trapv3_oid_send
Send SNMPv3 trap specifying OID directly 

### Prototype

```c
UINT nx_snmp_agent_trapv3_oid_send(
    NX_SNMP_AGENT *agent_ptr, 
    ULONG ip_address,
    UCHAR *username,        
    UCHAR *oid,
    ULONG elapsed_time,   
    NX_SNMP_TRAP_OBJECT *object_list_ptr);
```
### Description

This service sends an SNMPv3 trap to the SNMP Manager at the specified IP address (IPv4 only) and allows the caller to specify the OID directly. The preferred method for sending an SNMP trap with specified OID in NetX Duo is to use the *nxd_snmp_agent_trapv3_oid_send* service. *nx_snmp_agent_trapv3_oid_ send* is included in NetX Duo to support existing NetX SNMP Agent applications.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *ip_address* IP address of SNMP Manager.
- *username* Community name (username).
- *oid* Pointer to buffer containing OID.
- *elapsed_time* Time system has been up (sysUpTime).
- *object_list_ptr* Array of objects and their associated values to be included in the SNMP trap. The list is NX_NULL terminated.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP trap send.
- **NX_SNMP_ERROR** (0x100) Error sending SNMP trap.
- **NX_PTR_ERROR** (0x16) Invalid SNMP Agent or parameter pointer.
- **NX_IP_ADDRESS_ERROR** (0x21) Invalid destination IP address.
- **NX_OPTION_ERROR** (0x0a) Invalid parameter.

### Allowed From

Threads

### Example

```c
NX_SNMP_TRAP_OBJECT trap_list[5];

/* Build an empty object list */
trap_list[0].nx_snmp_object_string_ptr =  NX_NULL;

/* Send trap to SNMP manager at 193.2.2.61.  */
Status =  nx_snmp_agent_trapv3_oid_send(&my_agent, IP_ADDRESS(193,2,2,61), 
                                       "public", (UCHAR *)"0.9.9.9.9.9.9.9.9.9",  
                                       (tx_time_get()), trap_list);

/* If status is NX_SUCCESS the SNMP trap has been sent.  */
```
## nxd_snmp_agent_trapv3_send
Send SNMPv3 trap (IPv4 and IPv6)

### Prototype

```c
UINT nxd_snmp_agent_trapv3_send(
    NX_SNMP_AGENT *agent_ptr, 
    NXD_ADDRESS *ip_address, 
    UCHAR *username, UINT trap_type, 
    ULONG elapsed_time, 
    NX_SNMP_TRAP_OBJECT *object_list_ptr);
```
### Description

This service sends an SNMP trap to the SNMP Manager at the specified IP address. This trap is basically the same as the SNMP v2 trap, except the trap message format is contained in the SNMP v3 PDU.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *ip_address* IP (IPv4 or IPv6) address of the SNMP Manager.
- *username* Community name (username).
- *trap_type*  
   Type of trap requested. The standard events are:
   - NX_SNMP_TRAP_COLDSTART (0)
   - NX_SNMP_TRAP_WARMSTART (1)
   - NX_SNMP_TRAP_LINKDOWN (2)
   - NX_SNMP_TRAP_LINKUP (3)
   - NX_SNMP_TRAP_AUTHENTICATE_FAILURE (4)
   - NX_SNMP_TRAP_EGPNEIGHBORLOSS (5)  
   For proprietary data:
   - NX_SNMP_TRAP_CUSTOM (0xFFFFFFFF) (defined in *nxd_snmp.h*)
- *elapsed_time* Time system has been up (sysUpTime).
- *object_list_ptr* Array of objects and their associated values to be included in the SNMP trap. The list is NX_NULL terminated.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP trap send.
- **NX_SNMP_ERROR** (0x100) Error sending SNMP trap.
- **NX_NOT_ENABLED** (0x14) SNMPv3 not enabled.
- **NX_SNMP_INVALID_IP_PROTOCOL_ERROR** (0x104) Unsupported IP version
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Threads

### Example

```c
NX_SNMP_TRAP_OBJECT trap_list[5];
NXD_ADDRESS dest_ip_address;

    dest_ip_address.nxd_ip_version = NX_IP_VERSION_V6 ;
    dest_ip_address.nxd_ip_address.v6[0] = 0x20010db8;
    dest_ip_address.nxd_ip_address.v6[1] = 0xf101;
    dest_ip_address.nxd_ip_address.v6[2] = 0x00000000;
    dest_ip_address.nxd_ip_address.v6[3] = 0x00000101;

/* Build an empty object list, since it is not needed for the 
   NX_SNMP_TRAP_COLDSTART trap.  */
trap_list[0].nx_snmp_object_string_ptr =  NX_NULL;

/* Send trap to SNMP manager at 193.2.2.61.  */
Status =  nxd_snmp_agent_trapv3_send(&my_agent, &dest_ip_address, "public",
                                    NX_SNMP_TRAP_COLDSTART, tx_time_get(),  
                                    trap_list);

/* If status is NX_SUCCESS the SNMP trap has been sent.  */
```

## nxd_snmp_agent_trapv3_oid_send
Send SNMPv3 trap specifying OID directly 

### Prototype

```c
UINT nxd_snmp_agent_trapv3_oid_send(
    NX_SNMP_AGENT *agent_ptr, 
    NXD_ADDRESS *ip_address, 
    UCHAR *username,        
    UCHAR *oid, ULONG elapsed_time,   
    NX_SNMP_TRAP_OBJECT *object_list_ptr);
```
### Description

This service sends an SNMPv3 trap to the SNMP Manager at the specified IP address (IPv4/IPv6) and allows the caller to specify the OID directly.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block.
- *ip_address* Pointer to IP address of SNMP Manager.
- *username* Username (community name).
- *oid* Pointer to buffer containing OID.
- *elapsed_time* Time system has been up (sysUpTime).
- *object_list_ptr* Array of objects and their associated values to be included in the SNMP trap. The list is NX_NULL terminated.

### Return Values

- **NX_SUCCESS** (0x00) Successful SNMP trap send.
- **NX_SNMP_ERROR** (0x100) Error sending SNMP trap.
- **NX_PTR_ERROR** (0x16) Invalid SNMP Agent or parameter pointer.
- **NX_IP_ADDRESS_ERROR** (0x21) Invalid destination IP address.

### Allowed From

Threads

### Example

```c
NXD_ADDRESS         ip_address;
NX_SNMP_TRAP_OBJECT trap_list[5];

/* Build an empty object list */
trap_list[0].nx_snmp_object_string_ptr =  NX_NULL;

ip_address.nxd_ip_version = NX_IP_VERSION_V4;
ip_address.nxd_ip_address.v4 = IP_ADDRESS(193,2,2,61);

/* Send trap to SNMP manager at 193.2.2.61.  */
Status =  nxd_snmp_agent_trapv3_oid_send(&my_agent, &ip_address, 
                                       "public", (UCHAR *)"0.9.9.9.9.9.9.9.9.9",  
                                       tx_time_get(), trap_list);

/* If status is NX_SUCCESS the SNMP trap has been sent.  */
```
## nx_snmp_agent_v3_context_boots_set
Set the number of reboots (if SNMPv3 enabled)

### Prototype

```c
UINT nx_snmp_agent_v3_context_boots_set(
    NX_SNMP_AGENT *agent_ptr, 
    UINT boots);
```

### Description

This service sets the number of reboots recorded by the SNMP agent. This service is only available if SNMPv3 is enabled for the SNMP agent because boot count is only used in the SNMPv3 protocol.

### Input Parameters

- *agent_ptr* Pointer to SNMP Agent control block
- *boots* The value to set SNMP Agent boot count to

### Return Values

- **NX_SUCCESS** (0x00) Successfully set boot count
- **NX_SNMP_ERROR** (0x100) Error setting boot count
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
UINT my_boots = 4;

if (my_agent.nx_snmp_agent_v3_enabled == NX_TRUE)
{
   status = nx_snmp_agent_v3_context_boots_set(&my_agent, my_boots);
}


/* If status is NX_SUCCESS the SNMP boot count is set.  */
```

## nx_snmp_object_compare

Compare two objects

### Prototype

```c
UINT nx_snmp_object_compare(
    UCHAR *object,
    UCHAR *reference_object);
```

### Description

This service compares the supplied object ID with the reference object ID. Both object IDs are in the ASCII SMI notation, e.g., both object must start with the ASCII string "1.3.6".

This service is deprecated. Developers are encouraged to migrate to *nx_snmp_object_compare_extended.*

### Input Parameters

- *object* Pointer to object ID.
- *reference_object* Pointer to the reference object ID.

### Return Values

- **NX_SUCCESS** (0x00) The object matches the reference object.
- **NX_SNMP_NEXT_ENTRY** (0x101) The object is less than the reference object.
- **NX_SNMP_ERROR** (0x100) The object is greater than the reference object.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Compare "requested_object" with the sysDescr object ID of 
   "1.3.6.1.2.1.1.1.0".  */
Status =  nx_snmp_object_compare(requested_object, "1.3.6.1.2.1.1.1.0");

/* If status is NX_SUCCESS, requested_object is the sysDescr object. 
   Otherwise, if status is NX_SNMP_NEXT_ENTRY, the requested object is
   less than the sysDescr. If status is NX_SNMP_ERROR, the object is 
   greater than sysDescr. */
```

## nx_snmp_object_compare_extended
Compare two objects 

### Prototype

```c
UINT nx_snmp_object_compare_extended(
    UCHAR *request_object, 
    UINT requested_object_length,
    UCHAR *reference_object
    UINT reference_object_length);
```
### Description

This service compares the supplied object ID with the reference object
ID. Both object IDs are in the ASCII SMI notation, e.g., both object
must start with the ASCII string "1.3.6".

This service replaces *nx_snmp_object_compare.* This version
requires callers to supply length information.

### Input Parameters

- *request_object* Pointer to request object ID.
- *request_object_length* Length of the request object ID.
- *reference_object* Pointer to the reference object ID.
- *reference_object_length* Length of the reference object ID.

### Return Values

- **NX_SUCCESS** (0x00) The object matches the reference object.
- **NX_SNMP_NEXT_ENTRY** (0x101) The object is less than the reference object.
- **NX_SNMP_ERROR** (0x100) The object is greater than the reference object.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Compare "requested_object" with the sysDescr object ID of 
   "1.3.6.1.2.1.1.1.0".  */
Status =  nx_snmp_object_compare_extended(requested_object, 17,
                                          "1.3.6.1.2.1.1.1.0", 17);

/* If status is NX_SUCCESS, requested_object is the sysDescr object. 
   Otherwise, if status is NX_SNMP_NEXT_ENTRY, the requested object is
   less than the sysDescr. If status is NX_SNMP_ERROR, the object is 
   greater than sysDescr. */
```

## nx_snmp_object_copy
Copy an object 

### Prototype

```c
UINT  nx_snmp_object_copy(
    UCHAR *source_object_name, 
    UCHAR *destination_object_name);
```
### Description

This service copies the source object in ASCII SIM notation to the
destination object.

This service is deprecated. Developers are encouraged to migrate to
*nx_snmp_object_copy_extended.*

### Input Parameters

- *source_object_name* Pointer to source object ID.
- *destination_object_name* Pointer to destination object ID.

### Return Values

- **size** Number of bytes copied to destination name. If error, zero is returned.

### Allowed From

Initialization, Threads

### Example

```c
/* Copy "my_object" to "my_new_object".  */
size =  nx_snmp_object_copy(my_object, my_new_object);

/* Size contains the number of bytes copied. */
```

## nx_snmp_object_copy_extended
Copy an object 

### Prototype

```c
UINT  nx_snmp_object_copy_extended(
    UCHAR *source_object_name, 
    UINT source_object_name_length,
    UCHAR *destination_object_name_buffer,
    UINT destination_object_name_buffer_size);
```

### Description

This service copies the source object in ASCII SIM notation to the
destination object.

This service replaces *nx_snmp_object_copy.* This version
requires callers to supply length information.

### Input Parameters

- *source_object_name* Pointer to source object ID.
- *source_object_name_length* Length of source object ID.
- *destination_object_name_buffer* Pointer to destination object buffer.
- *destination_object_name_buffer_size* Size of destination object buffer.

### Return Values

- **size** Number of bytes copied to destination name. If error, zero is returned.

### Allowed From

Initialization, Threads

### Example

```c
UCHAR	my_object = "1.3.6.1.2.1.1.1.0";
UCHAR	my_new_object[32];


/* Copy "my_object" to "my_new_object".  */
size =  nx_snmp_object_copy(my_object, 17, my_new_object, sizeof(my_new_object));

/* Size contains the number of bytes copied. */
```

## nx_snmp_object_counter_get

Get counter object

### Prototype

```c
UINT  nx_snmp_object_counter_get(
    VOID *source_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```
### Description

This service retrieves the counter object at the address specified by
the source pointer and places it in the NetX Duo object data structure. This
routine is typically called from the GET or GETNEXT application callback
routine.

### Input Parameters

- *source_ptr* Pointer to counter source.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The counter object has be successfully retrieved.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Get the ifInOctets (1.3.6.1.2.1.2.2.1.10.0) MIB-2 object.  */
status =  nx_snmp_object_counter_get(&ifInOctets, my_object);

/* If status is NX_SUCCESS, the ifInOctets object has been 
   retrieved and is ready to be returned. */
```

## nx_snmp_object_counter_set
Set counter object 

### Prototype

```c
UINT  nx_snmp_object_counter_set(
    VOID *destination_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service sets the counter at the address specified by the
destination pointer with the counter value in the NetX Duo object data
structure. This routine is typically called from the SET application
callback routine.

### Input Parameters

- *destination_ptr* Pointer to counter destination.
- *object_data* Pointer to counter source object structure.

### Return Values

- **NX_SUCCESS** (0x00) The counter object has be successfully set.
- **NX_SNMP_ERROR_WRONGTYPE** (0x07) Invalid object type.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Set the ifInOctets (1.3.6.1.2.1.2.2.1.10.0) MIB-2 object with
   the counter object value contained in my_object.  */
status =  nx_snmp_object_counter_set(&ifInOctets, my_object);

/* If status is NX_SUCCESS, the ifInOctets object has been 
   set. */
```

## nx_snmp_object_counter64_get
Get 64-bit counter object 

### Prototype

```c
UINT  nx_snmp_object_counter64_get(
    VOID *source_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service retrieves the 64-bit counter object at the address
specified by the source pointer and places it in the NetX Duo object data
structure. This routine is typically called from the GET or GETNEXT
application callback routine.

### Input Parameters

- *source_ptr* Pointer to counter source.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The counter object has be successfully retrieved.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Get the value of my_64_bit_counter and place it into my_object
   for return.  */
status =  nx_snmp_object_counter64_get(&my_64_bit_counter, my_object);

/* If status is NX_SUCCESS, the my_64_bit_counter object has been 
   retrieved and is ready to be returned. */
```

## nx_snmp_object_counter64_set
Set 64-bit counter object 

### Prototype

```c
UINT  nx_snmp_object_counter64_set(
    VOID *destination_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```
### Description

This service sets the 64-bit counter at the address specified by the
destination pointer with the counter value in the NetX Duo object data
structure. This routine is typically called from the SET application
callback routine.

### Input Parameters

- *destination_ptr* Pointer to counter destination.
- *object_data* Pointer to counter source object structure.

### Return Values

- **NX_SUCCESS** (0x00) The counter object has be successfully set.
- **NX_SNMP_ERROR_WRONGTYPE** (0x07) Invalid object type.
- **NX_PTR_ERROR** (0x07) Invalid input pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Set the value of my_64_bit_counter with the value in my_object.  */
status =  nx_snmp_object_counter64_set(&my_64_bit_counter, my_object);

/* If status is NX_SUCCESS, the my_64_bit_counter object has been 
   set. */
```

## nx_snmp_object_end_of_mib
Set end-of-mib value 

### Prototype

```c
UINT  nx_snmp_object_end_of_mib(
    VOID *not_used_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service creates an object signaling the end of the MIB and is
typically called from the GET or GETNEXT application callback routine.

### Input Parameters

- *not_used_ptr* Pointer not used – should be NX_NULL.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The end-of-mib object has be successfully built.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Place an end-of-mib value in my_object.  */
status =  nx_snmp_object_end_of_mib(NX_NULL, my_object);

/* If status is NX_SUCCESS, the my_object is now an end-of-mib object. */
```

## nx_snmp_object_gauge_get
Get gauge object 

### Prototype

```c
UINT  nx_snmp_object_gauge_get(
    VOID *source_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service retrieves the gauge object at the address specified by the
source pointer and places it in the NetX Duo object data structure. This
routine is typically called from the GET or GETNEXT application callback
routine.

### Input Parameters

- *source_ptr* Pointer to gauge source.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The gauge object has be successfully retrieved.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Get the value of ifSpeed (1.3.6.1.2.1.2.2.1.5.0) and place it in my_object
   for return.  */
status =  nx_snmp_object_gauge_get(&ifSpeed, my_object);

/* If status is NX_SUCCESS, the my_object now contains the ifSpeed gauge value. */
```

## nx_snmp_object_gauge_set
Set gauge object 

### Prototype

```c
UINT  nx_snmp_object_gauge_set(
    VOID *destination_ptr,
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service sets the gauge at the address specified by the destination
pointer with the gauge value in the NetX Duo object data structure. This
routine is typically called from the SET application callback routine.

### Input Parameters

- *destination_ptr* Pointer to gauge destination.
- *object_data* Pointer to gauge source object structure.

### Return Values

- **NX_SUCCESS** (0x00) The gauge object has be successfully set.
- **NX_SNMP_ERROR_WRONGTYPE** (0x07) Invalid object type.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Set the value of "my_gauge" from the gauge value in my_object.  */
status =  nx_snmp_object_gauge_set(&my_gauge, my_object);

/* If status is NX_SUCCESS, the my_gauge now contains the new gauge value. */
```

## nx_snmp_object_id_get
Get object id 

### Prototype

```c
UINT nx_snmp_object_id_get(
    VOID *source_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service retrieves the object ID (in ASCII SIM notation) at the
address specified by the source pointer and places it in the NetX Duo object
data structure. This routine is typically called from the GET or GETNEXT
application callback routine.

### Input Parameters

- *source_ptr* Pointer to object ID source.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The object ID has be successfully retrieved.
- **NX_SNMP_ERROR** (0x100) Invalid length of object
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Get the value of sysObjectID(1.3.6.1.2.1.1.2.0) and place it in my_object
   for return.  */
status =  nx_snmp_object_id_get(&sysObjectID, my_object);

/* If status is NX_SUCCESS, the my_object now contains the sysObjectID value. */
```

## nx_snmp_object_id_set

Set object id

### Prototype

```c
UINT  nx_snmp_object_id_set(
    VOID *destination_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service sets the object ID (in ASCII SIM notation) at the address
specified by the destination pointer with the object ID in the NetX Duo
object data structure. This routine is typically called from the SET
application callback routine.

### Input Parameters

- *destination_ptr* Pointer to object ID destination.
- *object_data* Pointer to object structure.

### Return Values

- **NX_SUCCESS** (0x00) The object ID has been successfully set.
- **NX_SNMP_ERROR_WRONGTYPE** (0x07) Invalid object type.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Set the string "my_object_id" with the object ID value contained 
   in my_object.  */
status =  nx_snmp_object_id_set(my_object_id, my_object);

/* If status is NX_SUCCESS, the my_object_id now contains the object ID value. */
```

## nx_snmp_object_integer_get
Get integer object 

### Prototype

```c
UINT  nx_snmp_object_integer_get(
    VOID *source_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service retrieves the integer object at the address specified by
the source pointer and places it in the NetX Duo object data structure. This
routine is typically called from the GET or GETNEXT application callback
routine.

### Input Parameters

- *source_ptr* Pointer to integer source.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The integer object has been successfully retrieved.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Get the value of sysServices (1.3.6.1.2.1.1.7.0) and place it in my_object
   for return.  */
status =  nx_snmp_object_integer_get(&sysServices, my_object);

/* If status is NX_SUCCESS, the my_object now contains the sysServices value. */
```

## nx_snmp_object_integer_set
Set integer object 

### Prototype

```c
UINT  nx_snmp_object_integer_set(
    VOID *destination_ptr,
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service sets the integer at the address specified by the
destination pointer with the integer value in the NetX Duo object data
structure. This routine is typically called from the SET application
callback routine.

### Input Parameters

- *destination_ptr* Pointer to integer destination.
- *object_data* Pointer to integer source object structure.

### Return Values

- **NX_SUCCESS** (0x00) The integer object has been successfully set.
- **NX_SNMP_ERROR_WRONGTYPE** (0x07) Invalid object type.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Set the value of ifAdminStatus from the integer value in my_object.  */
status =  nx_snmp_object_integer_set(&ifAdminStatus, my_object);

/* If status is NX_SUCCESS, ifAdminStatus now contains the new integer value. */
```

## nx_snmp_object_ip_address_get
Get IP address object (IPv4 only)

### Prototype

```c
UINT  nx_snmp_object_ip_address_get(
    VOID *source_ptr,
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service retrieves the IP address object at the address specified by
the source pointer and places it in the NetX Duo object data structure. This
routine is typically called from the GET or GETNEXT application callback
routine.

### Input Parameters

- *source_ptr* Pointer to IPv4 address source.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The IP address object has been successfully retrieved.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Get the value of ipAdEntAddr (1.3.6.1.2.1.4.20.1.1.0) and place it in my_object
   for return.  */
status =  nx_snmp_object_ip_address_get(&ipAdEntAddr, my_object);

/* If status is NX_SUCCESS, the my_object now contains the ipAdEntAddr value. */
```

## nx_snmp_object_ipv6_address_get
Get IP address object (IPv6 only)

### Prototype

```c
UINT  nx_snmp_object_ipv6_address_get(
    VOID *source_ptr,
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service retrieves the IPv6 address object at the address specified
by the source pointer and places it in the NetX Duo object data structure.
This routine is typically called from the GET or GETNEXT application
callback routine.

### Input Parameters

- *source_ptr* Pointer to IPv6 address source.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The IP address object has been successfully retrieved.
- **NX_SNMP_ERROR_WRONGTYPE** (0x07) Incorrect input SNMP object code
- **NX_NOT_ENABLED** (0x14) IPv6 not enabled
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Get the value of ipAdEntAddr (1.3.6.1.2.1.4.20.1.1.0) and place it in my_object
   for return.  */
status =  nx_snmp_object_ipv6_address_get(&ipAdEntAddr, my_object);

/* If status is NX_SUCCESS, the my_object now contains the ipAdEntAddr value. */
```

## nx_snmp_object_ip_address_set
Set IPv4 address object 

### Prototype

```c
UINT  nx_snmp_object_ip_address_set(
    VOID *destination_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service sets the IPv4 address at the address specified by the
destination pointer with the IP address in the NetX Duo object data
structure. This routine is typically called from the SET application
callback routine.

### Input Parameters

- *destination_ptr* Pointer to IP address to set.
- *object_data* Pointer to IP address object structure.

### Return Values

- **NX_SUCCESS** (0x00) The IP address object has been successfully set.
- **NX_SNMP_ERROR_WRONGTYPE** (0x07) Invalid object type.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Set the value of atNetworkAddress to the IP address in my_object.  */
status =  nx_snmp_object_ip_address_set(&atNetworkAddress, my_object);

/* If status is NX_SUCCESS, atNetWorkAddress now contains the new IP address. */
```

## nx_snmp_object_ipv6_address_set

Set IPv6 address object

### Prototype

```c
UINT  nx_snmp_object_ipv6_address_set(
    VOID *destination_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service sets the IPv6 address at the address specified by the
destination pointer with the IP address in the NetX Duo object data
structure. This routine is typically called from the SET application
callback routine.

### Input Parameters

- *destination_ptr* Pointer to IP address to set.
- *object_data* Pointer to IP address object structure.

### Return Values

- **NX_SUCCESS** (0x00) The IPv6 address has been successfully set.
- **NX_SNMP_ERROR_WRONGTYPE** (0x07) Invalid object type.
- **NX_NOT_ENABLED** (0x14) IPv6 not enabled
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Set the value of atNetworkAddress to the IP address in my_object.  */
status =  nx_snmp_object_ipv6_address_set(&atNetworkAddress, my_object);

/* If status is NX_SUCCESS, atNetWorkAddress now contains the new IP address. */
```

## nx_snmp_object_no_instance

Set no-instance object

### Prototype

```c
UINT  nx_snmp_object_no_instance(
    VOID *not_used_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service creates an object signaling that there was no instance of
the specified object and is typically called from the GET or GETNEXT
application callback routine.

### Input Parameters

- *not_used_ptr* Pointer not used – should be NX_NULL.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The no-instance object has been successfully built.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Place no-instance value in my_object.  */
status =  nx_snmp_object_no_instance(NX_NULL, my_object);

/* If status is NX_SUCCESS, the my_object is now a no-instance object. */
```

## nx_snmp_object_not_found
Set not-found object 

### Prototype

```c
UINT  nx_snmp_object_not_found(
    VOID *not_used_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service creates an object signaling the object was not found and is
typically called from the GET or GETNEXT application callback routine.

### Input Parameters

- *not_used_ptr* Pointer not used – should be NX_NULL.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The not-found object has been successfully built.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Place not-found value in my_object.  */
status =  nx_snmp_object_not_found(NX_NULL, my_object);

/* If status is NX_SUCCESS, the my_object is now a not-found object. */
```

## nx_snmp_object_octet_string_get
Get octet string object 

### Prototype

```c
UINT  nx_snmp_object_octet_string_get(
    VOID *source_ptr,
    NX_SNMP_OBJECT_DATA *object_data, 
    UINT length);
```
### Description

This service retrieves the octet string at the address specified by the
source pointer and places it in the NetX Duo object data structure. This
routine is typically called from the GET or GETNEXT application callback
routine.

### Input Parameters

- *source_ptr* Pointer to octet string source.
- *object_data* Pointer to destination object structure.
- *length* Number of bytes in octet string.

### Return Values

- **NX_SUCCESS** (0x00) The octet string object has been successfully retrieved.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Get the value of the 6-byte ifPhysAddress (1.3.6.1.2.1.2.2.1.6.0) and place 
   it in my_object for return.  */
status =  nx_snmp_object_octet_string_get(ifPhysAddress, my_object, 6);

/* If status is NX_SUCCESS, the my_object now contains the ifPhysAddress value. */
```

## nx_snmp_object_octet_string_set
Set octet string object 

### Prototype

```c
UINT  nx_snmp_object_octet_string_set(
    VOID *destination_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service sets the octet string at the address specified by the
destination pointer with the octet string in the NetX Duo object data
structure. This routine is typically called from the SET application
callback routine.

### Input Parameters

- *destination_ptr* Pointer to octet string destination.
- *object_data* Pointer to octet string source object structure.

### Return Values

- **NX_SUCCESS** (0x00) The octet string object has been successfully set.
- **NX_SNMP_ERROR_WRONGTYPE** (0x07) Invalid object type.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Set the value of sysContact (1.3.6.1.2.1.1.4.0) from the 
   octet string in my_object.  */
status =  nx_snmp_object_octet_string_set(sysContact, my_object);

/* If status is NX_SUCCESS, sysContact now contains the new octet string. */
```

## nx_snmp_object_string_get
Get ASCII string object 

### Prototype

```c
UINT  nx_snmp_object_string_get(
    VOID *source_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service retrieves the ASCII string at the address specified by the
source pointer and places it in the NetX Duo object data structure. This
routine is typically called from the GET or GETNEXT application callback
routine.

### Input Parameters

- *source_ptr* Pointer to ASCII string source.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The ASCII string object has been successfully retrieved.
- **NX_SNMP_ERROR** (0x100) String is too big  
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Get the value of the sysDescr (1.3.6.1.2.1.1.1.0) and place 
   it in my_object for return.  */
status =  nx_snmp_object_string_get(sysDescr, my_object);

/* If status is NX_SUCCESS, the my_object now contains the sysDescr string. */
```

## nx_snmp_object_string_set
Set ASCII string object 

### Prototype

```c
UINT  nx_snmp_object_string_set(
    VOID *destination_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service sets the ASCII string at the address specified by the
destination pointer with the ASCII string in the NetX Duo object data
structure. This routine is typically called from the SET application
callback routine.

### Input Parameters

- *destination_ptr* Pointer to ASCII string destination.
- *object_data* Pointer to ASCII string source object structure.

### Return Values

- **NX_SUCCESS** (0x00) The ASCII string object has been successfully set.
- **NX_SNMP_ERROR** (0x100) String too large.
- **NX_SNMP_ERROR_BADVALUE** (0x03) Invalid character in string
- **NX_SNMP_ERROR_WRONGTYPE** (0x07) Invalid object type.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Set the value of sysContact (1.3.6.1.2.1.1.4.0) from the 
   ASCII string in my_object.  */
status =  nx_snmp_object_string_set(sysContact, my_object);

/* If status is NX_SUCCESS, sysContact now contains the new ASCII string. */
```

## nx_snmp_object_timetics_get
Get timetics object 

### Prototype

```c
UINT  nx_snmp_object_timetics_get(
    VOID *source_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service retrieves the timetics at the address specified by the
source pointer and places it in the NetX Duo object data structure. This
routine is typically called from the GET or GETNEXT application callback
routine.

### Input Parameters

- *source_ptr* Pointer to timetics source.
- *object_data* Pointer to destination object structure.

### Return Values

- **NX_SUCCESS** (0x00) The timetics object has been successfully retrieved.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Get the value of the sysUpTime (1.3.6.1.2.1.1.3.0) and place 
   it in my_object for return.  */
status =  nx_snmp_object_timetics_get(sysUpTime, my_object);

/* If status is NX_SUCCESS, the my_object now contains the sysUpTime value. */
```

## nx_snmp_object_timetics_set
Set timetics object 

### Prototype

```c
UINT  nx_snmp_object_timetics_set(
    VOID *destination_ptr, 
    NX_SNMP_OBJECT_DATA *object_data);
```

### Description

This service sets the timetics variable at the address specified by the
destination pointer with the timetics in the NetX Duo object data structure.
This routine is typically called from the SET application callback
routine.

### Input Parameters

- *destination_ptr* Pointer to timetics destination.
- *object_data* Pointer to timetics source object structure.

### Return Values

- **NX_SUCCESS** (0x00) The timetics object has been successfully set.
- **NX_SNMP_ERROR_WRONGTYPE** (0x07) Invalid object type.
- **NX_PTR_ERROR** (0x07) Invalid input pointer

### Allowed From

Initialization, Threads

### Example

```c
/* Set the value of "my_time" from the timetics value in my_object.  */
status =  nx_snmp_object_timetics_set(&my_time, my_object);

/* If status is NX_SUCCESS, my_time now contains the new timetics. */
```