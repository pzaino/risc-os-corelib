# CoreLib Process API (`proc`)

This document describes the POSIX-inspired process-related API provided by `CoreLib`. These wrappers offer basic process and command execution support for RISC OS. Some functions are stubbed or partially implemented due to architectural limitations.

All functions perform parameter validation and map RISC OS errors to standard `errno` values.

Include it in your code using:

```c
#include <CoreLib:proc.h>
```

---

## Process Identification

### `int os_getpid(void);`

Retrieve the current task's Wimp handle (if in desktop mode).

* Returns a pseudo-process ID (Wimp handle).
* Returns `0` if not running under Wimp.

### `int os_getppid(void);`

Stub. RISC OS has no concept of a parent process.

* Always returns `0`.

---

## Command Execution

### `int os_system(const char *command);`

Execute a shell (CLI) command using `OS_CLI`.

* Returns `0` on success.
* Returns `-1` and sets `errno` on failure.
* Returns `1` if `command` is NULL or empty.

---

## Task Management

### `int os_start_task(const char *cmd);`

Start a new Wimp task using the TaskManager.

* Returns `0` on success (no child PID is returned).
* Returns `-1` on failure.

---

## Unsupported Functions

These functions are not supported on RISC OS. They always return `-1` and set `errno = ENOTSUP`.

### `int os_fork(void);`

Stub for POSIX `fork()`. Not supported.

### `int os_execvp(const char *file, char *const argv[]);`

Stub for POSIX `execvp()`. Not supported.

---

## Notes

* `os_getpid()` is only meaningful when running under the desktop (Wimp environment).
* `os_start_task()` launches applications similarly to `*Filer_Run`, and does not return a child task ID.
* `os_system()` can be used to execute `*commands` from command line or from GUI context.
* There is no process hierarchy in RISC OS, so `getppid()` and `fork()` semantics do not apply.

---

## Example Usage

```c
int main() {
    os_system("Echo Hello from RISC OS CLI");
    int pid = os_getpid();
    os_start_task("MyApp:!MyProgram");
    return 0;
}
```

---

For more details on SWIs: `Wimp_ReadSysInfo`, `TaskManager_StartTask`, `OS_CLI` (see RISC OS PRM Volumes 1 and 3).
