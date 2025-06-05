# CoreLib Safe String API (`safe_str`)

This document describes the secure, CLib-independent string and memory utility functions implemented in the `safe_str` module of `CoreLib`. All functions perform input validation and are bounded by `LIBOS_MAX_STR_LEN` to avoid overflows and undefined behavior.

To use these functions, include the header:

```c
#include <CoreLib:safe_str.h>
```

---

## Memory Utilities

### `void *safe_memcpy(void *dst, const void *src, size_t n)`

Copies `n` bytes from `src` to `dst`. Returns `dst`. No operation if `n == 0` or pointers are null.

### `void *safe_memmove(void *dst, const void *src, size_t n)`

Moves `n` bytes from `src` to `dst` safely, even if regions overlap. Returns `dst`.

### `void *safe_memset(void *dst, int val, size_t n)`

Fills `dst` with `val` for `n` bytes. Returns `dst`.

---

## String Length and Null Checking

### `size_t find_null(const char *str, size_t maxlen)`

Finds the first null character within `maxlen` and returns its index. Sets `errno = ENAMETOOLONG` if none found.

### `size_t safe_strnlen(const char *str, size_t maxlen)`

Returns the length of the string up to `maxlen`. Returns 0 if not null-terminated.

---

## String Copy and Duplication

### `char *safe_strncpy(char *dest, const char *src, size_t dest_size)`

Copies at most `dest_size - 1` characters from `src` to `dest` and null-terminates. Returns `dest`.

### `char *safe_strndup(const char *src, size_t maxlen)`

Duplicates `src` up to `maxlen` characters into a heap-allocated string. Returns pointer or NULL.

### `char *safe_strcpyz(char *dest, const char *src, size_t size)`

Copies `src` into `dest` and zero-fills the remaining `size`. Ensures null-termination.

---

## String Concatenation

### `char *safe_strncat(char *dest, const char *src, size_t size)`

Appends `src` to `dest` within `size`, ensuring null-termination. Returns `dest`.

### `char *safe_strcat(char *dest, const char *src)`

Same as `safe_strncat`, with default size of `LIBOS_MAX_STR_LEN`.

---

## Search and Tokenization

### `char *safe_strchrnul(const char *str, int ch, size_t maxlen)`

Returns pointer to first occurrence of `ch` or `\0` within `maxlen`. NULL on error.

### `char *safe_strchr(const char *str, int ch)`

Returns pointer to first occurrence of `ch` or NULL.

### `const char *safe_strrchr(const char *str, char ch, size_t maxlen)`

Returns pointer to last occurrence of `ch` within `maxlen`, or NULL.

### `char *safe_strtok_r(char *str, const char *delim, char **saveptr)`

Reentrant version of `strtok`. Uses `saveptr` to store internal position.

---

## Character Classification

### `int safe_isspace(int c)`, `int safe_tolower(int c)`, `int safe_isdigit(int c)`

Safe character class checks. Avoids dependency on `ctype.h`.

### `int safe_ispunct(int c)`, `int safe_isalnum(int c)`

Extended classification helpers.

---

## String Comparison

### `int safe_strcmp(const char *s1, const char *s2, int flags)`

Compares `s1` and `s2` using bitmask `flags`:

* `IGNORE_CASE`
* `IGNORE_WHITESPACE`
* `IGNORE_PUNCT`
* `IGNORE_DIGITS`
* `IGNORE_SPECIAL`
* `MATCH_PREFIX`
* `MATCH_SUFFIX`

Returns 0 on match, <0 or >0 otherwise.

### `int safe_strstart(const char *str, const char *prefix, int flags)`

Returns non-zero if `str` starts with `prefix`, using `safe_strcmp()` with `MATCH_PREFIX`.

### `int safe_strhasprefix(const char *str, const char *prefix)`

Same as above but with no flags.

### `int safe_strend(const char *str, const char *suffix, int flags)`

Returns non-zero if `str` ends with `suffix`, using `safe_strcmp()` with `MATCH_SUFFIX`.

### `int safe_strhassuffix(const char *str, const char *suffix)`

Same as above but with no flags.

---

## Validation and Helpers

### `char *safe_strstrip(char *str)`

Trims leading and trailing whitespace from `str` in-place. Returns trimmed string.

### `int safe_strisprintable(const char *str, size_t maxlen)`

Checks if all characters in string are printable ASCII. Returns 1 if true.

### `size_t safe_strcount(const char *str, char ch, size_t maxlen)`

Counts occurrences of `ch` in `str` up to `maxlen`.

### `int safe_strvalid(const char *str, size_t maxlen)`

Returns 1 if `str` is null-terminated within `maxlen`, 0 otherwise.

---

For more details about CoreLib, refer to the [CoreLib API documentation](./README.md).
