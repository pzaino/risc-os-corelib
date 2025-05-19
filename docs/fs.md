# libOS File System API (`fs`)

This document describes the POSIX-like file system API exposed by `libOS`, a C99 library for RISC OS. All functions are prefixed with `os_` and operate with internal argument validation and SWI-level error handling.

Include it in your code using:

```c
#include <LibOS:fs.h>
```

---

## File and Directory Operations

### `int os_mkdir(const char *path, int mode);`

Create a new directory.

* `mode` is ignored.
* Returns 0 on success, -1 on failure (sets `errno`).

### `int os_rmdir(const char *path);`

Remove a directory if it's empty.

* Checks type using `os_stat()`.
* Returns 0 on success, -1 on failure.

### `int os_chdir(const char *path);`

Set the current working directory.

### `char *os_getcwd(char *buf, size_t size);`

Get the current working directory path into `buf`.

### `int os_unlink(const char *path);`

Delete a file.

### `int os_rename(const char *oldpath, const char *newpath);`

Rename or move a file or directory.

### `int os_stat(const char *path, os_stat_t *st);`

Get file status (size, type, etc).

### `int os_stat_fd(int fd, os_stat_t *st);`

Get file status from a file descriptor.

### `int os_access(const char *pathname, int mode);`

Check if file exists. `mode` is ignored.

### `int os_set_filetype(const char *path, int filetype);`

Set the RISC OS filetype (12-bit).

### `int os_get_filetype(const char *path, int *filetype);`

Retrieve the RISC OS filetype.

### `int os_file_exists(const char *path);`

Returns 1 if file exists, 0 if not.

### `int os_is_dir(const char *path);`

Check if the path refers to a directory.

---

## File Descriptors

### `int os_open(const char *path, int flags, int mode);`

Open a file. `flags` is a combination of POSIX-style flags (e.g., `O_RDONLY`, `O_WRONLY`, `O_CREAT`).

### `int os_close(int fd);`

Close a file descriptor.

### `int os_close_path(const char *path);`

Close the open file matching the given path.

### `int os_read(int fd, void *buf, size_t count);`

Read up to `count` bytes into `buf`.

### `int os_write(int fd, const void *buf, size_t count);`

Write up to `count` bytes from `buf`.

### `long os_lseek(int fd, long offset, int whence);`

Seek to position `offset`.

* `whence`: `SEEK_SET`, `SEEK_CUR`, or `SEEK_END`

---

## Directory Iteration

### `os_dir_t *os_opendir(const char *path);`

Open a directory stream.

### `os_dirent_t *os_readdir(os_dir_t *dirp);`

Read next entry from directory stream.

### `int os_closedir(os_dir_t *dirp);`

Close and free directory stream.

### `void os_rewinddir(os_dir_t *dirp);`

Reset directory stream to start.

### `long os_telldir(os_dir_t *dirp);`

Get current directory position.

### `void os_seekdir(os_dir_t *dirp, long loc);`

Seek to a position within a directory stream.

---

## File Attribute and Permissions (Stubs)

These functions are not supported on RISC OS and return `ENOTSUP`:

* `os_chmod()`
* `os_fchmod()`
* `os_chown()`
* `os_fchown()`
* `os_ftruncate()`

### `int os_umask(int mask);`

Always returns 0 (permissions not enforced).

---

## File Manipulation

### `int os_truncate(const char *path, long length);`

Truncate or expand a file to given length. Content is preserved or zero-padded.

### `int os_merge(const char *dest, const char *src);`

Append `src` to `dest`. Returns 0 on success.

### `int os_split(const char *path, long offset, const char *out1, const char *out2);`

Split `path` into two parts:

* `out1` contains bytes `[0, offset)`
* `out2` contains the rest

---

## Utilities

### `char *os_get_open_files_list(void);`

Returns a matrix of currently open file paths (256 entries max).

---

## Notes

* All functions perform input validation.
* Errors are translated from RISC OS to POSIX `errno` values.
* File and directory types follow POSIX constants (`S_IFDIR`, `S_IFREG`, etc.) internally.

---

## Types

### `struct os_stat_t`

```c
struct os_stat_t {
    int st_mode;    // file type (e.g., S_IFDIR, S_IFREG)
    size_t st_size; // file size in bytes
    time_t st_mtime;// last modified time (if available)
};
```

### `struct os_dir_t`

```c
struct os_dir_t {
    char path[256];
    int context;
    char entry[256];
};
```

### `struct os_dirent_t`

```c
struct os_dirent_t {
    char d_name[256];
};
```

---

For more details on the underlying SWIs, see RISC OS PRM Volumes 1 and 2.
For additional information on the library's design and usage, refer to the [README](README.md) file.
