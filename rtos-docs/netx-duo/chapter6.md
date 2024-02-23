---
title: Chapter 6 - TSN components of NetX Duo
description: This chapter contains an introduction to TSN components and a description of its applications and benefits.
---

# Chapter 6 - TSN Components of NetX Duo

This chapter contains an introduction to TSN components and a description of its applications and benefits.

## Protocol framework
Time-Sensitive Networking (TSN) is a set of standards developed by the IEEE 802.1 working group. These standards define the mechanisms for transmitting time-sensitive data effectively over Ethernet networks.

In this chapter, the TSN compents in below frame work in colour blue are described.
![TSN Framework](./media/user-guide/tsn-framework.png)

## Link layer



## Credit-based shaper (CBS)
### Introduction
Credit-based shaper (CBS) is a traffic shaping mechanism that is in audio video bridge(AVB) network, to ensure/control the bandwidth of specific audio and video traffic streams. this mechanism can ensure that the data is transmitted at a constant rate and to avoid congestion in the network.

### APIs
#### API to set the CBS parameters
```c
UINT nx_shaper_cbs_parameter_set(NX_INTERFACE *interface_ptr, NX_SHAPER_CBS_PARAMETER *cbs_parameter, UCHAR pcp);
```
This API sets the CBS parameters for the specified interface and priority. The CBS parameters include the maximum burst size, the time interval, and the credit limit.

#### API to get the CBS parameters
```c
UINT nx_shaper_cbs_parameter_get(NX_INTERFACE *interface_ptr, NX_SHAPER_CBS_PARAMETER *cbs_parameter, UCHAR pcp);
```
This API gets the CBS parameters for the specified interface and priority.


## IEEE 802.1Qbv Enhancements to Traffic Scheduling: Time-Aware Shaper (TAS)
The IEEE 802.1Qbv time-aware scheduler is designed to separate the communication on the Ethernet network into fixed length, repeating time cycles. Within these cycles, different time slices can be configured that can be assigned to one or several of the eight Ethernet priorities. By doing this, it is possible to grant exclusive use - for a limited time - to the Ethernet transmission medium for those traffic classes that need transmission guarantees and can't be interrupted. The basic concept is a time-division multiple access (TDMA) scheme. By establishing virtual communication channels for specific time periods, time-critical communication can be separated from non-critical background traffic.

## Frame preemption (FPE)
Frame preemption defines two MAC services for an egress port, preemptable MAC (pMAC) and express MAC (eMAC). Express frames can interrupt transmission of preemptable frames. On resume, MAC merge sublayer re-assembles frame fragments in the next bridge.

## Time synchronization(gPTP)
IEEE 802.1AS-2011 defines the Generalized Precision Time Protocol (gPTP) profile which, like all profiles of IEEE 1588, selects among the options of 1588, but also generalizes the architecture to allow PTP to apply beyond wired Ethernet networks.

To account for data path delays, the gPTP protocol measures the frame residence time within each bridge (the time required for receiving, processing, queuing and transmission of timing information from the ingress to egress ports), and the link latency of each hop (a propagation delay between two adjacent bridges). These calculated delays are then referenced to the GrandMaster (GM) clock in a bridge elected by the Best Master Clock Algorithm, a clock spanning tree protocol to which all from Clock Master (CM) and endpoint devices attempt to synchronize. Any device which does not synchronize to timing messages is outside of the timing domain boundaries.

## Stream reservation protocol (SRP)



## Multiple streams reservation protocol (MSRP)

## Multiple vlan registration protocol (MVRP)

## Multiple registration protocol (MRP)