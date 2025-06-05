# CoreLib Path API (`path`)

This document provides reference documentation for the path manipulation API in `CoreLib`. These functions offer basic utilities for joining, converting, and extracting components of file paths, with special support for RISC OS path formats.

To use the path functions, include the header file in your code:

```c
#include <CoreLib:path.h>
```

---

## Path Resolution and Validation

### `int os_realpath(const char *in, char *out, size_t size);`

Resolve the full canonical path of `in` and store it in `out`.

* Returns `0` on success.
* Returns `-1` on error (e.g., if buffer is too small or invalid input).
* Uses `OS_FSControl` reason 37.

### `int os_path_is_absolute(const char *path);`

Check if the given path is absolute (RISC OS-style).

* Returns `1` if `path` starts with `$`, `0` otherwise.

---

## Path Composition

### `char *os_path_join(const char *a, const char *b, char *out, size_t maxlen);`

Join two path segments `a` and `b` using RISC OS dot (`.`) separator.

* Adds separator if needed.
* Returns pointer to `out` on success, `NULL` on error.

---

## Path Component Extraction

### `const char *os_path_basename(const char *path);`

Return the final component of the path.

* Returns pointer to the basename (last token after `.`).
* Returns original path if no dot found.

### `char *os_path_dirname(const char *path, char *out, size_t maxlen);`

Extract the directory component from the path.

* Writes the portion before the last dot into `out`.
* Returns pointer to `out` on success, `NULL` on error.
* Returns `"."` if no dot found.

---

## Format Conversion

### `char *os_path_to_riscos(const char *unix_path, char *out, size_t maxlen);`

Convert a Unix-style path (`/path/to/file`) to RISC OS format (`$.path.to.file`).

* Leading `/` becomes `$.`
* Slashes `/` are replaced by dots `.`
* Returns `out` on success, `NULL` on failure.

### `char *os_path_to_unix(const char *riscos_path, char *out, size_t maxlen);`

Convert a RISC OS path (`$.path.to.file`) to Unix-style (`/path/to/file`).

* Leading `$.` becomes `/`
* Dots `.` are replaced by slashes `/`
* Returns `out` on success, `NULL` on failure.

---

## Notes

* All functions validate arguments and set `errno` appropriately.
* Output buffers must be pre-allocated by the caller.
* RISC OS paths use `.` as a separator; Unix paths use `/`.
* These helpers are essential for writing portable code across RISC OS and Unix environments.

---

## Example Usage

```c
char full[256];
os_realpath("App:foo.bar", full, sizeof(full));

char joined[256];
os_path_join("MyDir", "Sub.File", joined, sizeof(joined));

const char *base = os_path_basename("Disk.$.Utils.Editor");

char dir[256];
os_path_dirname("Disk.$.Utils.Editor", dir, sizeof(dir));

char unix[256];
os_path_to_unix("$.MyDir.Sub.File", unix, sizeof(unix));

char riscos[256];
os_path_to_riscos("/MyDir/Sub/File", riscos, sizeof(riscos));
```

---

For more details, see the PRM entry for `OS_FSControl` reason code 37 (Get full path).
