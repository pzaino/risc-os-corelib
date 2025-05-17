# LibOS

**WORK IN PROGRESS** This library is being converted from the old Acorn Archimedes/RPC era to the new RISC OS 5 and DDE.

This is a really old library of mine which was original written in 1996/1998 for my Acorn computers and RISC OS. Now I am updating it for RISC OS 5 and latest DDE (so it now uses shared Makefiles, new `_swix` syntax etc.).

It's a library that is meant to make RISC OS API look and feel like a POSIX library. This has few advantages:

- It makes RISC OS programming more intuitive for who's familiar with UNIX.
- It makes porting UNIX code to RISC OS a bit easier.
- It adds arguments data type and value checking which reduce the risk of passing wrong values to RISC OS API and getting crashes.
- It adds some extra functions that are not available in RISC OS API.

It's not a life changing library, but it does make programming in C on RISC OS faster, easier and more fun.

Another advantage is that one can use Linux man's to look up for functions and their arguments, all libOS has different is that the functions are prefixed with "os_" and the arguments are checked for validity. The library is not complete, but it does cover most of the common functions. The library is also not a replacement for RISC OS API, it's just a wrapper around it. So you can still use RISC OS API directly if you need to.

LibOS is designed to be CLib independent, so it should be usable also when building Utils.

This library has nothing to do with the RISC OS UnixLib and GCC, I created it at the time for Acorn/Castle/ROOL DDE. GCC and the UnixLib cover way more than libOS does.

## Building

To build the library, you need to have RISC OS development environment set up. When in the Desktop open the directory containing this library and double click on the `MkDDE` file.
