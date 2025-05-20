# libOS Safe String API (`safe_str`)

This document describes the secure, CLib-independent string and memory utility functions implemented in the `safe_str` module of `libOS`. All functions perform input validation and are bounded by `LIBOS_MAX_STR_LEN` to avoid overflows and undefined behavior.

To use these functions, include the header:

```c
#include <LibOS:safe_str.h>
```

---

## Memory Functions

### `void *safe_memcpy(void *dst, const void *src, size_t n);`

Safe alternative to `memcpy`. Copies `n` bytes.

### `void *safe_memmove(void *dst, const void *src, size_t n);`

Safe alternative to `memmove`. Handles overlapping memory.

### `void *safe_memset(void *dst, int val, size_t n);`

Safe memory zeroing or filling function.

---

## String Validation and Length

### `size_t safe_strnlen(const char *str, size_t maxlen);`

Returns the length of a null-terminated string up to `maxlen`. Returns 0 if not valid.

### `size_t find_null(const char *str, size_t maxlen);`

Find index of first null terminator in a string. Returns 0 and sets `errno = ENAMETOOLONG` if none found.

### `int safe_strvalid(const char *str, size_t maxlen);`

Checks whether the string is null-terminated within `maxlen`.

---

## String Copying and Concatenation

### `char *safe_strncpy(char *dest, const char *src, size_t dest_size);`

Safe string copy with enforced null-termination.

### `char *safe_strndup(const char *src, size_t maxlen);`

Duplicate a string safely, up to `maxlen`.

### `char *safe_strcat(char *dest, const char *src);`

Safe concatenation with internal size limit.

### `char *safe_strncat(char *dest, const char *src, size_t size);`

Concatenate with explicit size bounds.

### `char *safe_strcpyz(char *dest, const char *src, size_t size);`

Zero-padded string copy up to `size`.

---

## Comparison and Matching

### `int safe_strcmp(const char *s1, const char *s2, int flags);`

Compare two strings with optional flags:

* `IGNORE_CASE`
* `IGNORE_WHITESPACE`
* `IGNORE_PUNCT`
* `IGNORE_DIGITS`
* `IGNORE_SPECIAL`
* `MATCH_PREFIX`
* `MATCH_SUFFIX`

### `int safe_strstart(const char *str, const char *prefix, int flags);`

### `int safe_strhasprefix(const char *str, const char *prefix);`

Check if string starts with prefix.

### `int safe_strend(const char *str, const char *suffix, int flags);`

### `int safe_strhassuffix(const char *str, const char *suffix);`

Check if string ends with suffix.

---

## Search and Tokenization

### `char *safe_strchrnul(const char *str, int ch, size_t maxlen);`

Find character or null terminator safely.

### `char *safe_strtok_r(char *str, const char *delim, char **saveptr);`

Reentrant string tokenization.

---

## String Analysis and Classification

### `int safe_strisprintable(const char *str, size_t maxlen);`

Check if all characters are printable ASCII.

### `size_t safe_strcount(const char *str, char ch, size_t maxlen);`

Count occurrences of `ch` in `str`, up to `maxlen`.

### `char *safe_strstrip(char *str);`

Trim leading and trailing whitespace in-place.

---

## Character Helpers

### `int safe_isspace(int c);`

### `int safe_tolower(int c);`

### `int safe_isdigit(int c);`

### `int safe_ispunct(int c);`

### `int safe_isalnum(int c);`

Standard character classification functions.

---

## Constants

These constants are defined as flag bitmasks:

```c
#define IGNORE_CASE     0x01
#define IGNORE_WHITESPACE 0x02
#define IGNORE_PUNCT    0x04
#define IGNORE_DIGITS   0x08
#define IGNORE_SPECIAL  0x10
#define MATCH_PREFIX    0x20
#define MATCH_SUFFIX    0x40
```

---

## Example Usage

```c
char dest[128];
safe_strncpy(dest, "Hello", sizeof(dest));

if (safe_strstart("libOS", "lib", IGNORE_CASE)) {
    printf("Starts with 'lib'\n");
}

char *tokens, *rest;
tokens = safe_strtok_r("a,b,c", ",", &rest);
```

---

## Notes

* Functions never invoke undefined behavior and handle all edge cases.
* Avoid use of unsafe standard functions in kernel-level or module-level RISC OS code.
* Maximum string lengths are bounded by `LIBOS_MAX_STR_LEN` internally.

---

This module is essential for ensuring robustness and security in environments lacking full standard library support, such as standalone RISC OS modules.
