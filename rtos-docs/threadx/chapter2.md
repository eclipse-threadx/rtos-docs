---
title: Chapter 2 - Installation and Use of ThreadX
description: This chapter contains a description of various issues related to installation, setup, and usage of the high-performance ThreadX kernel.
---
# Chapter 2 - Installation and Use of ThreadX

This chapter contains a description of various issues related to installation, setup, and usage of the high-performance ThreadX kernel.

## Host Considerations

Embedded software is usually developed on Windows or Linux (Unix) host computers. After the application is compiled, linked, and located on the host, it is downloaded to the target hardware for execution.

Usually the target download is done from within the development tool debugger. After the download has completed, the debugger is responsible for providing target execution control (go, halt, breakpoint, etc.) as well as access to memory and processor registers.

Most development tool debuggers communicate with the target hardware via on-chip debug (OCD) connections such as JTAG (IEEE 1149.1) and Background Debug Mode (BDM). Debuggers also communicate with target hardware through In-Circuit Emulation (ICE) connections. Both OCD and ICE connections provide robust solutions with minimal intrusion on the target resident
software.

As for resources used on the host, the source code for ThreadX is delivered in ASCII format and requires approximately 1 MByte of space on the host computer's hard disk.

## Target Considerations

ThreadX requires between 2 KBytes and 20 KBytes of Read Only Memory (ROM) on the target. It also requires another 1 to 2 KBytes of the target's Random Access Memory (RAM) for the ThreadX system stack and other global data structures.

For timer-related functions like service call time-outs, time-slicing, and application timers to function, the underlying target hardware must provide a periodic interrupt source. If the processor has this capability, it is utilized by ThreadX. Otherwise, if the target processor does not have the ability to generate a periodic interrupt, the user's hardware must provide it. Setup and configuration of the timer interrupt is typically located in the ***tx_initialize_low_level*** assembly file in the ThreadX
distribution.

> **Note:** *ThreadX is still functional even if no periodic timer interrupt source is available. However, none of the timer-related services are functional.*

In function ***_tx_initialize_low_level***, the first-available RAM address is saved in global variable **_tx_initialize_unused_memory**. This address is later passed into ***tx_application_define*** (see ./chapter3.md#application-definition-function). The exception vector table and interrupt priorities may also be configured in ***_tx_initialize_low_level***.

## Product Distribution

ThreadX can be obtained from our public source code repository at <https://github.com/eclipse-threadx/threadx/>.

The following is a list of several important files in the repository.

| Filename | Description |
|------------------- | ----------- |
| **tx_api.h**                    	| C header file containing all system equates, data structures, and service prototypes.                                                         	|
| **tx_port.h**                   	| C header file containing all development tool and target specific data definitions and structures.                                             	|
| **demo_threadx.c**              	| C file containing a small demo application.                                                                                                   	|
| **tx.a (or tx.lib)**            	| Binary version of the ThreadX C library that is distributed with the *standard* package.                                                      	|
|                                 	|                                                                                                                                               	|

> **Note:** *All file names are in lower-case. This naming convention makes it easier to convert the commands to Linux (Unix) development platforms.*

## ThreadX Installation

ThreadX is installed by cloning the GitHub repository to your local machine. The following is typical syntax for creating a clone of the ThreadX repository on your PC.

```shell
    git clone https://github.com/eclipse-threadx/threadx
```

Alternatively you can download a copy of the repository using the download button on the GitHub main page.

You will also find instructions for building the ThreadX library on the front page of the online repository.

> **Note:** *Application software needs access to the ThreadX library file (usually **tx.a** or **tx.lib**) and the C include files ***tx_api.h*** and ***tx_port.h***. This is accomplished either by setting the appropriate path for the development tools or by copying these files into the application development area.*

## Using ThreadX

To use ThreadX, the application code must include ***tx_api.h*** during compilation and link with the ThreadX run-time library ***tx.a*** (or ***tx.lib***).

There are four steps required to build a ThreadX application.

1. Include the ***tx_api.h*** file in all application files that use ThreadX services or data structures.

1. Create the standard C ***main*** function. This function must eventually call ***tx_kernel_enter*** to start ThreadX. Application-specific initialization that does not involve ThreadX may be added prior to entering the kernel.

   > **Important:** *The ThreadX entry function ***tx_kernel_enter*** does not return. So be sure not to place any processing or function calls after it.*

1. Create the ***tx_application_define*** function. This is where the initial system resources are created. Examples of system resources include threads, queues, memory pools, event flags groups, mutexes, and semaphores.

1. Compile application source and link with the ThreadX run-time library ***tx.lib***. The resulting image can be downloaded to the target and executed!

## Small Example System

The small example system in Figure 1 shows the creation of a single thread with a priority of 3. The thread executes, increments a counter, then sleeps for one clock tick.
This process continues forever.

```c
#include "tx_api.h"
unsigned long my_thread_counter = 0;
TX_THREAD my_thread;
main( )
{
    /* Enter the ThreadX kernel. */
    tx_kernel_enter( );
}
void tx_application_define(void *first_unused_memory)
{
    /* Create my_thread! */
    tx_thread_create(&my_thread, "My Thread",
    my_thread_entry, 0x1234, first_unused_memory, 1024,
    3, 3, TX_NO_TIME_SLICE, TX_AUTO_START);
}
void my_thread_entry(ULONG thread_input)
{
    /* Enter into a forever loop. */
    while(1)
    {
        /* Increment thread counter. */
        my_thread_counter++;
        /* Sleep for 1 tick. */
        tx_thread_sleep(1);
    }
}
```

**FIGURE 1. Template for Application Development**

Although this is a simple example, it provides a good template for real application development.

## Troubleshooting

Each ThreadX port is delivered with a demonstration application. It is always a good idea to first get the demonstration system running—either on actual target hardware or simulated environment.

If the demonstration system does not execute properly, the following are some troubleshooting tips.

1. Determine how much of the demonstration is running.
1. Increase stack sizes (this is more important in actual application code than it is for the demonstration).
1. Rebuild the ThreadX library with TX_ENABLE_STACK_CHECKING defined. This enables the built-in ThreadX stack checking.
1. Temporarily bypass any recent changes to see if the problem disappears or changes.

## Configuration Options

There are several configuration options when building the ThreadX library and the application using ThreadX. The options below can be defined in the application source, on the command line, or within the ***tx_user.h*** include file.

> **Important:** *Options defined in ***tx_user.h*** are applied only if the application and ThreadX library are built with **TX_INCLUDE_USER_DEFINE_FILE** defined.*

### Smallest Configuration

For the smallest code size, the following ThreadX configuration options should be considered (in absence of all other options).

```c
TX_DISABLE_ERROR_CHECKING
TX_DISABLE_PREEMPTION_THRESHOLD
TX_DISABLE_NOTIFY_CALLBACKS
TX_DISABLE_REDUNDANT_CLEARING
TX_DISABLE_STACK_FILLING
TX_NOT_INTERRUPTABLE
TX_TIMER_PROCESS_IN_ISR
```

### Fastest Configuration

For the fastest execution, the same configuration options used for the Smallest Configuration previously, but with these options also considered.

```c
TX_REACTIVATE_INLINE
TX_INLINE_THREAD_RESUME_SUSPEND
```

[Detailed configuration options](#detailed-configuration-options) are described.

### Global Time Source

For other Eclipse ThreadX products (FileX, NetX Duo, GUIX, USBX, etc.), ThreadX defines the number of ThreadX timer ticks that represents one second. Others derive their time requirements based on this constant. By default, the value is 100, assuming a 10ms periodic interrupt. The user may override this value by defining TX_TIMER_TICKS_PER_SECOND with the desired value in ***tx_port.h*** or within the IDE or command line.

### Detailed Configuration Options

**TX_BLOCK_POOL_ENABLE_PERFORMANCE_INFO**

When defined, this option enables the gathering of performance information on block pools. By default, this option is not defined.

**TX_BYTE_POOL_ENABLE_PERFORMANCE_INFO**

When defined, this option enables the gathering of performance information on byte pools. By default, this option is not defined.

**TX_DISABLE_ERROR_CHECKING**

Bypasses basic service call error checking. When defined in the application source, all basic parameter error checking is disabled. This may improve performance by as much as 30% and may also reduce the image size.

> **Note:** *It is only safe to disable error checking if the application can absolutely guarantee all input parameters are always valid under all circumstances, including input parameters derived from external input. If invalid input is supplied to the API with error checking disabled, the resulting behavior is undefined and could result in memory corruption or system crash.*

> **Note:** *ThreadX API return values not affected by disabling error checking are listed in bold in the "Return Values" section of each API description in Chapter 4. The nonbold return values are void if error checking is disabled by using the TX_DISABLE_ERROR_CHECKING option.*

**TX_DISABLE_NOTIFY_CALLBACKS**

When defined, disables the notify callbacks for various ThreadX objects. Using this option slightly reduces code size and improves performance. By default, this option is not defined.

**TX_DISABLE_PREEMPTION_THRESHOLD**

When defined, disables the preemption-threshold feature and slightly reduces code size and improves performance. Of course, the preemption-threshold capabilities are no longer available. By default, this option is not defined.

**TX_DISABLE_REDUNDANT_CLEARING**

When defined, removes the logic for initializing ThreadX global C data structures to zero. This should only be used if the compiler's initialization code sets all un-initialized C global data to zero. Using this option slightly reduces code size and improves performance during initialization. By default, this option is not defined.

**TX_DISABLE_STACK_FILLING**

When defined, disables placing the 0xEF value in each byte of each thread's stack when created. By default, this option is not defined.

**TX_ENABLE_EVENT_TRACE**

When defined, ThreadX enables the event gathering code for creating a TraceX trace buffer.

**TX_ENABLE_STACK_CHECKING**

When defined, enables ThreadX run-time stack checking, which includes analysis of how much stack has been used and examination of data pattern "fences" before and after the stack area. If a stack error is detected, the registered application stack error handler is called. This option does result in slightly increased overhead and code size. Review the ***tx_thread_stack_error_notify*** API function for more information. By default, this option is not defined.

**TX_EVENT_FLAGS_ENABLE_PERFORMANCE_INFO**

When defined, enables the gathering of performance information on event flags groups. By default, this option is not defined.

**TX_INLINE_THREAD_RESUME_SUSPEND**

When defined, ThreadX improves the ***tx_thread_resume*** and ***tx_thread_suspend*** API calls via in-line code. This increases code size but enhances performance of these two API calls.

**TX_MAX_PRIORITIES**

Defines the priority levels for ThreadX. Legal values range from 32 through 1024 (inclusive) and *must* be evenly divisible by 32. Increasing the number of priority levels supported increases the RAM usage by 128 bytes for every group of 32 priorities. However, there is only a negligible effect on performance. By default, this value is set to 32 priority levels.

**TX_MINIMUM_STACK**

Defines the minimum stack size (in bytes). It is used for error checking when threads are created. The default value is port-specific and is found in ***tx_port.h***.

**TX_MISRA_ENABLE**

When defined, ThreadX utilizes MISRA C compliant conventions. Refer to ***[ThreadX SMP MISRA C compliance](appendix-e.md)*** for details.

**TX_MUTEX_ENABLE_PERFORMANCE_INFO**

When defined, enables the gathering of performance information on mutexes. By default, this option is not defined.

**TX_NO_TIMER**

When defined, the ThreadX timer logic is completely disabled. This is useful in cases where the ThreadX timer features (thread sleep, API timeouts, time-slicing, and application timers) are not utilized. If **TX_NO_TIMER** is specified, the option **TX_TIMER_PROCESS_IN_ISR** must also be defined.

**TX_NOT_INTERRUPTABLE**

When defined, ThreadX does not attempt to minimize interrupt lockout time. This results in faster execution but does slightly increase interrupt lockout time.

**TX_QUEUE_ENABLE_PERFORMANCE_INFO**

When defined, enables the gathering of performance information on queues. By default, this option is not defined.

**TX_REACTIVATE_INLINE**

When defined, performs reactivation of ThreadX timers inline instead of using a function call. This improves performance but slightly increases code size. By default, this option is not defined.

**TX_SEMAPHORE_ENABLE_PERFORMANCE_INFO**

When defined, enables the gathering of performance information on semaphores. By default, this option is not defined.

**TX_THREAD_ENABLE_PERFORMANCE_INFO**

When defined, enables the gathering of performance information on threads. By default, this option is not defined.

**TX_TIMER_ENABLE_PERFORMANCE_INFO**

When defined, enables the gathering of performance information on timers. By default, this option is not defined.

**TX_TIMER_PROCESS_IN_ISR**

When defined, eliminates the internal system timer thread for ThreadX. This results in improved performance on timer events and smaller RAM requirements because the timer stack and control block are no longer needed. However, using this option moves all the timer expiration processing to the timer ISR level. By default, this option is not defined.

> **Note:** *Services allowed from timers may not be allowed from ISRs and thus might not be allowed when using this option.*

**TX_TIMER_THREAD_PRIORITY**

Defines the priority of the internal ThreadX system timer thread. The default value is priority 0—the highest priority in ThreadX. The default value is defined in ***tx_port.h***.

**TX_TIMER_THREAD_STACK_SIZE**

Defines the stack size (in bytes) of the internal ThreadX system timer thread. This thread processes all thread sleep requests as well as all service call timeouts. In addition, all application timer callback routines are invoked from this context. The default value is port-specific and is found in ***tx_port.h***.

## ThreadX Version ID

The programmer can obtain the ThreadX version from examination of the ***tx_port.h*** file. In addition, this file also contains a version history of the corresponding port. Application software can obtain the ThreadX version by examining the global string **_tx_version_id**.
