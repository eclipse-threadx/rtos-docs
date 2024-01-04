---
title: Chapter 1 - Introduction to NetX Duo mDNS/DNS-SD
description: NetX Duo mDNS/DNS-SD augments the traditional DNS service.
---

# Chapter 1 - Introduction to NetX Duo mDNS/DNS-SD

The mDNS and DNS-SD are protocols designed to augment the traditional DNS service. mDNS provides host name and service lookup to the nodes on the local network. Each node uses IPv4 or IPv6 multicast channel to announce services it offers to its neighbors, responds to queries from its neighbors, and sends queries on behave of its applications. By design, mDNS operates in a distributed environment, thus eliminates a centralized serve.

DNS-SD is built on top of mDNS. DNS-SD allows nodes to announce services they provide to the local network or to discover services offered by other nodes on the local network. Throughout the document, the term *mDNS* refers to the services that cover both the mDNS specification and the DNS-SD specification.

## mDNS Standard

NetX Duo mDNS/DNS-SD implementation conforms to the following RFCs:

- RFC 6762: mDNS Specification
- RFC 6763: DNS-SD Specification