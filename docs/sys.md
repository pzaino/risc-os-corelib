# libOS System Information API (`sys`)

This document describes the `sys` module in `libOS`, which provides basic system information and identity queries for RISC OS. These functions offer POSIX-style wrappers around static or SWI-derived values.

RISC OS is a single-user OS by design, so several fields are either hardcoded or simulated.

To use these functions, include the header file in your code:

```c
#include <LibOS:sys.h>
```

---

## System Identity

### `int os_uname(struct os_utsname *name);`

Fill out a `struct os_utsname` with system software information.

* Returns `0` on success.
* Returns `-1` and sets `errno` on invalid input.

Populates the following fields:

* `sysname`: Always "RISCOS"
* `nodename`: Always "localhost"
* `machine`: Always "ARM"
* `release` and `version`: Retrieved from `OS_Byte` call (fallbacks to "unknown")

### `int os_getsysinfo(os_sysinfo_t *info);`

Retrieve system hardware information.

* `info` is a pointer to a `struct os_sysinfo`.
* Returns `0` on success, `-1` on failure.
* Fills `info` with system information using `OS_ReadSysInfo`.

```c
typedef struct {
    // Raw register values
    int hw_word_0;
    int hw_word_1;
    int hw_word_2;
    int unique_id_low;
    int unique_id_high;

    // Decoded fields (safe writable buffers)
    char special_chip[SYS_STR_SHORT];
    char io_controller[SYS_STR_SHORT];
    char mem_controller[SYS_STR_SHORT];
    char video_controller[SYS_STR_SHORT];
    char io_chip[SYS_STR_SHORT];
    char lcd_controller[SYS_STR_SHORT];
    char iomd_variant[SYS_STR_SHORT];
    char vidc_variant[SYS_STR_SHORT];
    char hal_device[SYS_STR_LONG];

    // Booleans
    int iic_fast;
    int idle_stops_clk;
} os_sysinfo_t;
```

### `int os_get_cpuid(void);`

Get the CPU ID.

* Returns the CPU ID as an integer.

### `int os_gethostname(char *name, size_t len);`

Get the hostname of the system.

* Writes "localhost" to `name`.
* Returns `0` on success, `-1` on failure.

### `char *os_getlogin(void);`

Return the current login name.

* Always returns the static string: `"riscos"`

---

## User Identity

These functions always return `0`, simulating root-equivalent identity since RISC OS is single-user:

### `int os_getuid(void);`

### `int os_geteuid(void);`

### `int os_getgid(void);`

### `int os_getegid(void);`

---

## Types

### `struct os_utsname`

```c
struct os_utsname {
    char sysname[128];   // OS name (e.g., "RISCOS")
    char nodename[128];  // Network node name (e.g., "localhost")
    char release[128];   // OS release version
    char version[128];   // OS version string
    char machine[128];   // Hardware type (e.g., "ARM")
};
```

---

## Example Usage

```c
struct os_utsname info;
if (os_uname(&info) == 0) {
    printf("System: %s\n", info.sysname);
    printf("Version: %s\n", info.version);
}

char hostname[128];
os_gethostname(hostname, sizeof(hostname));
printf("Host: %s\n", hostname);
```

---

## Notes

* These functions allow simple porting of POSIX programs needing basic identity and host data.
* All fields are simulated or stubbed, as RISC OS lacks multi-user and network identity by default.

---

For additional detail, refer to the PRM documentation on `OS_Byte` and `OS_ReadSysInfo` for version detection methods.
