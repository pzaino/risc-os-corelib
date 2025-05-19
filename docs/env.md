# libOS Environment API (`env`)

This document describes the POSIX-style environment variable API implemented in `libOS`, designed to provide safe wrappers around RISC OS SWIs for variable access and manipulation.

All functions use internal validation and map RISC OS errors to POSIX-style `errno` codes.

Include it in your code using:

```c
#include <LibOS:env.h>
```

---

## Environment Variable Access

### `char *os_getenv(const char *name);`

Retrieve the value of an environment variable.

* Returns a static buffer with the value.
* Returns `NULL` on failure and sets `errno`.

### `int os_setenv(const char *name, const char *value, int overwrite);`

Set or update an environment variable with a string value.

* `overwrite = 0` prevents overwriting an existing variable.
* Returns 0 on success, -1 on failure.

### `int os_setenv_typed(const char *name, const char *value, int length, int type, int overwrite);`

Lower-level function for setting variables with explicit `length` and `type`.

* `type` is a RISC OS variable type (0 = string).
* If `length == -1`, the value is interpreted as NULL (to delete).
* Returns 0 on success, -1 on failure.

### `int os_unsetenv(const char *name);`

Unset (delete) an environment variable.

* Returns 0 on success, -1 on error.
* If the variable doesn't exist, succeeds silently (POSIX behavior).

### `int os_env_exists(const char *name);`

Check whether an environment variable exists.

* Returns 1 if it exists, 0 otherwise.
* Returns 0 and sets `errno` on error.

### `int os_clearenv(void);`

Clear a known set of common environment variables.

* Always returns 0 (best-effort approach).
* Removes: `Alias$`, `File$`, `Inet$`, `Inet$ResolvConf`, `Wimp$ScrapDir`, `Run$`, `App$`, `Path$`.

---

## Notes

* `os_getenv()` and `os_env_exists()` both use internal scratch buffers for safety.
* `os_setenv_typed()` allows direct access to typed variables (e.g., integers or blocks), but most use cases should prefer `os_setenv()`.
* Environment variables in RISC OS are more flexible than POSIX: they support types and may represent dynamic lookups (like `Alias$` and `Run$Path`).
* Internally uses `OS_ReadVarVal` and `OS_SetVarVal` SWIs.

---

## Constants

### `#define OS_ENV_MAX_LEN 4096`

Maximum environment variable size used internally (buffer length).

---

## Example Usage

```c
os_setenv("MY_VAR", "hello", OS_ENV_OVERWRITE);
const char *val = os_getenv("MY_VAR");
os_unsetenv("MY_VAR");
```

---

For more details on the underlying SWIs, refer to the PRM section on `OS_ReadVarVal` and `OS_SetVarVal`.
For further information on libOS check the [libOS documentation](README.md).
