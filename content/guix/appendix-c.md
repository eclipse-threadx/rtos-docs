---
title: Appendix C - GUIX Widget Styles
description: Learn about the GUIX widget styles.
---


__***General Styles (Used with most widget types):***__

**GX_STYLE_BORDER_NONE**
  - Value: 0x00000000
  - Description: Use this style to draw a widget with no border.

**GX_STYLE_BORDER_RAISED**
  - Value: 0x00000001
  - Description: Draw widget with a raised border.

**GX_STYLE_BORDER_RECESSED**
  - Value: 0x00000002
  - Description: Draw widget with a recessed border.

**GX_STYLE_BORDER_THIN**
  - Value: 0x00000004
  - Description: Draw a one-pixel width border.

**GX_STYLE_BORDER_THICK** 
  - Value: 0x00000008
  - Description: Draw widget with a thick border.

**GX_STYLE_BORDER_MASK**
  - Value: 0x0000000f
  - Description: Mask value used to test only the style fields of the widget style member.

**GX_STYLE_TRANSPARENT**
  - Value: 0x10000000
  - Description: Create a widget that is at least partially transparent. This style should be used when a widget does not draw itself fully opaque, including widgets that draw a semi-transparent pixelmap as the widget background. This style flag informs GUIX that the widget parent must be drawn to refresh the widget background area.

**GX_STYLE_DRAW_SELECTED**
  - Value: 0x20000000
  - Description: Specify that the widget should be drawn using selected state colors and fonts. Different widget types use the DRAW_SELECTED style in different ways to indicate the widget is
currently selected.

**GX_STYLE_ENABLED**
  - Value: 0x40000000
  - Description: Mark the widget as enabled, which allows the widget to accept user input events and generate output signals.
  
**GX_STYLE_DYNAMICALLY_ALLOCATED**
  - Value: 0x80000000
  - Description: Indicates the widget control block memory is dynamically allocated using the gx_system_memory_allocator service when the widget is created, and the control block memory is freed if the widget is destroyed.

**GX_STYLE_USE_LOCAL_ALPHA**
  - Value: 0x01000000
  - Description: Instructs GUIX drawing functions to use the local widget alpha value when drawing the widget. This flag is normally used by the internal GUIX logic to implement widget fading
    animations.


__***Text Alignment Styles (styles applied to all widgets that draw text):***__

**GX_STYLE_TEXT_LEFT**
  - Value: 0x00001000
  - Description: Text is drawn left-aligned within the widget client area.

**GX_STYLE_TEXT_RIGHT** 
  - Value: 0x00002000
  - Description: Text is drawn right-aligned within the widget client
    area.

**GX_STYLE_TEXT_CENTER**
  - Value: 0x00004000
  - Description: Text is drawn center-aligned within the widget client area.

**GX_STYLE_TEXT_COPY**
  - Value: 0x00008000
  - Description: By default, widgets that draw text keep only a pointer to the text which is passed in by the application. For statically defined text that is defined within the string table, there is no reason for the widget to make a private copy of the text assigned. However, if the text assigned to a widget is created dynamically using functions like sprint() or gx_utility_ltoa, then it is often convenient to tell the widget to keep it's own private copy of any text assigned. This allows the application to use automatic or temporary variables when defining the text string, when the application would otherwise be required to define statically defined character arrays for each text widget that is using dynamically defined text. When this style flag is set, the widget will use the gx_system_memory_allocator function to dynamically allocate the memory block needed to hold a private copy of the assigned string. Therefore using this style flag is predicated on the application defining memory_allocator and memory_deallocator functions. GX_STYLE_TEXT_COPY should not be cleared after it has been set, and doing so will cause unpredictable results.

__***Button Styles (apply only to GUIX button widget types):***__

**GX_STYLE_BUTTON_PUSHED**
  - Value 0x00000010
  - Description: Indicates the button is in the pushed or selected state.

**GX_STYLE_BUTTON_TOGGLE**
  - Value 0x00000020
  - Description: Button will switch status between pushed and unpushed on every click event. This style is commonly used with "checkbox" style buttons.

**GX_STYLE_BUTTON_RADIO**
  - Value 0x00000040
  - Description: This style indicates the button will be exclusive, and deselect any button siblings when selected. This style is commonly used with "radio button" style buttons.

**GX_STYLE_BUTTON_EVENT_ON_PUSH**
  - Value: 0x00000080
  - Description: Indicates the button generates a click event when initially pushed. The default operation is to generate a click event when the button is released.

**GX_STYLE_BUTTON_REPEAT**
  - Value 0x00000100
  - Description: Indicates the button should send repeated click events to the button parent when the button is held in the pushed state.

__***List Styles (apply only to GUIX list widget types):***__

**GX_STYLE_CENTER_SELECTED** 
  - Value: 0x00000010
  - Description: Reserved

**GX_STYLE_WRAP**
  - Value 0x00000020
  - Description: The list children wrap from start to end when the list is dragged or scrolled past the starting or ending list index.

**GX_STYLE_FLICKABLE**
  - Value: 0x00000040
  - Description: Reserved

__***Pixelmap Button and Icon Button Styles:***__

**GX_STYLE_HALIGN_CENTER**
  - Value: 0x00010000
  - Description: The button pixelmap should be center aligned within the button boundary on the horizontal axis.

**GX_STYLE_HALIGN_LEFT**
  - Value: 0x00020000
  - Description: The button pixelmap should be left aligned within the button boundary on the horizontal axis.

**GX_STYLE_HALIGN_RIGHT**
  - Value 0x00040000
  - Description: The button pixelmap should be right aligned within the button boundary on the horizontal axis.

**GX_STYLE_VALIGN_CENTER**
  - Value 0x00080000
  - Description: The button pixelmap should be center aligned within the button boundary on the vertical axis.

**GX_STYLE_VALIGN_TOP**
  - Value: 0x00100000
  - Description: The button pixelmap should be top aligned within the button boundary on the vertical axis.

**GX_STYLE_VALIGN_BOTTOM**
  - Value: 0x00200000
  - Description: The button pixelmap should be bottom aligned within the button boundary on the vertical horizontal axis.

__***Slider Styles (Apply only to GX_SLIDER and derived widget types):***__

**GX_STYLE_SHOW_NEEDLE**
  - Value: 0x00000200
  - Description: This style must be included for the slider to draw the needle indicator. This style can be disabled if the application wants to disable the slider needle or draw a custom needle indicator.

**GX_STYLE_SHOW_TICKMARKS**
  - Value: 0x00000400
  - Description: The slider widget will do software drawing of dashed tickmark lines when this style is enabled.

**GX_STYLE_SLIDER_VERTICAL**
  - Value 0x00000800
  - Description: Set this style flag to create a vertical slider, and clear this style flag to create a horizontal slider.

__***Sprite Styles (Applies only to GX_SPRITE widget types):***__

**GX_STYLE_SPRITE_AUTO**
  - Value: 0x00000010
  - Description: Indicates the sprite animation will run automatically when the sprite widget received the GX_EVENT_SHOW event.

**GX_STYLE_SPRITE_LOOP**
  - Value: 0x00000020
  - Description: With this style, the sprite widget will continuously loop through sprite animation frames until the sprite is stopped by the application.

__***Pixelmap Slider Styles:***__

**GX_STYLE_TILE_BACKGROUND**
  - Value 0x00001000
  - Description: The slider background image is tiled to fill the sprite bounding rectangle. This allows a small vertical or horizontal stripe image to be used to fill the slider background.

__***Additional Progress Bar Styles:***__

**GX_STYLE_PROGRESS_PERCENT**
  - Value: 0x00000010
  - Description: When this style is set, the progress bar will draw  bar value as a percentage rather than a raw value. The text is centered in the progress bar bounding rectangle.

**GX_STYLE_PROGRESS_TEXT_DRAW**
  - Value: 0x00000020
  - Description: Draw the current progress bar value as decimal text centered within the progress bar.

**GX_STYLE_PROGRESS_VERTICAL**
  - Value: 0x0000040
  - Description: Indicate the progress is vertically oriented. The default is horizontal orientation.

**GX_STYLE_PROGRESS_SEGMENT_FILL**:
  - **Value**: 0x00000100
  - Description: The progress bar value is indicated with segmented filled rectangles, rather than a solid fill.

__***Additional Radial Progress Bar Styles:***__

**GX_STYLE_RADIAL_PROGRESS_ALIAS**
  - Value: 0x00000200
  - Description: Draw the radial progress bar using anti-aliased brush styles. This requires more CPU bandwidth but also produces a nicer appearance. For lower performance CPU targets, clearing this style flag will result in faster drawing speed.

**GX_STYLE_RADIAL_PROGRESS_ROUND**
  - Value: 0x00000400
  - Description: Use a round line end brush style when drawing the radial progress bar arc. The default is a square line end.

__***Additional Text Input Styles:***__

**GX_STYLE_ CURSOR_BLINK**
  - Value: 0x00000040
  - Description: The text input widget cursor will flash on and off rather than being steady.

**GX_STYLE_ CURSOR_ALWAYS**
  - Value: 0x00000080
  - Description. The text input widget cursor is normally only displayed when the widget owns input focus. This style flag will make the cursor always visible regardless of input focus.

**GX_STYLE_TEXT_INPUT_NOTIFY_ALL**
  - Value: 0x00000100
  - Description: With this style flag set the GX_EVENT_TEXT_EDITED event every time key down event is received by the text input widget.

__***Additional Window Styles:***__

**GX_STYLE_TILE_WALLPAPER**
  - Value: 0x00040000
  - Description: The window will tile any assigned wallpaper image to fill the window client rectangle.

**GX_STYLE_AUTO_HSCROLL**
  - Value: 0x00100000
  - Description: Reserved for future use.

**GX_STYLE_AUTO_VSCROLL**
  - Value: 0x00200000
  - Description: Reserved for future use.

__***Additional Menu Styles:***__

**GX_STYLE_MENU_EXPANDED**
  - Value: 0x00000010
  - Description: Accordion menu widget is initially in expanded state.

__***Additional Tree View Styles:***__

**GX_STYLE_TREE_VIEW_SHOW_ROOT_LINES**
  - Value: 0x00000010
  - Description: Tree view widget should draw lines from node icon to root tree node.

__***Additional Scrollbar Styles:***__

**GX_SCROLLBAR_BACKGROUND_TILE**
  - Value: 0x00010000
  - Description: Reserved for future use.

**GX_SCROLLBAR_RELATIVE_THUMB**
  - Value: 0x00020000
  - Description: The scrollbar thumb width (for a horizontal scroll bar) or height (for a vertical scroll bar) are calculated based on the ratio of the visible area of the parent window to the min and max scrollbar range.

**GX_SCROLLBAR_END_BUTTONS**
  - Value: 0x00040000
  - Description: The scrollbar automatically creates and attaches buttons at each end of the scrollbar region.

**GX_SCROLLBAR_VERTICAL** 
  - Value: 0x01000000
  - Description: The scrollbar is vertically oriented.

**GX_SCROLLBAR_HORIZONTAL**
  - Value: 0x02000000
  - Description: The scrollbar is horizontally oriented.

__***Text Scroll Wheel Styles:***__

**GX_STYLE_TEXT_SCROLL_WHEEL_ROUND**
  - Value: 0x00000200
  - Description: The scroll wheel uses a Sinusoidal algorithm to make the scroll wheel appear to have a rounded shape. This style flag can add significant overhead to the performance of the scroll wheel widget, but can also give the wheel a 3D realistic appearance.