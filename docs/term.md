# CoreLib Terminal I/O API (`term`)

This document describes the terminal I/O functions provided by `CoreLib`. These functions offer basic input/output support compatible with RISC OS's SWI calls for console interaction.

Some functions mimic POSIX terminal behavior where possible, while others are stubs due to limitations of the RISC OS kernel.

To use these functions, include the header file in your code:

```c
#include <CoreLib:term.h>
```

---

## Character Input/Output

### `int os_putchar(int c);`

Write a single character to the output stream (`stdout`).

* Returns the character written on success.
* Returns `-1` and sets `errno` on failure.
* Uses `OS_WriteC` SWI.

### `int os_getchar(void);`

Read a single character from the input stream (`stdin`).

* Returns the character read (masked to 8 bits).
* Returns `-1` and sets `errno` on failure.
* Uses `OS_ReadC` SWI.

---

## Terminal Identification

### `int os_isatty(int fd);`

Determine whether a file descriptor refers to a terminal.

* Returns `1` if `fd` is 0 (stdin), 1 (stdout), or 2 (stderr).
* Returns `0` otherwise.

### `const char *os_ttyname(int fd);`

Return the name of the terminal associated with a file descriptor.

* Always returns the static string `"tty"`.

---

## Unsupported Terminal Control

These functions are stubs. Terminal attribute control (`termios`) is not supported on RISC OS.

### `int os_tcgetattr(int fd, void *termios_p);`

Stub for `tcgetattr()`.

* Always returns `-1` and sets `errno = ENOTSUP`.

### `int os_tcsetattr(int fd, int optional_actions, const void *termios_p);`

Stub for `tcsetattr()`.

* Always returns `-1` and sets `errno = ENOTSUP`.

---

## Notes

* Terminal I/O functions are limited to single-character operations.
* Multi-character I/O, echo control, and line buffering must be implemented externally.
* These functions operate only on the console and are best used in CLI tools or debug output.

---

## Example Usage

```c
os_putchar('H');
os_putchar('i');
os_putchar('\n');

int c = os_getchar();
os_putchar(c);  // Echo input
```

---

For more details, see the RISC OS PRM sections on `OS_WriteC` and `OS_ReadC`.
