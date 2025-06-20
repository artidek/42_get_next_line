# get_next_line

This is my implementation of the `get_next_line` function, designed for the 42 School curriculum. This project provides a function that reads a line from a file descriptor, handling buffered input efficiently and safely. The bonus implementation supports reading lines from multiple file descriptors simultaneously.

---

## Table of Contents

- [Features](#features)
- [How It Works](#how-it-works)
- [Bonus: Multiple File Descriptors](#bonus-multiple-file-descriptors)
- [Usage](#usage)
- [Implementation Details](#implementation-details)
- [API Reference](#api-reference)
- [Customization](#customization)
- [Testing](#testing)
- [License](#license)
- [Author](#author)

---

## Features

- **Buffered reading**: Efficiently reads from files or input streams with customizable buffer size.
- **Handles incomplete lines**: Correctly returns lines even if they span multiple reads.
- **Bonus**: Supports reading from multiple file descriptors simultaneously (e.g., multiple files).

---

## How It Works

`get_next_line` reads a file descriptor until it encounters a newline `\n` or EOF. It maintains internal static storage to handle cases where a line spans multiple reads.

**Key steps:**
1. **Buffering**: Reads chunks of data into a buffer.
2. **Static Storage**: Keeps leftover data from the previous call that didn’t complete a line.
3. **Line Construction**: Concatenates buffered data until a newline is found.
4. **Return Value**: Returns the next line, including the newline character if present, or `NULL` at EOF.

---

## Bonus: Multiple File Descriptors

The bonus implementation allows `get_next_line` to be called on several file descriptors interleaved, and it will return the correct next line for each. This is achieved by maintaining a separate buffer for each file descriptor.

**Use-case Example:**
```c
int fd1 = open("file1.txt", O_RDONLY);
int fd2 = open("file2.txt", O_RDONLY);

char *line1 = get_next_line(fd1);
char *line2 = get_next_line(fd2);
// You can alternate calls between fd1, fd2, etc.
```

---

## Usage

1. **Include the header:**
   ```c
   #include "get_next_line.h"
   // or for bonus
   #include "get_next_line_bonus.h"
   ```

2. **Compile the sources:**
   ```sh
   gcc -Wall -Wextra -Werror get_next_line.c get_next_line_utils.c -o gnl
   # For bonus:
   gcc -Wall -Wextra -Werror get_next_line_bonus.c get_next_line_utils_bonus.c -o gnl_bonus
   ```

3. **Call the function:**
   ```c
   int fd = open("file.txt", O_RDONLY);
   char *line = get_next_line(fd);
   while (line) {
       // process line
       free(line);
       line = get_next_line(fd);
   }
   close(fd);
   ```

---

## Implementation Details

- **Static Buffering**: Uses a static array indexed by file descriptor for residue data.
- **Helper Functions**: Includes custom implementations of `ft_strlen`, `ft_strdup`, `ft_strchr`, and a dynamic `realloc_line`.
- **Buffer Size**: The buffer size is set via the `BUFFER_SIZE` macro (default 4096). Can be overridden at compile-time.
- **Error Handling**: Returns `NULL` on errors or when no more lines are available.

---

## API Reference

### `char *get_next_line(int fd);`

- **Parameters**: `fd` – file descriptor to read from
- **Returns**: Next line as a `char*` (including `\n` if present), or `NULL` at EOF/error
- **Memory**: The returned string is dynamically allocated. Caller must `free()` it.

### Helper Functions

- `size_t ft_strlen(const char *str);`
- `char *ft_strdup(const char *s);`
- `char *ft_strchr(const char *str, int ch);`
- `void realloc_line(char **r_line, char *buffer, int size);`

---

## Customization

- **Change buffer size** by defining `BUFFER_SIZE` at compile time:
  ```sh
  gcc -DBUFFER_SIZE=1024 ...
  ```

---

## Testing

You can test with multiple files and file descriptors to verify correct behavior, especially for the bonus part.

---

## License

This project is released for educational purposes as part of the 42 School curriculum.

---

## Author

Artem Obshatko  
[@artidek](https://github.com/artidek)
