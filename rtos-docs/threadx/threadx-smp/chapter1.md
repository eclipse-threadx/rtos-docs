---
title: Chapter 1 - Introduction to ThreadX SMP
description: This chapter contains an introduction to the product and a description of its applications and benefits.
---

# Chapter 1: Introduction to ThreadX SMP

ThreadX SMP is a high-performance real-time SMP kernel designed specifically for embedded applications. This chapter contains an introduction to the product and a description of its applications and benefits.

## ThreadX SMP Unique Features

ThreadX SMP brings Symmetric Multi-Processing (SMP) technology to embedded applications. ThreadX SMP application threads (of varying priority) that are "READY" to run are dynamically allocated to available processor cores during scheduling. This results in true SMP processing, including automatic load balancing of application thread execution across all available processor cores.

Unlike other real-time kernels, ThreadX SMP is designed to be versatile—easily scaling among small micro-controller-based applications through those that use powerful CISC, RISC, and DSP processors.

ThreadX SMP is scalable based on its underlying architecture. Because ThreadX SMP services are implemented as a C library, only those services actually used by the application are brought into the run-time image. Hence, the actual size of ThreadX SMP is completely determined by the application. For most applications, the instruction image of ThreadX SMP ranges between 5 KBytes and 20 KBytes in size.

### Picokernel Architecture

Instead of layering kernel functions on top of each other like traditional _microkernel_ architectures, ThreadX SMP services plug directly into its core. This results in the fastest possible context switching and service call performance. We call this nonlayering design a _picokernel_ architecture.

### ANSI C Source Code

ThreadX SMP is written primarily in ANSI C. A small amount of assembly language is needed to tailor the kernel to the underlying target processor. This design makes it possible to port ThreadX SMP to a new processor family in a very short time—usually within weeks!

### Advanced Technology

The following are highlights of the ThreadX SMP advanced technology:

- Simple _picokernel_ architecture
- Automatic load balancing
- Per-thread processor exclusion
- Automatic scaling (small footprint)
- Deterministic processing
- Fast real-time performance
- Preemptive and cooperative scheduling
- Flexible thread priority support (32-1024)
- Dynamic system object creation
- Unlimited number of system objects
- Optimized interrupt handling
- Preemption-threshold
- Priority inheritance
- Event-chaining
- Fast software timers
- Run-time memory management
- Run-time performance monitoring
- Run-time stack analysis
- Built-in system trace
- Vast processor support
- Vast development tool support
- Completely endian neutral

### Not A Black Box

Most distributions of ThreadX SMP include the complete C source code as well as the processor specific assembly language. This eliminates the "black-box" problems that occur with many commercial kernels. With ThreadX SMP, application developers can see exactly what the kernel is doing—there are no mysteries!

The source code also allows for application specific modifications. Although not recommended, it is certainly beneficial to have the ability to modify the kernel if it is absolutely required.

These features are especially comforting to developers accustomed to working with their own _inhouse kernels_. They expect to have source code and the ability to modify the kernel. ThreadX SMP is the ultimate kernel for such developers.

### The RTOS Standard

Because of its versatility, high-performance _picokernel_ architecture, advanced technology, and demonstrated portability, ThreadX SMP is deployed in more than two-billion devices today. This effectively makes ThreadX SMP the RTOS standard for deeply embedded applications.

## Safety Certifications

### TÜV Certification

ThreadX SMP has been certified by SGS-TÜV Saar for use in safety-critical systems, according to IEC-61508 SIL 4. The certification confirms that ThreadX SMP can be used in the development of safety-related software for the highest safety integrity levels of IEC-61508 for the "Functional Safety of electrical, electronic, and programmable electronic safety-related systems." SGS-TUV Saar, formed through a joint venture of Germany's SGS-Group and TUV Saarland, has become the leading accredited, independent company for testing, auditing, verifying, and certifying embedded software for safety-related systems worldwide.

![TÜV Certification](media/image2.png)

- IEC 61508 up to SIL 4

### MISRA C Compliant

MISRA C is a set of programming guidelines for critical systems using the C programming language. The original MISRA C guidelines were primarily targeted toward automotive applications; however, MISRA C is now widely recognized as being applicable to any safety critical application. ThreadX SMP is compliant with all "required" and "mandatory" rules of MISRA-C:2004 and MISRA C:2012. ThreadX SMP is also compliant with all but three "advisory" rules. Refer to the **_ThreadX_MISRA_Compliance.pdf_** document for more details.

### UL Certification

ThreadX SMP has been certified by UL for compliance with UL 60730-1 Annex H, CSA E607301 Annex H, IEC 60730-1 Annex H, UL 60335-1 Annex R, IEC 60335-1 Annex R, and UL 1998 safety standards for software in programmable components. Along with IEC/UL 60730-1, which has requirements for "Controls Using Software" in its Annex H, the IEC 60335-1 standard describes the requirements for "Programmable Electronic Circuits" in its Annex R. IEC 60730 Annex H and IEC 60335-1 Annex R address the safety of MCU hardware and software used in appliances such as washing machines, dishwashers, dryers, refrigerators, freezers, and ovens.

![UL Certification](media/image3.png)

_UL/IEC 60730, UL/IEC 60335, UL 1998_

## Embedded Applications

Embedded applications execute on microprocessors buried within products such as wireless communication devices, automobile engines, laser printers, medical devices, etc. Another distinction of embedded applications is that their software and hardware have a dedicated purpose.

### Real-time Software

When time constraints are imposed on the application software, it is called the _real-time_ software. Basically, software that must perform its processing within an exact period of time is called _real-time_ software. Embedded applications are almost always real-time because of their inherent interaction with external events.

### Multitasking

As mentioned, embedded applications have a dedicated purpose. To fulfill this purpose, the software must perform a variety of _tasks_. A task is a semi-independent portion of the application that carries out a specific duty. It is also the case that some tasks are more important than others. One of the major difficulties in an embedded application is the allocation of the processor between the various application tasks. This allocation of processing between competing tasks is the primary purpose of ThreadX SMP.

### Tasks vs. Threads

Another distinction about tasks must be made. The term task is used in a variety of ways. It sometimes means a separately loadable program. In other instances, it may refer to an internal program segment.

In contemporary operating system discussion, there are two terms that more or less replace the use of task: _process_ and _thread_. A _process_ is a completely independent program that has its own address space, while a _thread_ is a semi-independent program segment that executes within a process. Threads share the same process address space. The overhead associated with thread management is minimal.

Most embedded applications cannot afford the overhead (both memory and performance) associated with a full-blown process-oriented operating system. In addition, smaller microprocessors don't have the hardware architecture to support a true process-oriented operating system. For these reasons, ThreadX SMP implements a thread model, which is both extremely efficient and practical for most real-time embedded applications.

To avoid confusion, ThreadX SMP does not use the term _task_. Instead, the more descriptive and contemporary name _thread_ is used.

## ThreadX SMP Benefits

Using ThreadX SMP provides many benefits to embedded applications. Of course, the primary benefit rests in how embedded application threads are allocated processing time.

### Automatic Load Balancing

ThreadX SMP provides automatic load balancing (thread execution across available cores), which makes utilizing multicore processors as easy as possible.

### Improved Responsiveness

Prior to real-time kernels like ThreadX SMP, most embedded applications allocated processing time with a simple control loop, usually from within the C _main_ function. This approach is still used in very small or simple applications. However, in large or complex applications, it is not practical because the response time to any event is a function of the worst case processing time of one pass through the control loop.

Making matters worse, the timing characteristics of the application change whenever modifications are made to the control loop. This makes the application inherently unstable and difficult to maintain and improve on.

ThreadX SMP provides fast and deterministic response times to important external events. ThreadX SMP accomplishes this through its preemptive, priority-based scheduling algorithm, which allows a higher-priority thread to preempt an executing lower-priority thread. As a result, the worst-case response time approaches the time required to perform a context switch. This is not only deterministic, but it is also extremely fast.

### Software Maintenance

The ThreadX SMP kernel enables application developers to concentrate on specific requirements of their application threads without having to worry about changing the timing of other areas of the application. This feature also makes it much easier to repair or enhance an application that utilizes ThreadX SMP.

### Increased Throughput

A possible work-around to the control loop response time problem is to add more polling. This improves the responsiveness, but it still doesn't guarantee a constant worst-case response time and does nothing to enhance future modification of the application. Also, the processor is now performing even more unnecessary processing because of the extra polling. All of this unnecessary processing reduces the overall throughput of the system.

An interesting point regarding overhead is that many developers assume that multithreaded environments like ThreadX SMP increase overhead and have a negative impact on total system throughput. But in some cases, multithreading actually reduces overhead by eliminating all of the redundant polling that occurs in control loop environments. The overhead associated with multithreaded kernels is typically a function of the time required for context switching. If the context switch time is less than the polling process, ThreadX SMP provides a solution with the potential of less overhead and more throughput. This makes ThreadX SMP an obvious choice for applications that have any degree of complexity or size.

### Processor Isolation

ThreadX SMP provides a robust processor independent interface between the application and the underlying processor. This allows developers to concentrate on the application rather than spending a significant amount of time learning hardware details.

### Dividing the Application

In control loop-based applications, each developer must have an intimate knowledge of the entire application's run-time behavior and requirements. This is because the processor allocation logic is dispersed throughout the entire application. As an application increases in size or complexity, it becomes impossible for all developers to remember the precise processing requirements of the entire application.

ThreadX SMP frees each developer from the worries associated with processor allocation and allows them to concentrate on their specific piece of the embedded application. In addition, ThreadX SMP forces the application to be divided into clearly defined threads. By itself, this division of the application into threads makes development much simpler.

### Ease of Use

ThreadX SMP is designed with the application developer in mind. The ThreadX SMP architecture and service call interface are designed to be easily understood. As a result, ThreadX SMP developers can quickly use its advanced features.

### Improve Time-to-market

All of the benefits of ThreadX SMP accelerate the software development process. ThreadX SMP takes care of most processor issues and the most common safety certifications, thereby removing this effort from the development schedule. All of this results in a faster time to market!

### Protecting the Software Investment

Because of its architecture, ThreadX SMP is easily ported to new processor and/or development tool environments. This, coupled with the fact that ThreadX SMP insulates applications from details of the underlying processors, makes ThreadX SMP applications highly portable. As a result, the application's migration path is guaranteed, and the original development investment is protected.
