---
title: GUIX Example
description: The GUIX demonstration system is delivered with a small example, defined in examples/helloworld/helloworld.c.
---

The GUIX demonstration system is delivered with a small example, defined in examples/helloworld/helloworld.c. This example illustrates the steps needed to take to initialize the GUIX system, to set up display drivers. The source code is listed on the following pages.

```c
/* This is a small demonstration of the high-performance GUIX embedded UI run-time environment. This demonstration consists of a simple "Hello World" prompt on top of the root window. */

/* Include necessary system files.

#include "tx_api.h"
#include "gx_api.h"

/* Define constants for the GUIX Win32 demo. */

/* Define the display dimensions specific to this implementation. */
#define DEMO_DISPLAY_WIDTH              320
#define DEMO_DISPLAY_HEIGHT             240

/* Define the number of pixels on the canvas */
#define DEFAULT_CANVAS_PIXELS     (DEMO_DISPLAY_WIDTH * DEMO_DISPLAY_HEIGHT)

/* Define the ThreadX demo thread control block. */
TX_THREAD        demo_thread;

/* Define the stack for the demo thread. */
ULONG            demo_thread_stack[4096 / sizeof(ULONG)];

/* Define the GUIX resources for this demo. */

/* GUIX display represents the physical display device */
GX_DISPLAY       demo_display;

/* GUIX canvas is the frame buffer on top of GUIX displays. */
GX_CANVAS        default_canvas;

/* The root window is a special GUIX background window, right on top of the canvas. */
GX_WINDOW_ROOT   demo_root_window;

/* GUIX Prompt displays a string. */
GX_PROMPT        demo_prompt;

/* Memory for the frame buffer. */
GX_COLOR default_canvas_memory[DEFAULT_CANVAS_PIXELS];

/* Define GUIX strings ID for the demo. */ 
enum demo_string_ids
{
    SID_HELLO_WORLD = 1,
    SID_MAX
};

/* Define GUIX string for the demo. */
CHAR *demo_strings[] = {
    NULL,
    "Hello World"
};

/* User-defined color ID */
#define GX_COLOR_ID_BLACK      GX_FIRST_USER_COLOR
#define GX_COLOR_ID_WHITE       (GX_FIRST_USER_COLOR + 1)

/* User-defined color table. */
static GX_COLOR demo_color_table[] =
{
    /* First, bring in GUIX default color table. These colors are used by GUIX internals. */ 
    GX_SYSTEM_DEFAULT_COLORS_DECLARE,

    /* In this demo, two color entries are added to the color table. */
    GX_COLOR_BLACK,
    GX_COLOR_WHITE 
};

/* Define prototypes. */

VOID  demo_thread_entry(ULONG thread_input);

int main(void)
{
    /* Enter ThreadX. */ 
    tx_kernel_enter();
    
    return (0);
}

VOID tx_application_define(void *first_unused_memory)
{
    /* Create the main demo thread. */
    tx_thread_create(&demo_thread, "GUIX Demo Thread", demo_thread_entry, 0, 
                    demo_thread_stack, sizeof(demo_thread_stack),
                    1, 1, TX_NO_TIME_SLICE, TX_AUTO_START);
}

VOID demo_thread_entry(ULONG thread_input)
{

GX_RECTANGLE    root_window_size;
GX_RECTANGLE    prompt_position;

   /* Initialize GUIX. */ 
   gx_system_initialize();

   /* Install the demo string table. */
   gx_system_string_table_set(demo_strings, SID_MAX);

   /* Install the demo color table. */
   gx_system_color_table_set(demo_color_table, sizeof(demo_color_table) / 
                         sizeof(GX_COLOR));

   /* Create the demo display and associated driver. */
   gx_display_create(&demo_display, "demo display",
                 win32_graphics_driver_setup_16bpp, 
                 DEMO_DISPLAY_WIDTH, DEMO_DISPLAY_HEIGHT);

   /* Create the default canvas. */
   gx_canvas_create(&default_canvas, "demo canvas",&demo_display,
                   GX_CANVAS_MANAGED | GX_CANVAS_VISIBLE, 
                   DEMO_DISPLAY_WIDTH,DEMO_DISPLAY_HEIGHT,
                   default_canvas_memory, sizeof(default_canvas_memory));

   /*Define the size of the root window. */
   gx_utility_rectangle_define(&root_window_size, 0, 0,
                              DEMO_DISPLAY_WIDTH - 1, DEMO_DISPLAY_HEIGHT - 1);

   /* Create a background root window and attach to the canvas. */
   gx_window_root_create(&demo_root_window, "demo root window", &default_canvas,
                        GX_STYLE_BORDER_NONE, GX_ID_NONE, &root_window_size);

   /* Set the root window to be black. */
   gx_widget_background_set(&demo_root_window, GX_COLOR_ID_BLACK, 
                           GX_COLOR_ID_BLACK);

   /* Create a text prompt on the root window. Set the text color to white, and the background to black. */

   /* Define the size and the position of the prompt. */
   gx_utility_rectangle_define(&prompt_position, 100, 90, 220, 130);

   /* Create the prompt on top of the root window using the string defined by string ID SID_HELLO_WORLD. */
   gx_prompt_create(&demo_prompt, NULL, &demo_root_window, SID_HELLO_WORLD, 
                   GX_STYLE_NONE, GX_ID_NONE, &prompt_position);

   /* Set the text color to be white, and the background color to be black. */ 
   gx_prompt_text_color_set(&demo_prompt, GX_COLOR_ID_WHITE, GX_COLOR_ID_WHITE);
   gx_widget_background_set(&demo_prompt, GX_COLOR_ID_BLACK,GX_COLOR_ID_BLACK);

   /* Show the root window. */
   gx_widget_show(&demo_root_window);

   /* let GUIX run! */
   gx_system_start();
}
```
