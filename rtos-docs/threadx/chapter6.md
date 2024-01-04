---
title: Chapter 6 - Demonstration System for ThreadX
description: This chapter contains a description of the demonstration system that is delivered with all ThreadX processor support packages.
---
# Chapter 6 - Demonstration System for ThreadX

This chapter contains a description of the demonstration system that
is delivered with all ThreadX processor support packages.

## Overview

Each ThreadX product distribution contains a demonstration system that
runs on all supported microprocessors.

This example system is defined in the distribution file
***demo_threadx.c*** and is designed to illustrate how ThreadX is
used in an embedded multithread environment. The demonstration
consists of initialization, eight threads, one byte pool, one block
pool, one queue, one semaphore, one mutex, and one event flags group.

> **Note:** *Except for the thread's stack size, the demonstration application is
identical on all ThreadX supported processors.*

The complete listing of
***demo_threadx.c***, including the line numbers referenced
throughout the remainder of this chapter.

## Application Define

The ***tx_application_define*** function executes after the basic
ThreadX initialization is complete. It is responsible for setting up
all of the initial system resources, including threads, queues,
semaphores, mutexes, event flags, and memory pools.

The demonstration system's ***tx_application_define*** (*line
numbers 60-164*) creates the demonstration objects in the following
order:

- byte_pool_0
- thread_0
- thread_1
- thread_2
- thread_3
- thread_4
- thread_5
- thread_6
- thread_7
- queue_0
- semaphore_0
- event_flags_0
- mutex_0
- block_pool_0

The demonstration system does not create any other additional ThreadX
objects. However, an actual application may create system objects
during runtime inside of executing threads.

### Initial Execution

All threads are created with the **TX_AUTO_START**
option. This makes them initially ready for execution. After
***tx_application_define*** completes, control is transferred to the
thread scheduler and from there to each individual thread.

The order in which the threads execute is determined by their priority
and the order that they were created. In the demonstration system,
***thread_0*** executes first because it has the highest priority
(*it was created with a priority of 1*). After ***thread_0***
suspends, ***thread_5*** is executed, followed by the execution of
***thread_3***, ***thread_4***, ***thread_6***, ***thread_7***,
***thread_1***, and finally ***thread_2***.

> **Note:** *Even though **thread_3** and **thread_4** have the same priority
(both created with a priority of 8), **thread_3** executes first.
This is because **thread_3** was created and became ready before
**thread_4**. Threads of equal priority execute in a FIFO fashion.*

## Thread 0

The function ***thread_0_entry*** marks the entry point of the
thread *(lines 167-190*). ***Thread_0*** is the first thread in the
demonstration system to execute. Its processing is simple: it
increments its counter, sleeps for 10 timer ticks, sets an event flag
to wake up ***thread_5***, then repeats the sequence.

***Thread_0*** is the highest priority thread in the system. When its
requested sleep expires, it will preempt any other executing thread in
the demonstration.

## Thread 1

The function
***thread_1_entry*** marks the entry point of the thread *(lines
193-216*). ***Thread_1*** is the second-to-last thread in the
demonstration system to execute. Its processing consists of
incrementing its counter, sending a message to ***thread_2***
(*through* ***queue_0***), and repeating the sequence. Notice that
***thread_1*** suspends whenever ***queue_0*** becomes full (*line
207*).

## Thread 2

The function ***thread_2_entry*** marks the entry point of the
thread *(lines 219-243*). ***Thread_2*** is the last thread in the
demonstration system to execute. Its processing consists of
incrementing its counter, getting a message from ***thread_1***
(through ***queue_0***), and repeating the sequence. Notice that
***thread_2*** suspends whenever ***queue_0*** becomes empty (*line
233*).

Although ***thread_1*** and ***thread_2*** share the lowest priority
in the demonstration system (*priority 16*), they *Threads 3 and 4*

are also the only threads that are ready for execution most of the
time. They are also the only threads created with time-slicing (*lines
87 and 93*). Each thread is allowed to execute for a maximum of 4
timer ticks before the other thread is executed.

## Threads 3 and 4

The function
***thread_3_and_4_entry*** marks the entry point of both
***thread_3*** and ***thread_4*** (lines 246-280). Both threads
have a priority of 8, which makes them the third and fourth threads in
the demonstration system to execute. The processing for each thread is
the same: incrementing its counter, getting ***semaphore_0***,
sleeping for 2 timer ticks, releasing ***semaphore_0***, and
repeating the sequence. Notice that each thread suspends whenever
***semaphore_0*** is unavailable (line 264).

Also both threads use the same function for their main processing.
This presents no problems because they both have their own unique
stack, and C is naturally reentrant. Each thread determines which one
it is by examination of the thread input parameter (line 258), which
is setup when they are created (lines 102 and 109).

> **Note:** *It is also reasonable to obtain the current thread point during
thread execution and compare it with the control block's address to
determine thread identity.*

## Thread 5

The function ***thread_5_entry*** marks the entry point of the
thread (lines 283-305). ***Thread_5*** is the second thread in the
demonstration system to execute. Its processing consists of
incrementing its counter, getting an event flag from ***thread_0***
(through ***event_flags_0***), and repeating the sequence. Notice
that ***thread_5*** suspends whenever the event flag in
***event_flags_0*** is not available (line 298).

## Threads 6 and 7

The function ***thread_6_and_7_entry*** marks the entry point of
both ***thread_6*** and ***thread_7*** (lines 307-358). Both
threads have a priority of 8, which makes them the fifth and sixth
threads in the demonstration system to execute. The processing for
each thread is the same: incrementing its counter, getting
***mutex_0*** twice, sleeping for 2 timer ticks, releasing
***mutex_0*** twice, and repeating the sequence. Notice that each
thread suspends whenever ***mutex_0*** is unavailable (line 325).

Also both threads use the same
function for their main processing. This presents no problems because
they both have their own unique stack, and C is naturally reentrant.
Each thread determines which one it is by examination of the thread
input parameter (line 319), which is setup when they are created
(lines 126 and 133).

## Observing the Demonstration

Each of the demonstration threads increments its own unique counter.
The following counters may be examined to check on the demo's
operation:

- thread_0_counter
- thread_1_counter
- thread_2_counter
- thread_3_counter
- thread_4_counter
- thread_5_counter
- thread_6_counter
- thread_7_counter

Each of these counters should continue to increase as the demonstration executes, with
***thread_1_counter*** and ***thread_2_counter*** increasing at the fastest rate.

## Distribution file: demo_threadx.c

This section displays the complete listing of ***demo_threadx.c***,
including the line numbers referenced throughout this chapter.

```c
/* This is a small demo of the high-performance ThreadX kernel. It includes examples of eight
threads of different priorities, using a message queue, semaphore, mutex, event flags group,
byte pool, and block pool. */

#include "tx_api.h"

#define DEMO_STACK_SIZE 1024
#define DEMO_BYTE_POOL_SIZE 9120
#define DEMO_BLOCK_POOL_SIZE 100
#define DEMO_QUEUE_SIZE 100

/* Define the ThreadX object control blocks... */

TX_THREAD thread_0;
TX_THREAD thread_1;
TX_THREAD thread_2;
TX_THREAD thread_3;
TX_THREAD thread_4;
TX_THREAD thread_5;
TX_THREAD thread_6;
TX_THREAD thread_7;
TX_QUEUE queue_0;
TX_SEMAPHORE semaphore_0;
TX_MUTEX mutex_0;
TX_EVENT_FLAGS_GROUP event_flags_0;
TX_BYTE_POOL byte_pool_0;
TX_BLOCK_POOL block_pool_0;

/* Define the counters used in the demo application... */

ULONG thread_0_counter;
ULONG thread_1_counter;
ULONG thread_1_messages_sent;
ULONG thread_2_counter;
ULONG thread_2_messages_received;
ULONG thread_3_counter;
ULONG thread_4_counter;
ULONG thread_5_counter;
ULONG thread_6_counter;
ULONG thread_7_counter;

/* Define thread prototypes. */

void thread_0_entry(ULONG thread_input);
void thread_1_entry(ULONG thread_input);
void thread_2_entry(ULONG thread_input);
void thread_3_and_4_entry(ULONG thread_input);
void thread_5_entry(ULONG thread_input);
void thread_6_and_7_entry(ULONG thread_input);


/* Define main entry point. */

int main()
{
    /* Enter the ThreadX kernel. */
    tx_kernel_enter();
}

/* Define what the initial system looks like. */
void tx_application_define(void *first_unused_memory)
{

    CHAR *pointer;

    /* Create a byte memory pool from which to allocate the thread stacks. */
    tx_byte_pool_create(&byte_pool_0, "byte pool 0", first_unused_memory,
        DEMO_BYTE_POOL_SIZE);

    /* Put system definition stuff in here, e.g., thread creates and other assorted
        create information. */

    /* Allocate the stack for thread 0. */
    tx_byte_allocate(&byte_pool_0, &pointer, DEMO_STACK_SIZE, TX_NO_WAIT);

    /* Create the main thread. */
    tx_thread_create(&thread_0, "thread 0", thread_0_entry, 0,
        pointer, DEMO_STACK_SIZE,
        1, 1, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the stack for thread 1. */
    tx_byte_allocate(&byte_pool_0, &pointer, DEMO_STACK_SIZE, TX_NO_WAIT);

    /* Create threads 1 and 2. These threads pass information through a ThreadX
        message queue. It is also interesting to note that these threads have a time
        slice. */
    tx_thread_create(&thread_1, "thread 1", thread_1_entry, 1,
        pointer, DEMO_STACK_SIZE,
        16, 16, 4, TX_AUTO_START);

    /* Allocate the stack for thread 2. */
    tx_byte_allocate(&byte_pool_0, &pointer, DEMO_STACK_SIZE, TX_NO_WAIT);
        tx_thread_create(&thread_2, "thread 2", thread_2_entry, 2,
        pointer, DEMO_STACK_SIZE,
        16, 16, 4, TX_AUTO_START);

    /* Allocate the stack for thread 3. */
    tx_byte_allocate(&byte_pool_0, &pointer, DEMO_STACK_SIZE, TX_NO_WAIT);

    /* Create threads 3 and 4. These threads compete for a ThreadX counting semaphore.
        An interesting thing here is that both threads share the same instruction area. */
    tx_thread_create(&thread_3, "thread 3", thread_3_and_4_entry, 3,
        pointer, DEMO_STACK_SIZE,
        8, 8, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the stack for thread 4. */
    tx_byte_allocate(&byte_pool_0, &pointer, DEMO_STACK_SIZE, TX_NO_WAIT);

    tx_thread_create(&thread_4, "thread 4", thread_3_and_4_entry, 4,
        pointer, DEMO_STACK_SIZE,
        8, 8, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the stack for thread 5. */
    tx_byte_allocate(&byte_pool_0, &pointer, DEMO_STACK_SIZE, TX_NO_WAIT);

    /* Create thread 5. This thread simply pends on an event flag, which will be set
        by thread_0. */
    tx_thread_create(&thread_5, "thread 5", thread_5_entry, 5,
        pointer, DEMO_STACK_SIZE,
        4, 4, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the stack for thread 6. */
    tx_byte_allocate(&byte_pool_0, &pointer, DEMO_STACK_SIZE, TX_NO_WAIT);

    /* Create threads 6 and 7. These threads compete for a ThreadX mutex. */
    tx_thread_create(&thread_6, "thread 6", thread_6_and_7_entry, 6,
        pointer, DEMO_STACK_SIZE,
        8, 8, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the stack for thread 7. */
    tx_byte_allocate(&byte_pool_0, &pointer, DEMO_STACK_SIZE, TX_NO_WAIT);

    tx_thread_create(&thread_7, "thread 7", thread_6_and_7_entry, 7,
        pointer, DEMO_STACK_SIZE,
        8, 8, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the message queue. */
    tx_byte_allocate(&byte_pool_0, &pointer, DEMO_QUEUE_SIZE*sizeof(ULONG), TX_NO_WAIT);

    /* Create the message queue shared by threads 1 and 2. */
    tx_queue_create(&queue_0, "queue 0", TX_1_ULONG, pointer, DEMO_QUEUE_SIZE*sizeof(ULONG));

    /* Create the semaphore used by threads 3 and 4. */
    tx_semaphore_create(&semaphore_0, "semaphore 0", 1);

    /* Create the event flags group used by threads 1 and 5. */
    tx_event_flags_create(&event_flags_0, "event flags 0");

    /* Create the mutex used by thread 6 and 7 without priority inheritance. */
    tx_mutex_create(&mutex_0, "mutex 0", TX_NO_INHERIT);

    /* Allocate the memory for a small block pool. */
    tx_byte_allocate(&byte_pool_0, &pointer, DEMO_BLOCK_POOL_SIZE, TX_NO_WAIT);

    /* Create a block memory pool to allocate a message buffer from. */
    tx_block_pool_create(&block_pool_0, "block pool 0", sizeof(ULONG), pointer,
        DEMO_BLOCK_POOL_SIZE);

    /* Allocate a block and release the block memory. */
    tx_block_allocate(&block_pool_0, &pointer, TX_NO_WAIT);

    /* Release the block back to the pool. */
    tx_block_release(pointer);
}

/* Define the test threads. */
void thread_0_entry(ULONG thread_input)
{
    UINT status;


    /* This thread simply sits in while-forever-sleep loop. */
    while(1)
    {

        /* Increment the thread counter. */
        thread_0_counter++;

        /* Sleep for 10 ticks. */
        tx_thread_sleep(10);

        /* Set event flag 0 to wakeup thread 5. */
        status = tx_event_flags_set(&event_flags_0, 0x1, TX_OR);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;
    }
}


void thread_1_entry(ULONG thread_input)
{
    UINT status;


    /* This thread simply sends messages to a queue shared by thread 2. */
    while(1)
    {
        /* Increment the thread counter. */
        thread_1_counter++;

        /* Send message to queue 0. */
        status = tx_queue_send(&queue_0, &thread_1_messages_sent, TX_WAIT_FOREVER);

        /* Check completion status. */
        if (status != TX_SUCCESS)
            break;

        /* Increment the message sent. */
        thread_1_messages_sent++;
    }
}


void thread_2_entry(ULONG thread_input)
{
    ULONG received_message;
    UINT status;

    /* This thread retrieves messages placed on the queue by thread 1. */
    while(1)
    {
        /* Increment the thread counter. */
        thread_2_counter++;

        /* Retrieve a message from the queue. */
        status = tx_queue_receive(&queue_0, &received_message, TX_WAIT_FOREVER);

        /* Check completion status and make sure the message is what we
        expected. */
        if ((status != TX_SUCCESS) || (received_message != thread_2_messages_received))
            break;

        /* Otherwise, all is okay. Increment the received message count. */
        thread_2_messages_received++;
    }
}


void thread_3_and_4_entry(ULONG thread_input)
{
    UINT status;


    /* This function is executed from thread 3 and thread 4. As the loop
    below shows, these function compete for ownership of semaphore_0. */
    while(1)
    {
        /* Increment the thread counter. */
        if (thread_input == 3)
            thread_3_counter++;
        else
            thread_4_counter++;

        /* Get the semaphore with suspension. */
        status = tx_semaphore_get(&semaphore_0, TX_WAIT_FOREVER);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;

        /* Sleep for 2 ticks to hold the semaphore. */
        tx_thread_sleep(2);

        /* Release the semaphore. */
        status = tx_semaphore_put(&semaphore_0);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;
    }
}


void thread_5_entry(ULONG thread_input)
{
    UINT status;
    ULONG actual_flags;


    /* This thread simply waits for an event in a forever loop. */
    while(1)
    {
        /* Increment the thread counter. */
        thread_5_counter++;

        /* Wait for event flag 0. */
        status = tx_event_flags_get(&event_flags_0, 0x1, TX_OR_CLEAR,
            &actual_flags, TX_WAIT_FOREVER);

        /* Check status. */
        if ((status != TX_SUCCESS) || (actual_flags != 0x1))
            break;
    }
}

void thread_6_and_7_entry(ULONG thread_input)
{
    UINT status;

    /* This function is executed from thread 6 and thread 7. As the loop
        below shows, these function compete for ownership of mutex_0. */
    while(1)
    {
        /* Increment the thread counter. */
        if (thread_input == 6)
            thread_6_counter++;
        else
            thread_7_counter++;

        /* Get the mutex with suspension. */
        status = tx_mutex_get(&mutex_0, TX_WAIT_FOREVER);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;

        /* Get the mutex again with suspension. This shows
            that an owning thread may retrieve the mutex it
            owns multiple times. */
        status = tx_mutex_get(&mutex_0, TX_WAIT_FOREVER);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;

        /* Sleep for 2 ticks to hold the mutex. */
        tx_thread_sleep(2);

        /* Release the mutex. */
        status = tx_mutex_put(&mutex_0);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;

        /* Release the mutex again. This will actually
            release ownership since it was obtained twice. */
        status = tx_mutex_put(&mutex_0);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;
    }
}
```
