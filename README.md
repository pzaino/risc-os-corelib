# LibOS

**WORK IN PROGRESS**  
This library is currently being updated from its original Acorn Archimedes/RPC-era implementation to support RISC OS 5 and the modern DDE toolset. Please be patient while this work is in progress. If you'd like to contribute, feel free to get in touch.

This is a legacy library I originally wrote between 1996 and 1998 for use on Acorn machines running RISC OS. I'm now modernising it to support RISC OS 5, Shared Makefiles, and the newer `_swix()` syntax.

LibOS is designed to make the RISC OS API feel more like a POSIX-compatible C library. The goal is to improve developer experience by offering:

- A more intuitive interface for developers familiar with Unix-style programming.
- Easier porting of Unix software to RISC OS.
- Built-in validation of argument types and values to reduce crashes due to incorrect SWI usage.
- A few additional helper functions not present in the native RISC OS API.

LibOS is not intended to replace the RISC OS API. It's a wrapper — a safety layer that enhances usability while still allowing you to call native SWIs directly when needed.

It's not a "life-changing" library — but it does make writing C programs for RISC OS faster, safer, and more enjoyable.

One practical benefit is that you can often consult standard Linux `man` pages for documentation. LibOS adopts POSIX-style function names (prefixed with `os_`) and behavior, but with added argument validation for RISC OS safety. The library is not yet complete, but already wraps most commonly used functionality.

**Important note:** This library is **not related to GCC's UnixLib**. I originally created it for use with Acorn/Castle/ROOL's DDE (and CLib). At the time, OSLib and RISC_OSLib were still prone to errors, and on RISC OS a simple mistake in a SWI call can crash or destabilize the system.  
UnixLib and GCC cover a wider range of POSIX functions than LibOS — but the purpose here is different. LibOS uses a POSIX-like API to make things more intuitive, while ensuring safety and compatibility.

## License

This library is released under the MPL 2.0 license. See the [LICENSE](LICENSE) file for full terms.

## Building

To build the library, make sure your RISC OS development environment is correctly set up.  
From the RISC OS Desktop:

1. Open the directory containing this library.
2. Double-click the file named `MkDDE`.

To clean the build and remove generated files, double-click on `MkDDEClean`.
