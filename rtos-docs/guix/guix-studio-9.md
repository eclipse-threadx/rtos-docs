---
title: GUIX Studio Command Line
description: GUIX Studio provides command-line invocation that is useful for build pipelines that are required to update the Studio-generated output files.
---
# Chapter 9: GUIX Studio Command Line

GUIX Studio supports command-line invocation,  which is useful for build pipelines that are required to update of the Studio-generated output files.

## Command-Line Usage

**Usage:** guix_studio \[OPTION\] \[ARGUMENT\]

Open the *.gxp* project.

Open the Studio project and generate desired output files.


**Examples:**

`guix_studio.exe demo.gxp`  
Open "demo.gxp" project

`guix_studio.exe –p demo.gxp`  
Open "demo.gxp" project

`guix_studio.exe –n –p demo.gxp`  
Generate all output files of demo.gxp project.

`guix_studio.exe –n –r –p demo.gxp`  
Generate resource files of demo.gxp project.

`guix_studio.exe -x resource.xml -b`
Generate binary resource file from a resource project resource.xml.


## Command-Line Options

***-n --nogui***  

The "nogui" option. Tell GUIX Studio to run without starting the windowing UI interface.

***-o pathname, --log***

Log option, specify a log file.

***-b, --binary***

Binary resource option. Produces a binary resource file rather than a C file.

***-d display1, display2, --display***

Display names option. If this option is used, then only the specified display names are included in any generated resource or specification files. If this option is not used,  all displays are included.

***-t theme1, theme2, --theme***

Theme name(s) option. If this option is used, then only the specified theme names are included in any generated resource or specification files. If this option is not used, all themes are included.

***-l language1, language2, --language***

Language name(s) option. If this option is used,  the specified language names are included in the generated resource or specification files. Otherwise all language names are included.

***-r [filename], --resource***

The resource option, specifies that Studio should produce a resource file for previously designated display(s), theme(s), and language(s).

***-s [filename], --specification***

The specification option, specify that studio should produce a specification file for designated display(s), theme(s), and language(s).

***-p project_pathname, --project***

Project pathname option, specify the example project to be loaded.

***-i [pathname], --import***

Import string from xliff or csv format file.

***--big_endian***  

Generate resource data in big-endian format.

***--no_res_header***

Not generating resource header.

***-x [pathname], --xml***

Specify the input resource XML file.

***--output_path pathname***

Specify the output directory. If not specified, the project directory will be used for the output files.
