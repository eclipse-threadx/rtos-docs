---
title: Chapter 1 - Introduction to GUIX
description: GUIX is a high-performance real-time implementation of a GUI designed exclusively for embedded ThreadX-based applications.
---


GUIX is a high-performance real-time implementation of a graphical interface framework designed
exclusively for embedded ThreadX-based applications. This chapter
contains an introduction to GUIX and a description of its applications
and benefits.

## GUIX Feature Overview

Unlike many other GUI implementations, GUIX is designed to be versatile—easily scaling from small micro-controller-based applications to those that use powerful RISC and DSP processors. This is in sharp contrast to public domain or other commercial implementations originally intended for workstation environments but then squeezed into
embedded designs. An overview of GUIX features follows:

- Easy to use with host-based design tool GUIX Studio

- Win32 GUIX run-time environment for complete hosted prototyping

- Supports most processors supported by ThreadX

- Written exclusively in ANSI C

- Endian neutral

- Smallest, Fasted Embedded GUI

- Run-time configurable, number of objects, screen size, etc.

- Easy to write display driver interface

- Color (up to 32-bpp color depth), monochrome, and grayscale support

- Multilingual support via UTF8 string encoding and string resources

- Default free fonts and easy to add new fonts

- Multiple drawing Canvases supported, of various sizes

- Multiple displays of different sizes and color depths supported

- Screen Transition support (fade in, fade out, swipe, etc.)

- Touch Screen, Gesture, and Virtual Keyboard Support

- Bitmap compression

- Alpha Blending Support

- Dither Support

- Anti-Aliasing Support

- Skinning and Themes

- Canvas Blending

- Complete Window Management

  - Parent/Child Relationship

  - Dynamic creation, deletion, resizing, moving
  - Separate event handling and drawing 
  - Z-order
  - Clipping and views

- Extensive Set of Widgets

  - Various button types, sliders, and dials

  - Drop Down List
  
  - Prompt

  - Multi-Line text view
  
  - Single and Multi-Line text input
  
  - Numeric and Textual Scroll Wheels
  
  - Windows and Scroll Bars
  
  - Radial Progress Bar
  
  - Sprite

### ANSI C Source Code

GUIX is written completely in ANSI C and is portable immediately to
virtually any processor architecture that has an ANSI C compiler and
ThreadX support. Although written in ANSI C, GUIX uses an object
oriented model and inheritance.

### Not A Black Box

Most distributions of GUIX include the complete C source code. This
eliminates the "black-box" problems that occur with many commercial GUI
implementations. By using GUIX, applications developers can see exactly
what the GUI is doing—there are no mysteries!

Having the source code also allows for application specific
modifications. Although not recommended, it is certainly beneficial to
have the ability to modify the GUI if it is required. These features are
especially comforting to developers accustomed to working with in-house
or public domain products. They expect to have source code and the
ability to modify it. GUIX is the ultimate GUI software for such
developers.

## Embedded GUI Applications

Embedded GUI applications are applications that have a user interface
requirement and execute on microprocessors hidden inside products such
as cellular phones, communication equipment, automotive engines, laser
printers, medical devices, and so forth. Such applications almost always
have some memory and performance constraints. Another distinction of
embedded GUI is that their software and hardware have a dedicated
purpose.

### Real-time GUI Software

Basically, GUI software that must perform its processing within an exact
period of time is called *real-time GUI* software, and when time
constraints are imposed on GUI applications, they are classified as
realtime applications. Embedded GUI applications are almost always
real-time because of their inherent interaction with the external world.

## GUIX Benefits

The primary benefits of using GUIX for embedded applications are
high-performance, feature rich, and very small memory requirements. GUIX
is also completely integrated with the high-performance, multitasking
ThreadX real-time operating system.

### Improved Responsiveness

The high-performance GUIX product enables applications to respond faster
than ever before. This is especially important for embedded applications
that either have a significant volume of visual information or strict
timing requirements on displaying such information.

### Software Maintenance

Using GUIX allows developers to easily partition the GUI aspects of
their embedded application. This partitioning makes the entire
development process easy and significantly enhances future software
maintenance.

### Increased Throughput

GUIX provides the highest-performance GUI available, which directly
transfers to the embedded application. GUIX applications are able to
process user interface information faster than non-GUIX applications!

### Processor Isolation

GUIX provides a robust, processor-independent interface between the
application and the underlying processor and display hardware. This
allows developers to concentrate on the high-level aspects of the user
interface rather than spending extra time dealing with display hardware
issues.

### Ease of Use

GUIX is designed with the application developer in mind. The GUIX
architecture and service call interface are easy to understand. As a
result, GUIX developers can quickly use its advanced features.

### Improve Time to Market

The powerful features of GUIX accelerate the software development
process. GUIX abstracts most processor and display hardware issues,
thereby removing these concerns from a majority of application user
interface implementation. This feature, coupled with the ease-of-use and
advanced feature set, results in a faster time to market!

### Protecting the Software Investment

GUIX is written exclusively in ANSI C and is fully integrated with the
ThreadX real-time operating system. This means GUIX applications are
instantly portable to all ThreadX supported processors. Better yet, a
completely new processor architecture can be supported with ThreadX in a
matter of weeks. As a result, using GUIX ensures the application's
migration path and protects the original development investment.
