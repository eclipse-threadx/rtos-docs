---
title: Chapter 1 - Introduction to ThreadX for ARMv8-M.
description: This chapter is the starting point for reading about the ThreadX Addendum for ARMv8-M.
---

# Chapter 1  Overview

The ARMv8-M architecture introduces new security features, including TrustZone, which allows memory to be tagged as secure or non-secure. Following ARM's guidelines, ThreadX (and the user application) is designed to be run in non-secure mode. ThreadX (and the user application) can also be run in secure mode. In order to interface with secure mode software, some new ThreadX APIs are necessary. This document describes these ThreadX services that are specific to the ARMv8-M architecture, including the Cortex-M23, Cortex-M33, Cortex-M35P, and Cortex-M55.
