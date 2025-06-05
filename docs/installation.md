# CoreLib installation Guide

You'll find CoreLib on the [RISC OS Community](https://riscoscommunity.org/add-our-riscpkg-repositories-to-your-packman/) PacMan repository when it'll be completed, prebuilt and packaged, ready for use.

Here you have it in source code format, which you can build yourself.

## Building CoreLib

- git clone this repository on a RISC OS machine and open the directory containing this library.
- Make sure your RISC OS Filer has seen Acorn/Castle/ROOL's DDE (DDE is the RISC OS development environment).
- Double-click the file named `MkDDE` to build the library.
- To clean the build and remove generated files, double-click on `MkDDEClean`.

The build artifacts will be automatically installed in the `!CoreLib` directory, so you can then copy it and place it where you normally have your programming libraries.

## Using CoreLib

To use CoreLib, include the header file in your C source code:

```c
#include "CoreLib:<name_of_the_file>.h"
```

(Check the documentation and the source for a list of available files and the functions they offer.)

Then, link against the library make sure you add:

```text
CoreLib:o.CoreLib
```

In the list of LIBS in your ROOL Shared MakeFile.
