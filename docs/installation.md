# libOS installation Guide

You'll find libOS on the [RISC OS Community](https://www.riscoscommunity.org/) PAcMan repository when it'll be completed, prebuilt and packaged, ready for use.

Here you have it in source code format, which you can build yourself. 

## Building libOS

- git clone this repository on a RISC OS machine and open the directory containing this library.
- Make sure your RISC OS Filer has seen Acorn/Castle/ROOL's DDE (DDE is the RISC OS development environment).
- Double-click the file named `MkDDE` to build the library.
- To clean the build and remove generated files, double-click on `MkDDEClean`.

The build artifacts will be automatically installed in the `!LibOS` directory, so you can then copy it and place it where you normally have your programming libraries.

## Using libOS

To use libOS, include the header file in your C source code:

```c
#include "LibOS:<name_of_the_file>.h"
```

(Check the documentation and the source for a list of available files and the functions they offer.)

Then, link against the library make sure you add:

```text
LibOS:o.LibOS
```

In the list of LIBS in your ROOL Shared MakeFile.
