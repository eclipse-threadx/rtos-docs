---
title: Chapter 1 - Introduction to TraceX
description: TraceX is a Eclipse Foundation system analysis tool that displays system event information gathered by ThreadX running on an embedded target. 
---


TraceX is a Eclipse Foundation system analysis tool that displays system event information gathered by ThreadX running on an embedded target. The
user is responsible for transferring the trace buffer stored in RAM in the embedded target to a binary file on the host computer. The user
can then open this file with TraceX and graphically analyze the target events, diagnosing system problems and tuning a working application to improve performance and resource management.

## TraceX Requirements

TraceX requires Windows XP (or above). The system should have a minimum of 192 MB of RAM, 2 GB of available hard-disk space, and a minimum display of 1024x768 with 256 colors. In addition, the application must be running on ThreadX V5.0 or later.

TraceX also requires the .NET framework be installed, which the TraceX installer does automatically.

## TraceX Constraints

TraceX has the following constraints:

- TraceX files are limited to a maximum of 32,768 events (roughly 1 MB ).
- The time-stamp source must have reasonable resolution. If the resolution is too low, the events will overlap. If the resolution is too high, there is potential for long gaps between events.
- TraceX cannot accurately measure intervals between events greater than the timer period.
