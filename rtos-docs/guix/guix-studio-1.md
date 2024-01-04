---
title: GUIX Studio User Guide
description: This guide provides comprehensive information about GUIX Studio, the Microsoft Windows-based rapid UI development environment specifically designed for the GUIX runtime library from Eclipse Foundation.
---
# Chapter 1: Introduction to GUIX Studio

GUIX Studio is a Microsoft Windows-based rapid UI development environment specifically designed for the GUIX runtime library from Eclipse Foundation.

Embedded UI Developers can utilize the GUIX Studio WYSIWYG screen designer to quickly create and update their embedded UI using the GUIX run-time environment. GUIX Studio designs are saved and maintained in a GUIX Studio project file, which has the extension .gxp. When your design is ready for execution on the target, GUIX Studio generates C code that contains all the necessary UI information and code.

## GUIX Studio Requirements

In order to function properly, GUIX Studio requires *Windows XP* (or above). The system should have a minimum of 200MB of RAM, 2GB of available hard-disk space, and a minimum display of 1024x768 with 256 colors. In addition, the embedded application must be running on *ThreadX/GUIX V6.0* or later.

If you would like to be able to build and run the embedded UI application as a stand-alone Windows executable, you will also need a compiler or build environment capable of compiling C source code to produce a Windows executable. The evaluation package included with GUIX Studio also includes Visual Studio 2019 compatible project files and solutions for each of the provided example applications. If you are using a different compiler, you will need to create your own project files or make files for the purposes of building your example applications.

## GUIX Studio Constraints

The GUIX Studio UI design tool has several constraints, as follows:

- A maximum of 4 displays per project.
- A maximum of 100,000 widgets per GUIX Studio project.
- A maximum of 100,000 distinct resources, e.g., colors, fonts, pixelmaps, strings, etc.
