---
title: Appendix G - GUIX Font Structure
description: Learn about the GUIX font structure.
---

# Appendix G - GUIX Font Structure

GUIX fonts are normally produced by the GUIX Studio application, and font glyphs are rendered by the GUIX display driver. The application software need only specify the font and colors that each text display widget should use. The GUIX font data structures are documented here for completeness, and to enable developers to create their own methods for generating or converting other fonts into the GUIX font format.

Each GUIX font starts with a GX_FONT structure. The GX_FONT structure defines global font parameters, such as the character included within the font and the line height of the font. The GX_FONT structure points at an array of GX_GLYPH structures. Each GX_GLYPH structure defines
the width, height, and baseline offset of one specific character glyph. The GX_GLYPH structure also points to the actual glyph bitmap data (which may be NULL for whitespace characters).

The **GX_FONT** structure, contained in gx_api.h, is declared as follows:

```c
typedef struct GX_FONT_STRUCT
{
    GX_UBYTE                     gx_font_format
    GX_UBYTE                     gx_font_prespace
    GX_UBYTE                     gx_font_postspace
    GX_UBYTE                     gx_font_line_height 
    GX_UBYTE                     gx_font_baseline
    USHORT                       gx_font_first_glyph
    USHORT                       gx_font_last_glyph 
    GX_CONST GX_GLYPH           *gx_font_glyphs
    const struct GX_FONT_STRUCT *gx_font_next_page
} GX_FONT;
```

The *gx_font_format* field defines the font bits-per-pixel and other flags, as defined in the gx_api.h header file.

The *gx_font_prespace* defines the pixel space to skip above each line of text in a multi-line text display.

The *gx_font_postspace* field defines the pixel space to skip below each line of text in a multi-line text display.

The *gx_font_line_height* field defines the height of the tallest glyph in the font.

The *gx_font_baseline* field defines the distance, in pixels, from the top row of glyph pixels to the font baseline.

The *gx_font_first_glyph* field defines the first Unicode character encoding included in this font page.

The *gx_font_last_glyph* field defines the last Unicode character encoding included in this font page.

The *gx_font_glyphs* pointer points to an array of GX_GLYPH structures. This array must be equal in size to the number of characters contained on this font page, i.e (*gx_font_last_glyph* – *gx_font_first_glyph*) + 1.

The *gx_font_next_page* member is used for multiple page fonts. Multiple page fonts are used for extended character sets and to optimize the size of the GX_GLYPH structure arrays. If all of the characters of the font are contained within one font page, or if this is the last page of the font in question, the *gx_font_next_page* member is set to GX_NULL.

As noted above, the **GX_FONT** structure above contains a pointer to an array of GX_GLYPHS structures. There must be one GX_GLYPH structure for each character on the font page. The **GX_GLYPH** structure is defined as:

```c
typedef struct GX_GLYPH_STRUCT
{
    GX_CONST GX_UBYTE *gx_glyph_map;
    GX_BYTE            gx_glyph_ascent;
    GX_BYTE            gx_glyph_descent;
    GX_BYTE            gx_glyph_advance;
    GX_BYTE            gx_glyph_leading;
    GX_UBYTE           gx_glyph_width;
    GX_UBYTE           gx_glyph_height;
} GX_GLYPH;
```

The gx_glyph_map pointer points to the glyph bitmap. This pointer may be GX_NULL for whitespace characters. The bitmap data is encoded as 1 bpp, 2 bpp, 4 bpp, or 8 bpp alpha values. For 1 bit data, a value of 1 indicates that the pixel should be written in the foreground color, and a value of 0 indicates that the pixel is transparent. For 8 bit data, the values range from 0 (fully transparent) to 255 (fully opaque). All intermediate values represent a blending value for anti-aliased fonts. The glyph bitmap data is always padded to full byte alignment for formats using less than 8bpp data values.

The *gx_glyph_ascent* and gx_glyph_descent values position the glyph vertically with respect to the font baseline.

The *gx_glyph_width* and gx_glyph_height values specify the size of the glyph bitmap data.

The *gx_glyph_advance* value specifies the pixel width to advance the drawing position after drawing the glyph (this may not be equal to the glyph width).

The *gx_glyph_leading* value specifies the pixels to advance in the x-direction prior to rendering the glyph.