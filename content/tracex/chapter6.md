---
title: Chapter 6 - ThreadX trace events
description: This chapter describes the ThreadX events. 
---


This chapter describes the ThreadX events. 

## List of Events and Icons

The following is a list of ThreadX events displayed by TraceX:

| **Icon**                         | **Meaning** |
| -------------------------------- | ------------------------------------- |
| ![Internal thread resume icon](../media/user-guide/tx-events/image1.png)    | Internal thread resume  |
| ![Internal thread suspend icon](../media/user-guide/tx-events/image2.png)    | Internal thread suspend |
| ![Interrupt Service Routine Enter icon](../media/user-guide/tx-events/image3.png)    | Interrupt Service Routine (ISR) Enter |
| ![Interrupt Service Routine Exit icon](../media/user-guide/tx-events/image4.png)    | Interrupt Service Routine (ISR) Exit  |
| ![Internal time-slice icon](../media/user-guide/tx-events/image5.png)    | Internal time-slice |
| ![Running icon](../media/user-guide/tx-events/image6.png)    | Running |
| ![Block pool allocate icon](../media/user-guide/tx-events/image7.png)    | **Block pool allocate** (*tx_block_allocate*)  |
| ![Block pool create icon](../media/user-guide/tx-events/image8.png)    | **Block pool create** (*tx_block_pool_create*) |
| ![Block pool delete icon](../media/user-guide/tx-events/image9.png)    | **Block pool delete** (*tx_block_pool_delete*) |
| ![Block pool information get icon](../media/user-guide/tx-events/image10.png)    | **Block pool information get** (*tx_block_pool_info_get*) |
| ![Block pool performance information get con](../media/user-guide/tx-events/image11.png)    | **Block pool performance information get** (*tx_block_pool_performance_info_get*) |
| ![Block pool system performance information get icon](../media/user-guide/tx-events/image12.png)    | **Block pool system performance information get** (*tx_block_pool_performance_system_info_get*) |
| ![Block pool prioritize icon](../media/user-guide/tx-events/image13.png)    | **Block pool prioritize** (*tx_block_pool_prioritize*) |
| ![Block release to pool icon](../media/user-guide/tx-events/image14.png)    | **Block release to pool** (*tx_block_release*) |
| ![Byte pool allocate memory icon](../media/user-guide/tx-events/image15.png)    | **Byte pool allocate memory** (*tx_byte_allocate*) |
| ![Byte pool create icon](../media/user-guide/tx-events/image16.png)    | **Byte pool create** (*tx_byte_pool_create*) |
| ![Byte pool delete icon](../media/user-guide/tx-events/image17.png)    | **Byte pool delete** (*tx_byte_pool_delete*) |
| ![Byte pool information get icon](../media/user-guide/tx-events/image18.png)    | **Byte pool information get** (*tx_byte_pool_info_get*) |
| ![Byte pool performance information get icon](../media/user-guide/tx-events/image19.png)    | **Byte pool performance information get** (*tx_byte_pool_performance_info_get*) |
| ![Byte pool system performance information get icon](../media/user-guide/tx-events/image20.png)    | **Byte pool system performance information get** (*tx_byte_pool_performance_system_info_get*) |
| ![*Byte pool prioritize icon](../media/user-guide/tx-events/image21.png)    | **Byte pool prioritize** (*tx_byte_pool_prioritize*) |
| ![Byte memory release to pool icon](../media/user-guide/tx-events/image22.png)    | **Byte memory release to pool** (*tx_byte_release*) |
| ![Event flags create icon](../media/user-guide/tx-events/image23.png)    | **Event flags create** (*tx_event_flags_create*) |
| ![Event flags delete icon](../media/user-guide/tx-events/image24.png)    | **Event flags delete** (*tx_event_flags_delete*) |
| ![Event flags get icon](../media/user-guide/tx-events/image25.png)    | **Event flags get** (*tx_event_flags_get*) |
| ![Event flags information get icon](../media/user-guide/tx-events/image26.png)    | **Event flags information get** (*tx_event_flags_info_get*) |
| ![Event flags performance information get icon](../media/user-guide/tx-events/image27.png)    | **Event flags performance information get** (*tx_event_flags_performance_info_get*) |
| ![Event flags system performance information get icon](../media/user-guide/tx-events/image28.png)    | **Event flags system performance information get** (*tx_event_flags_performance_system_info_get*) |
| ![Event flags set icon](../media/user-guide/tx-events/image29.png)    | **Event flags set** (*tx_event_flags_set*) |
| ![Event flags set notify icon](../media/user-guide/tx-events/image30.png)    | **Event flags set notify** (*tx_event_flags_set_notify*) |
| ![Interrupt enable/disable icon](../media/user-guide/tx-events/image31.png)    | **Interrupt enable/disable** (*tx_interrupt_control*) |
| ![Mutex create icon](../media/user-guide/tx-events/image32.png)    | **Mutex create** (*tx_mutex_create*) |
| ![Mutex delete icon](../media/user-guide/tx-events/image33.png)    | **Mutex delete** (*tx_mutex_delete*) |
| ![Mutex get icon](../media/user-guide/tx-events/image34.png)    | **Mutex get** (*tx_mutex_get*) |
| ![Mutex information get icon](../media/user-guide/tx-events/image35.png)    | **Mutex information get** (*tx_mutex_info_get*) |
| ![Mutex performance information get icon](../media/user-guide/tx-events/image36.png)    | **Mutex performance information get** (*tx_mutex_performance_info_get*) |
| ![Mutex system performance information get icon](../media/user-guide/tx-events/image37.png)    | **Mutex system performance information get** (*tx_mutex_performance_system_info_get*) |
| ![Mutex prioritize icon](../media/user-guide/tx-events/image38.png)    | **Mutex prioritize** (*tx_mutex_prioritize*) |
| ![Mutex put icon](../media/user-guide/tx-events/image39.png)    | **Mutex put** (*tx_mutex_put*) |
| ![Queue create icon](../media/user-guide/tx-events/image40.png)    | **Queue create** (*tx_queue_create*) |
| ![Queue delete icon](../media/user-guide/tx-events/image41.png)    | **Queue delete** (*tx_queue_delete*) |
| ![Queue flush icon](../media/user-guide/tx-events/image42.png)    | **Queue flush** (*tx_queue_flush*) |
| ![Queue front send icon](../media/user-guide/tx-events/image43.png)    | **Queue front send** (*tx_queue_front_send*) |
| ![Queue information get icon](../media/user-guide/tx-events/image44.png)    | **Queue information get** (*tx_queue_info_get*) |
| ![Queue performance information get icon](../media/user-guide/tx-events/image45.png)    | **Queue performance information get** (*tx_queue_performance_info_get*) |
| ![Queue system performance information get icon](../media/user-guide/tx-events/image46.png)    | **Queue system performance information get** (*tx_queue_performance_system_info_get*) |
| ![Queue prioritize icon](../media/user-guide/tx-events/image47.png)    | **Queue prioritize** (*tx_queue_prioritize*) |
| ![Queue receive message icon](../media/user-guide/tx-events/image48.png)    | **Queue receive message** (*tx_queue_receive*) |
| ![Queue send message icon](../media/user-guide/tx-events/image49.png)    | **Queue send message** (*tx_queue_send*) |
| ![Queue send notify icon](../media/user-guide/tx-events/image50.png)    | **Queue send notify** (*tx_queue_send_notify*) |
| ![Semaphore ceiling put icon](../media/user-guide/tx-events/image51.png)    | **Semaphore ceiling put** (*tx_semaphore_ceiling_put*) |
| ![Semaphore create icon](../media/user-guide/tx-events/image52.png)    | **Semaphore create** (*tx_semaphore_create*) |
| ![Semaphore delete icon](../media/user-guide/tx-events/image53.png)    | **Semaphore delete** (*tx_semaphore_delete*) |
| ![Semaphore get icon](../media/user-guide/tx-events/image54.png)    | **Semaphore get** (*tx_semaphore_get*) |
| ![Semaphore information get icon](../media/user-guide/tx-events/image55.png)    | **Semaphore information get** (*tx_semaphore_info_get*) |
| ![Semaphore performance information get icon](../media/user-guide/tx-events/image56.png)    | **Semaphore performance information get** (*tx_semaphore_performance_info_get*) |
| ![Semaphore system performance information get icon](../media/user-guide/tx-events/image57.png)    | **Semaphore system performance information get** (*tx_semaphore_performance_system_info_get*) |
| ![Semaphore prioritize icon](../media/user-guide/tx-events/image58.png)    | **Semaphore prioritize** (*tx_semaphore_prioritize*) |
| ![Semaphore put icon](../media/user-guide/tx-events/image59.png)    | **Semaphore put** (*tx_semaphore_put*) |
| ![Semaphore put notify icon](../media/user-guide/tx-events/image60.png)    | **Semaphore put notify** (*tx_semaphore_put_notify*) |
| ![Thread create icon](../media/user-guide/tx-events/image61.png)    | **Thread create** (*tx_thread_create*) |
| ![Thread delete icon](../media/user-guide/tx-events/image62.png)    | **Thread delete** (*tx_thread_delete*) |
| ![Thread exit/entry notify icon](../media/user-guide/tx-events/image63.png)    | **Thread exit/entry notify** (*tx_thread_entry_exit_notify*) |
| ![Thread identify icon](../media/user-guide/tx-events/image64.png)    | **Thread identify** (*tx_thread_identify*) |
| ![Thread information get icon](../media/user-guide/tx-events/image65.png)    | **Thread information get** (*tx_thread_info_get*) |
| ![Thread performance information get icon](../media/user-guide/tx-events/image66.png)    | **Thread performance information get** (*tx_thread_performance_info_get*) |
| ![Thread performance system information get icon](../media/user-guide/tx-events/image67.png)    | **Thread performance system information get** (*tx_thread_performance_system_info_get*) |
| ![Thread preemption change icon](../media/user-guide/tx-events/image68.png)    | **Thread preemption change** (*tx_thread_preemption_change*) |
| ![Thread priority change icon](../media/user-guide/tx-events/image69.png)    | **Thread priority change** (*tx_thread_priority_change*) |
| ![Thread relinquish icon](../media/user-guide/tx-events/image70.png)    | **Thread relinquish** (*tx_thread_relinquish*) |
| ![Thread reset icon](../media/user-guide/tx-events/image71.png)    | **Thread reset** (*tx_thread_reset*) |
| ![*Thread resume icon](../media/user-guide/tx-events/image72.png)    | **Thread resume** (**tx_thread_resume*) |
| ![Thread Sleep icon](../media/user-guide/tx-events/image73.png)    | **Thread Sleep** (*tx_thread_sleep*)* |
| ![Thread stack error notify icon](../media/user-guide/tx-events/image74.png)    | **Thread stack error notify** (*tx_thread_stack_error_notify*) |
| ![Thread suspend icon](../media/user-guide/tx-events/image75.png)    | **Thread suspend** (*tx_thread_suspend*) |
| ![Thread terminate icon](../media/user-guide/tx-events/image76.png)    | **Thread terminate** (*tx_thread_terminate*) |
| ![Thread time-slice change icon](../media/user-guide/tx-events/image77.png)    | **Thread time-slice change** (*tx_thread_time_slice_change*) |
| ![Thread wait abort icon](../media/user-guide/tx-events/image78.png)    | **Thread wait abort** (*tx_thread_wait_abort*) |
| ![Time get icon](../media/user-guide/tx-events/image79.png)    | **Time get** (*tx_time_get*) |
| ![Time set icon](../media/user-guide/tx-events/image80.png)    | **Time set** (*tx_time_set*) |
| ![*Timer activate icon](../media/user-guide/tx-events/image81.png)    | **Timer activate** (*tx_timer_activate*) |
| ![Timer change icon](../media/user-guide/tx-events/image82.png)    | **Timer change** (*tx_timer_change*) |
| ![Timer create icon](../media/user-guide/tx-events/image83.png)    | **Timer create** (*tx_timer_create*) |
| ![Timer deactivate icon](../media/user-guide/tx-events/image84.png)    | **Timer deactivate** (*tx_timer_deactivate*) |
| ![Timer delete icon](../media/user-guide/tx-events/image85.png)    | **Timer delete** (*tx_timer_delete*) |
| ![Timer information get icon](../media/user-guide/tx-events/image86.png)    | **Timer information get** (*tx_timer_info_get*) |
| ![Timer performance information get icon](../media/user-guide/tx-events/image87.png)    | **Timer performance information get** (*tx_timer_performance_info_get*) |
| ![*Timer performance system information get icon](../media/user-guide/tx-events/image88.png)    | **Timer performance system information get** (*tx_timer_performance_system_info_get*) |
| ![User-Defined Eventicon](../media/user-guide/tx-events/image0.png)    | **User-Defined Event** (See Chapter 10)    |

## Event Descriptions

### Internal thread resume

#### Internal thread resume

**Icon** ![Internal thread resume icon](../media/user-guide/tx-events/image1.png)

**Description**

This event represents the internal processing in ThreadX that resumes a thread for execution. If the specified thread is the highest priority and preemption-threshold does not block its execution, the system will start executing this newly ready thread.

**Information Fields**

- Info Field 1: Pointer to the thread being resumed.
- Info Field 2: Previous state of the thread being resumed, as follows:

  |  Thread state                     | Value        |
  |---------------------------------- | --------|
  |  TX_READY                         | 0       |
  |  TX_COMPLETED                     | 1       |
  |  TX_TERMINATED                    | 2       |
  |  TX_SUSPENDED                     | 3       |
  |  TX_SLEEP                         | 4       |
  |  TX_QUEUE_SUSP                    | 5       |
  |  TX_SEMAPHORE_SUSP                | 6       |
  |  TX_EVENT_FLAG                    | 7       |
  |  TX_BLOCK_MEMORY                  | 8       |
  |  TX_BYTE_MEMORY                   | 9       |
  |  TX_TCP_IP                        | 12      |
  |  TX_MUTEX_SUSP                    | 13      |

- Info Field 3: Stack pointer value during the call. 
- Info Field 4: Pointer to next highest priority thread to execute.

### Internal thread suspend

#### Internal thread suspend

**Icon** ![Internal thread suspend icon](../media/user-guide/tx-events/image2.png)

**Description**

This event represents the internal processing in ThreadX that suspends a thread's execution. The next highest priority thread ready for execution is placed in the fourth information field. If this value is NULL, there is no other thread ready for execution and the system is idle.

**Information Fields**

- Info Field 1: Pointer to the thread being suspended.
- Info Field 2: New state of the thread being suspended, as follows:

  |  Thread state                     | Value        |
  |---------------------------------- | --------|
  |  TX_COMPLETED                     | 1       |
  |  TX_TERMINATED                    | 2       |
  |  TX_SUSPENDED                     | 3       |
  |  TX_SLEEP                         | 4       |
  |  TX_QUEUE_SUSP                    | 5       |
  |  TX_SEMAPHORE_SUSP                | 6       |
  |  TX_EVENT_FLAG                    | 7       |
  |  TX_BLOCK_MEMORY                  | 8       |
  |  TX_BYTE_MEMORY                   | 9       |
  |  TX_TCP_IP                        | 12      |
  |  TX_MUTEX_SUSP                    | 13      |

- Info Field 3: Stack pointer value during the call. Info Field 4: Pointer to next highest priority thread to execute. If NULL, the system is idle.

### Interrupt Service Routine (ISR) enter 

#### Enter ISR 

**Icon** ![Enter I S R icon](../media/user-guide/tx-events/image3.png)

**Description**

This event represents entering an Interrupt Service Routine (ISR) in the application. The interrupt service routine execution continues until the ISR exit event takes place.

**Information Fields**

- Info Field 1: Stack pointer value during the call.
- Info Field 2: Application-defined ISR number (optional).
- Info Field 3: Nested interrupt count.
- Info Field 4: Internal preemption disable flag.

### Interrupt Service Routine (ISR) exit 

#### Exit ISR

**Icon** ![Exit I S R icon](../media/user-guide/tx-events/image4.png)

**Description**

This event represents exiting an Interrupt Service Routine (ISR) in the application.

**Information Fields**

- Info Field 1: Stack pointer value during the call.
- Info Field 2: Application-defined ISR number (optional).
- Info Field 3: Nested interrupt count.
- Info Field 4: Internal preemption disable flag.

### Internal time-slice

#### Internal time-slice

**Icon** ![Internal time-slice icon](../media/user-guide/tx-events/image5.png)

**Description**

This event represents the internal processing in ThreadX that performs the time-slice operation. The next thread of the same priority is placed in the first information field. If this value is the same as the current thread, no time-slice was performed.

- Info Field 1: Pointer to the next thread to execute.
- Info Field 2: Nested interrupt count.
- Info Field 3: Internal preemption disable flag.
- Info Field 4: Stack pointer value during the call.

### Running

#### Running in context

**Icon** ![Running icon](../media/user-guide/tx-events/image6.png)

**Description**

This event represents running within a thread context or idle system. It is used to illustrate subsequent changes in context as a result of an interrupt.

**Information Fields**
- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Block Allocate 

#### tx_block_allocate

**Icon** ![Block allocate icon](../media/user-guide/tx-events/image7.png)

**Description**

This event represents allocating a memory block via tx_block_allocate. If successful, the address of the block allocated is returned in the second information field.

**Information Fields**
- Info Field 1: Pointer to the corresponding block pool.
- Info Field 2: Pointer to the memory block returned (if successful).
- Info Field 3: The wait option supplied to the tx_block_allocate call.
- Info Field 4: Remaining available blocks in the pool after this allocation.

### Block Pool Create

#### tx_block_pool_create

**Icon** ![Block pool create icon](../media/user-guide/tx-events/image8.png)

**Description**

This event represents creating a memory block pool via tx_block_pool_create.

**Information Fields**

- Info Field 1: Pointer to the corresponding block pool control block.
- Info Field 2: Pointer to the starting memory area of the pool.
- Info Field 3: The number of blocks in the pool. Info Field 4: The size of each block in the pool in bytes.

### Block Pool Delete

#### tx_block_pool_delete

**Icon** ![Block pool delete icon](../media/user-guide/tx-events/image9.png)

**Description**

This event represents deleting a memory block pool via tx_block_pool_delete.

**Information Fields**

- Info Field 1: Pointer to the block pool control block.
- Info Field 2: Stack pointer value during the call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Block Pool Information Get 

#### tx_block_pool_info_get

**Icon** ![Block pool information get icon](../media/user-guide/tx-events/image10.png)

**Description**

This event represents getting information about a memory block pool via tx_block_pool_info_get.

**Information Fields**

- Info Field 1: Pointer to the block pool control block.
- Info Field 2: Stack pointer value during the call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Block Pool Performance Information Get

#### tx_block_pool_performance_info_get

**Icon** ![Block pool performance information get icon](../media/user-guide/tx-events/image11.png)

**Description**

This event represents getting performance information about a memory block pool via tx_block_pool_performance_info_get.

**Information Fields**

- Info Field 1: Pointer to the block pool control block.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Block Pool Performance System Information Get

#### tx_block_pool_performance_system_info_get

**Icon** ![Block pool performance system information get icon](../media/user-guide/tx-events/image12.png)

**Description**

This event represents getting performance information about all memory block pools via tx_block_pool_performance_system_info_get.

**Information Fields**
- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Block Pool Prioritize

#### tx_block_pool_prioritize

**Icon** ![Block pool prioritize icon](../media/user-guide/tx-events/image13.png)

**Description**

This event represents placing the highest priority suspended thread at the front of the block pool suspension list. If this is done prior to calling tx_block_release, the highest priority suspended thread will receive the released block.

**Information Fields**
- Info Field 1: Memory block pool pointer.
- Info Field 2: Number of threads suspended on this block pool.
- Info Field 3: Stack pointer at the time of the call.
- Info Field 4: Not used.

### Block Release 

#### tx_block_release

**Icon** ![Block release icon](../media/user-guide/tx-events/image14.png)

**Description**

This event represents releasing a previously allocated block back to the block pool.

**Information Fields**

- Info Field 1: Memory block pool pointer.
- Info Field 2: Pointer to block to release.
- Info Field 3: Number of threads suspended on this block pool.
- Info Field 4: Stack pointer at the time of the call.

### Byte Allocate 

#### tx_byte_allocate

**Icon** ![Byte allocate icon](../media/user-guide/tx-events/image15.png)

**Description**

This event represents allocating memory via tx_byte_allocate. If successful, the address of the memory allocated is returned in the second information field.

**Information Fields**
- Info Field 1: Pointer to the corresponding byte pool.
- Info Field 2: Pointer to the memory returned (if successful).
- Info Field 3: Number of bytes requested. Info Field 4: The wait option supplied to the tx_byte_allocate call.

### Byte Pool Create

#### tx_byte_pool_create

**Icon** ![Byte pool create icon](../media/user-guide/tx-events/image16.png)

**Description**

This event represents creating a byte pool via tx_byte_pool_create.

**Information Fields**

- Info Field 1: Pointer to the corresponding byte pool.
- Info Field 2: Pointer to the start of the memory area. Info Field 3: Number of bytes in the byte pool.
- Info Field 4: The stack pointer at the time of the call.

### Byte Pool Delete 

#### tx_byte_pool_delete

**Icon** ![Byte pool delete icon](../media/user-guide/tx-events/image17.png)

**Description**

This event represents deleting a byte pool via tx_byte_pool_delete.

**Information Fields**

- Info Field 1: Pointer to the corresponding byte pool.
- Info Field 2: The stack pointer at the time of the call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Byte Pool Information Get

#### tx_byte_pool_info_get

**Icon** ![Byte pool information get icon](../media/user-guide/tx-events/image18.png)

**Description**

This event represents getting byte pool information via tx_byte_pool_info_get.

**Information Fields**

- Info Field 1: Pointer to the corresponding byte pool.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Byte Pool Performance Info Get 

#### tx_byte_pool_info_get

**Icon** ![Byte pool performance info get icon](../media/user-guide/tx-events/image19.png)

**Description**

This event represents getting byte pool performance information via tx_byte_pool_performance_info_get.

**Information Fields**

- Info Field 1: Pointer to the corresponding byte pool.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Byte Pool Performance System Info Get 

#### tx_byte_pool_performance_system_info_get

**Icon** ![Byte pool performance system info get icon](../media/user-guide/tx-events/image20.png)

**Description**

This event represents getting byte pool performance system information via tx_byte_pool_performance_system_info_get.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Byte Pool Prioritize

#### tx_byte_pool_prioritize

**Icon** ![Byte pool prioritize icon](../media/user-guide/tx-events/image21.png)

**Description**

This event represents prioritizing the byte pool's suspension list via tx_byte_pool_prioritize.

**Information Fields**

- Info Field 1: Pointer to corresponding byte pool.
- Info Field 2: Number of threads currently suspended on byte pool.
- Info Field 3: Stack pointer at time of call.
- Info Field 4: Not used.

### Byte Release

#### tx_byte_release

**Icon** ![Byte release icon](../media/user-guide/tx-events/image22.png)

**Description**

This event represents releasing a block of memory allocated from a byte pool via tx_byte_release.

**Information Fields**

- Info Field 1: Pointer to corresponding byte pool.
- Info Field 2: Pointer to previously allocated byte pool memory.
- Info Field 3: Number of threads suspended on this byte pool.
- Info Field 4: Number of available bytes of memory.

### Event Flags Create 

#### tx_event_flags_create

**Icon** ![Event flags create icon](../media/user-guide/tx-events/image23.png)

**Description**

This event represents creating a new event flags group via tx_event_flags_create.

**Information Fields** 
- Info Field 1: Pointer to event flags group control block.
- Info Field 2: Stack pointer at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Event Flags Delete 

#### tx_event_flags_delete

**Icon** ![Event flags delete icon](../media/user-guide/tx-events/image24.png)

**Description**

This event represents deleting an event flags group via tx_event_flags_delete.

**Information Fields** 

- Info Field 1: Pointer to event flags group.
- Info Field 2: Stack pointer at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Event Flags Get 

#### tx_event_flags_get

**Icon** ![Event flags gt icon](../media/user-guide/tx-events/image25.png)

**Description**

This event represents retrieving event flags from an existing event flags group via tx_event_flags_get.

**Information Fields**

- Info Field 1: Pointer to event flags group.
- Info Field 2: Event flags requested.
- Info Field 3: Event flags currently set in the group.
- Info Field 4: Option requested on the event flags get.

### Event Flags Information Get

#### tx_event_flags_info_get

**Icon** ![Event flags information get icon](../media/user-guide/tx-events/image26.png)

**Description**

This event represents retrieving information regarding an existing event flags group via tx_event_flags_info_get.

**Information Fields**

- Info Field 1: Pointer to event flags group.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Event Flags Performance Information Get 

#### tx_event_flags_performance_info_get

**Icon** ![Event flags performance information get icon](../media/user-guide/tx-events/image27.png)

**Description**

This event represents retrieving performance information regarding an existing event flags group via tx_event_flags_performance_info_get.

**Information Fields** 
- Info Field 1: Pointer to event flags group.
- Info Field 2: Not used
- Info Field 3: Not used
- Info Field 4: Not Used

### Event Flags Performance System Info Get

#### tx_event_flags_performance_system_info_get

**Icon** ![Event flags performance system info get icon](../media/user-guide/tx-events/image28.png)

**Description**

This event represents retrieving performance information regarding an existing event flags group via tx_event_flags_performance_system_info_get.

**Information Fields**
- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Event Flags Set 

#### tx_event_flags_set

**Icon** ![Event flags set icon](../media/user-guide/tx-events/image29.png)

**Description**

This event represents setting (or clearing) event flags in an existing event flags group via tx_event_flags_set.

**Information Fields**

- Info Field 1: Pointer to event flags group.
- Info Field 2: Event flags to set (or clear).
- Info Field 3: AND or OR event flag option.
- Info Field 4: Number of threads suspended on event flag group.

### Event Flags Set Notify

#### tx_event_flags_set_notify

**Icon** ![Event flags set notify icon](../media/user-guide/tx-events/image30.png)

**Description**

This event represents registering a notification callback for any event flag set operation on an existing event flags group via tx_event_flags_set_notify.

**Information Fields**

- Info Field 1: Pointer to event flags group.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Interrupt Control 

#### tx_interrupt_control

**Icon** ![Interrupt control icon](../media/user-guide/tx-events/image31.png)

**Description**

This event represents changing the interrupt lockout posture of the processor via tx_interrupt_control.

**Information Fields**

- Info Field 1: New interrupt posture.
- Info Field 2: Stack pointer at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Mutex Create 

#### tx_mutex_create

**Icon** ![Mutex create icon](../media/user-guide/tx-events/image32.png)

**Description**

This event represents creating a mutex via tx_mutex_create.

**Information Fields**

- Info Field 1: Pointer to mutex control block.
- Info Field 2: Priority inheritance option
- (TX_INHERIT or TX_NO_INHERIT).
- Info Field 3: Stack pointer at time of call.
- Info Field 4: Not used.

### Mutex Delete 

#### tx_mutex_delete

**Icon** ![Mutex delete icon](../media/user-guide/tx-events/image33.png)

**Description**

This event represents deleting a mutex via tx_mutex_delete.

**Information Fields** 

- Info Field 1: Pointer to mutex.
- Info Field 2: Stack pointer at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Mutex Get 

#### tx_mutex_get

**Icon** ![Mutex get icon](../media/user-guide/tx-events/image34.png)

**Description**

This event represents obtaining a mutex via tx_mutex_get.

**Information Fields**

- Info Field 1: Pointer to mutex.
- Info Field 2: The wait option supplied to the tx_mutex_get call.
- Info Field 3: Pointer to thread that owns the mutex (NULL implies the mutex is not owned).
- Info Field 4: Number of times the owning thread has called tx_mutex_get.

### Mutex Information Get

#### tx_mutex_info_get

**Icon** ![Mutex information get icon](../media/user-guide/tx-events/image35.png)

**Description**

This event represents retrieving mutex information via tx_mutex_info_get.

**Information Fields**

- Info Field 1: Pointer to mutex.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Mutex Performance Information Get 

#### tx_mutex_performance_info_get

**Icon** ![Mutex performance information get icon](../media/user-guide/tx-events/image36.png)

**Description**

This event represents retrieving mutex performance information via tx_mutex_performance_info_get.

**Information Fields**

- Info Field 1: Pointer to mutex.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Mutex Performance System Info Get

#### tx_mutex_performance_system_info_get

**Icon** ![Mutex performance system info get icon](../media/user-guide/tx-events/image37.png)

**Description**

This event represents retrieving mutex system performance information via tx_mutex_performance_system_info_get.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Mutex Prioritize 

#### tx_mutex_prioritize

**Icon** ![Mutex prioritize icon](../media/user-guide/tx-events/image38.png)

**Description**

This event represents prioritizing the mutex's suspension list via tx_mutex_prioritize.

**Information Fields**

- Info Field 1: Pointer to corresponding mutex.
- Info Field 2: Number of threads currently suspended on the mutex.
- Info Field 3: Stack pointer at time of call.
- Info Field 4: Not used.

### Mutex Put 

#### tx_mutex_put

**Icon** ![Mutex put icon](../media/user-guide/tx-events/image39.png)

**Description**

This event represents releasing a previously owned mutex via tx_mutex_put.

**Information Fields**

- Info Field 1: Pointer to corresponding mutex.
- Info Field 2: Pointer of thread owning the mutex.
- Info Field 3: Number of outstanding mutex get requests.
- Info Field 4: Stack pointer at time of call.

### Queue Create 

#### tx_queue_create

**Icon** ![Queue create icon](../media/user-guide/tx-events/image40.png)

**Description**

This event represents creating a message queue via tx_queue_create.

**Information Fields**

- Info Field 1: Pointer to queue control block.
- Info Field 2: Size of message – in terms of 32-bit words.
- Info Field 3: Pointer to start of queue memory area.
- Info Field 4: Number of bytes in the queue memory area.

### Queue Delete 

#### tx_queue_delete

**Icon** ![Queue delete icon](../media/user-guide/tx-events/image41.png)

**Description**

This event represents deleting a queue via tx_queue_delete.

**Information Fields**

- Info Field 1: Pointer to queue.
- Info Field 2: Stack pointer at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Queue Flush 

#### tx_queue_flush

**Icon** ![Queue flush icon](../media/user-guide/tx-events/image42.png)

**Description**

This event represents flushing (clearing all queue contents) of a queue via tx_queue_flush.

**Information Fields**

- Info Field 1: Pointer to queue.
- Info Field 2: Stack pointer at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Queue Front Send 

#### tx_queue_front_send

**Icon** ![Queue front send icon](../media/user-guide/tx-events/image43.png)

**Description**

This event represents sending a message to the front of a queue via tx_queue_front_send.

**Information Fields**

- Info Field 1: Pointer to queue.
- Info Field 2: Pointer to start of message.
- Info Field 3: Wait option supplied to the
- tx_queue_front_send call.
- Info Field 4: Number of messages already enqueued.

### Queue Information Get 

#### tx_queue_info_get

**Icon** ![Queue information get icon](../media/user-guide/tx-events/image44.png)

**Description**

This event represents getting information about a queue via tx_queue_info_get.

**Information Fields** 
- Info Field 1: Pointer to queue.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Queue Performance Info Get 

#### tx_queue_performance_info_get

**Icon** ![Queue performance info get icon](../media/user-guide/tx-events/image45.png)

**Description**

This event represents getting performance information about a queue via tx_queue_performance_info_get.

**Information Fields** 

- Info Field 1: Pointer to queue.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Queue Performance System Info Get 

#### tx_queue_performance_system_info_get

**Icon** ![Queue performance system info get icon](../media/user-guide/tx-events/image46.png)

**Description**

This event represents getting system performance information about all the queues in the system.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Queue Prioritize 

#### tx_queue_prioritize

**Icon** ![Queue prioritize icon](../media/user-guide/tx-events/image47.png)

**Description**

This event represents prioritizing the queue's suspension list via tx_queue_prioritize.

**Information Fields** 

- Info Field 1: Pointer to corresponding queue.
- Info Field 2: Number of threads currently suspended on the queue.
- Info Field 3: Stack pointer at time of call.
- Info Field 4: Not used.

#### Queue Receive 

##### tx_queue_receive

**Icon** ![Queue receive icon](../media/user-guide/tx-events/image48.png)

**Description**

This event represents receiving a message from a queue via tx_queue_receive.

**Information Fields** 

- Info Field 1: Pointer to queue.
- Info Field 2: Pointer to destination for message. Info Field 3: Wait option supplied to the call.
- Info Field 4: Number of messages currently queued.

### Queue Send 

#### tx_queue_send

**Icon** ![Queue send icon](../media/user-guide/tx-events/image49.png)

**Description**

This event represents sending a message to a queue via tx_queue_send.

**Information Fields** 

- Info Field 1: Pointer to queue.
- Info Field 2: Pointer to message.
- Info Field 3: Wait option supplied to the call.
- Info Field 4: Number of messages currently queued.

### Queue Send Notify 

#### tx_queue_send_notify

**Icon** ![Queue send notify icon](../media/user-guide/tx-events/image50.png)

**Description**

<p>This event represents registering a callback via tx_queue_send_notify which is called whenever a message is sent to a queue.

**Information Fields** 

- Info Field 1: Pointer to queue.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Semaphore Ceiling Put 

#### tx_semaphore_ceiling_put

**Icon** ![Semaphore ceiling put icon](../media/user-guide/tx-events/image51.png)

**Description**

This event represents putting to a semaphore via tx_semaphore_ceiling_put. This differs from tx_semaphore_put in that the maximum value of the semaphore is examined such that the put operation is not allowed to exceed the maximum value or ceiling.

**Information Fields**

- Info Field 1: Pointer to semaphore.
- Info Field 2: Current semaphore count.
- Info Field 3: Number of threads suspended on the semaphore.
- Info Field 4: Ceiling limit supplied to the call.

#### Semaphore Create 

##### tx_semaphore_create

**Icon** ![Semaphore create icon](../media/user-guide/tx-events/image52.png)

**Description**

This event represents creating a semaphore via tx_semaphore_create.

**Information Fields**

- Info Field 1: Pointer to semaphore control block.
- Info Field 2: Initial semaphore count.
- Info Field 3: Stack pointer at time of call.
- Info Field 4: Not used.

### Semaphore Delete 

#### tx_semaphore_delete

**Icon** ![Semaphore delete icon](../media/user-guide/tx-events/image53.png)

**Description**

This event represents deleting a semaphore via tx_semaphore_delete.

**Information Fields**

- Info Field 1: Pointer to semaphore.
- nfo Field 2: Stack pointer at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Semaphore Get 

#### tx_semaphore_get

**Icon** ![Semaphore get icon](../media/user-guide/tx-events/image54.png)

**Description**

This event represents obtaining a semaphore via tx_semaphore_get.

**Information Fields**

- Info Field 1: Pointer to semaphore.
- Info Field 2: Wait option supplied to the call.
- Info Field 3: Current semaphore count.
- Info Field 4: Stack pointer at time of call.

### Semaphore Information Get 

#### tx_semaphore_info_get

**Icon** ![Semaphore information get icon](../media/user-guide/tx-events/image55.png)

**Description**

This event represents obtaining information about a semaphore via tx_semaphore_info_get.

**Information Fields**

- Info Field 1: Pointer to semaphore.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Semaphore Performance Info Get 

#### tx_semaphore_performance_info_get

**Icon** ![Semaphore performance info get icon](../media/user-guide/tx-events/image56.png)

**Description**

This event represents obtaining performance information about a semaphore via tx_semaphore_performance_info_get.

**Information Fields**

- Info Field 1: Pointer to semaphore.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Semaphore Performance System Info 

#### tx_semaphore_performance_system_info_get

**Icon** ![Semaphore performance system info icon](../media/user-guide/tx-events/image57.png)

**Description**

This event represents obtaining performance information about all semaphores in the system via tx_semaphore_performance_system_info_get.

**Information Fields** 

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Semaphore Prioritize 

#### tx_semaphore_prioritize

**Icon** ![Semaphore prioritize icon](../media/user-guide/tx-events/image58.png)

**Description**

This event represents prioritizing the semaphore's suspension list via tx_semaphore_prioritize.

**Information Fields**

- Info Field 1: Pointer to corresponding semaphore.
- Info Field 2: Number of threads currently suspended on the semaphore.
- Info Field 3: Stack pointer at time of call.
- Info Field 4: Not used.

### Semaphore Put 

#### tx_semaphore_put

**Icon** ![Semaphore put icon](../media/user-guide/tx-events/image59.png)

**Description**

This event represents releasing a semaphore instance via tx_semaphore_put.

**Information Fields** 

- Info Field 1: Pointer to corresponding semaphore. Info Field 2: Current semaphore count.
- Info Field 3: Number of threads suspended on the semaphore.
- Info Field 4: Stack pointer at time of call.

### Semaphore Put Notify 

#### tx_semaphore_put_notify

**Icon** ![Semaphore put notify icon](../media/user-guide/tx-events/image60.png)

**Description**

This event represents registering a callback via tx_semaphore_put_notify that is called whenever a semaphore instance is put.

**Information Fields** 

- Info Field 1: Pointer to semaphore.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Thread Create 

#### tx_thread_create

**Icon** ![Thread create icon](../media/user-guide/tx-events/image61.png)

**Description**

This event represents creating a thread via tx_thread_create.

**Information Fields**

- Info Field 1: Pointer to thread control block.
- Info Field 2: Priority of thread.
- Info Field 3: Stack pointer for thread.
- nfo Field 4: Size of stack in bytes.

### Thread Delete 

#### tx_thread_delete

**Icon** ![Thread delete icon](../media/user-guide/tx-events/image62.png)

**Description**

This event represents deleting a thread via tx_thread_delete.

**Information Fields** 

- Info Field 1: Pointer to thread.
- Info Field 2: Stack pointer at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Thread Entry/Exit Notify 

#### tx_thread_entry_exit_notify

**Icon** ![Thread entry/exit notify icon](../media/user-guide/tx-events/image63.png)

**Description**

This event represents registering a callback via tx_thread_entry_exit_notify that is called whenever a thread is entered or exits.

**Information Fields** 

- Info Field 1: Pointer to thread.
- Info Field 2: Thread state at time of the registration.
- Info Field 3: Pointer to stack at time of call.
- Info Field 4: Not used.

#### Thread Identify 

##### tx_thread_identify

**Icon** ![Thread identify icon](../media/user-guide/tx-events/image64.png)

**Description**

This event represents getting the current thread pointer via tx_thread_identify.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Thread Information Get 

#### tx_thread_info_get

**Icon** ![Thread information get icon](../media/user-guide/tx-events/image65.png)

**Description**

This event represents getting information about the specified thread via tx_thread_info_get.

**Information Fields**

- Info Field 1: Pointer to thread.
- Info Field 2: State of thread at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

#### Thread Performance Information Get 

##### tx_thread_performance_info_get

**Icon** ![Thread performance information get icon](../media/user-guide/tx-events/image66.png)

**Description**

This event represents getting performance information about the specified thread via tx_thread_performance_info_get.

**Information Fields**

- Info Field 1: Pointer to thread.
- Info Field 2: State of thread at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Thread Performance System Info Get 

#### tx_thread_performance_system_info_get

**Icon** ![Thread performance system info get icon](../media/user-guide/tx-events/image67.png)

**Description**

This event represents getting performance information about all threads via tx_thread_performance_system_info_get.

**Information Fields** 

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Thread Preemption Change 

#### tx_thread_preemption_change

**Icon** ![Thread preemption change icon](../media/user-guide/tx-events/image68.png)

**Description**

This event represents changing a thread's preemption-threshold via tx_thread_preemption_change.

**Information Fields** 

- Info Field 1: Pointer to thread.
- Info Field 2: New preemption-threshold.
- Info Field 3: Previous preemption-threshold.
- Info Field 4: Thread's state at time of call.

### Thread Priority Change 

#### tx_thread_priority_change

**Icon** ![Thread priority change icon](../media/user-guide/tx-events/image69.png)

**Description**

This event represents changing a thread's priority via tx_thread_priority_change.

- Information Fields 
- Info Field 1: Pointer to thread.
- Info Field 2: New priority.
- Info Field 3: Previous priority.
- Info Field 4: Thread's state at time of call.

### Thread Relinquish 

#### tx_thread_relinquish

**Icon** ![Thread relinquish icon](../media/user-guide/tx-events/image70.png)

**Description**

This event represents relinquishing the processor from a thread via tx_thread_relinquish.

**Information Fields**

- Info Field 1: Stack pointer at time of call.
- Info Field 2: Pointer to the next thread to execute.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Thread Reset 

#### tx_thread_reset

**Icon** ![Thread reset icon](../media/user-guide/tx-events/image71.png)

**Description**

This event represents resetting a completed or terminated thread via tx_thread_reset.

**Information Fields**

- Info Field 1: Pointer to thread.
- Info Field 2: Thread's state at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

#### Thread Resume 

##### tx_thread_resume

**Icon** ![Thread resume icon](../media/user-guide/tx-events/image72.png)

**Description**

This event represents resuming a suspended thread via tx_thread_resume.

**Information Fields**

- Info Field 1: Pointer to thread.
- Info Field 2: Thread's state at time of call.
- Info Field 3: Stack pointer at time of call.
- Info Field 4: Not used.

### Thread Sleep 

#### tx_thread_sleep

**Icon** ![Thread sleep icon](../media/user-guide/tx-events/image73.png)

**Description**

This event represents suspending the current thread for a specified number of timer ticks via tx_thread_sleep.

**Information Fields**

- Info Field 1: Number of ticks to suspend for.
- Info Field 2: Thread's state at time of call.
- Info Field 3: Stack pointer at time of call.
- Info Field 4: Not used.

### Thread Stack Error Notify 

#### tx_thread_stack_error_notify_event

**Icon** ![Thread stack error notify icon](../media/user-guide/tx-events/image74.png)

**Description**

This event represents registering a thread stack error notification routine via tx_thread_stack_error_notify_event.

**Information Fields** 

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Thread Suspend 

#### tx_thread_suspend

**Icon** ![Thread suspend icon](../media/user-guide/tx-events/image75.png)

**Description**

This event represents suspending a thread via tx_thread_suspend.

**Information Fields** 

- Info Field 1: Pointer to thread to suspend.
- Info Field 2: Thread's state at time of call.
- Info Field 3: Stack pointer at time of call.
- Info Field 4: Not used.

### Thread Terminate 

#### tx_thread_terminate

**Icon** ![Thread terminate icon](../media/user-guide/tx-events/image76.png)

**Description**

This event represents terminating a thread via tx_thread_terminate.

**Information Fields** 

- Info Field 1: Pointer to thread to terminate.
- Info Field 2: Thread's state at time of call.
- Info Field 3: Stack pointer at time of call.
- Info Field 4: Not used.

### Thread Time-Slice Change 

#### tx_thread_time_slice_change

**Icon** ![Thread time-slice change icon](../media/user-guide/tx-events/image77.png)

**Description**

This event represents changing a thread's time-slice via tx_thread_time_slice_change.

**Information Fields**

- Info Field 1: Pointer to thread.
- Info Field 2: New time-slice.
- Info Field 3: Previous time-slice.
- Info Field 4: Not used.

### Thread Wait Abort 

#### tx_thread_wait_abort

**Icon** ![Thread wait abort icon](../media/user-guide/tx-events/image78.png)

**Description**

This event represents aborting a thread's suspension via tx_thread_wait_abort.

**Information Fields**

- Info Field 1: Pointer to thread.
- Info Field 2: Thread's state at time of call.
- Info Field 3: Stack pointer at time of call.
- Info Field 4: Not used.

### Time Get 

#### tx_time_get

**Icon** ![Time get icon](../media/user-guide/tx-events/image79.png)

**Description**

This event represents getting the current number of timer ticks via tx_time_get.

**Information Fields**

- Info Field 1: Current number of timer ticks.
- Info Field 2: Stack pointer at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Time Set 

#### tx_time_set

**Icon** ![Time set icon](../media/user-guide/tx-events/image80.png)

**Description**

This event represents setting the current number of timer ticks via tx_time_set.

**Information Fields**

- Info Field 1: New number of timer ticks.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Timer Activate 

#### tx_timer_activate

**Icon** ![Timer activate icon](../media/user-guide/tx-events/image81.png)

**Description**

This event represents activating the specified timer via tx_timer_activate.

**Information Fields**

- Info Field 1: Pointer to timer.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Timer Change 

#### tx_timer_change

**Icon** ![Timer change icon](../media/user-guide/tx-events/image82.png)

**Description**

This event represents changing the specified timer via tx_timer_change.

**Information Fields**

- Info Field 1: Pointer to timer.
- Info Field 2: Initial expiration ticks.
- Info Field 3: Reschedule expiration ticks.
- Info Field 4: Not used.

### Timer Create 

#### tx_timer_create

**Icon** ![Timer create icon](../media/user-guide/tx-events/image83.png)

**Description**

This event represents creating a timer via tx_timer_create.

**Information Fields**

- Info Field 1: Pointer to timer control block.
- Info Field 2: Initial expiration ticks.
- Info Field 3: Reschedule expiration ticks.
- Info Field 4: Automatic enable value—either TX_AUTO_ACTIVATE (1) or TX_NO_ACTIVATE (0).

### Timer Deactivate 

#### tx_timer_deactivate

**Icon** ![Timer deactivate icon](../media/user-guide/tx-events/image84.png)

**Description**

This event represents deactivating a timer via tx_timer_deactivate.

**Information Fields**

- Info Field 1: Pointer to timer.
- Info Field 2: Stack pointer at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Timer Delete 

#### tx_timer_delete

**Icon** ![Timer delete icon](../media/user-guide/tx-events/image85.png)

**Description**

This event represents deleting a timer via tx_timer_delete.

**Information Fields** 

- Info Field 1: Pointer to timer.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Timer Information Get 

#### tx_timer_info_get

**Icon** ![Timer get information icon](../media/user-guide/tx-events/image86.png)

**Description**

This event represents getting timer information via tx_timer_info_get.

**Information Fields**

- Info Field 1: Pointer to timer.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Timer Performance Information Get 

#### tx_timer_performance_info_get

**Icon** ![Timer performance information get icon](../media/user-guide/tx-events/image87.png)

**Description** 

This event represents getting timer performance information via tx_timer_performance_info_get.

**Information Fields**

- Info Field 1: Pointer to timer.
- Info Field 2: Stack pointer at time of call.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Timer System Performance Info Get 

#### tx_timer_performance_system_info_get

**Icon** ![Timer system performance info get icon](../media/user-guide/tx-events/image88.png)


**Description** 

This event represents getting all timer performance information via tx_timer_performance_system_info_get.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.