# Minishell

This project has been created as part of the 42 curriculum by "norabino" - Noé RABINOVICI.

![C Language](https://img.shields.io/badge/Language-C-blue.svg)
![42 School](https://img.shields.io/badge/School-42-black.svg)
![Build](https://img.shields.io/badge/Build-Passing-brightgreen.svg)

A minimal Unix shell implementation written in C, replicating core functionalities of `bash`. Designed to master low-level system programming concepts including process creation, file descriptor manipulation, pipes, redirections, and signal handling.

---

## Features

- **Interactive Shell Loop**: Displays a prompt (`$> `) and maintains command history via `readline`.
- **Command Execution**: Handles external binaries using `execve()`, with `PATH` resolution, process creation (`fork()`), and status tracking (`wait() / waitpid()`).
- **Pipes & Redirections**:
  - Chains multiple commands via pipes (`|`).
  - Output redirection (`>` overwrite, `>>` append).
  - Input redirection (`<`).
  - Here-documents (`<<` multi-line reader until a delimiter).
- **Environment Variables**:
  - Variable expansion (e.g., `$VAR`).
  - Special variable `$?` tracking the exit status of the last executed pipeline.
- **Quote Management**:
  - Single quotes (`'...'`): Treats all enclosed characters literally.
  - Double quotes (`"..."`): Preserves literal value except for variable expansion (`$`).
- **Built-in Commands**:
  - `echo` (with `-n` option)
  - `cd` (with relative/absolute paths)
  - `pwd`
  - `export`
  - `unset`
  - `env`
  - `exit`
- **Signal Handling**:
  - `Ctrl+C` (`SIGINT`): Displays a new prompt on a new line.
  - `Ctrl+\` (`SIGQUIT`): Ignored in interactive mode (matching `bash` behavior).
  - `Ctrl+D` (`EOF`): Exits the shell cleanly.

---

## Usage Examples

```bash
./minishell                          # Start the shell
$> ls -la | grep .c | wc -l        # Pipes
$> echo "Hello $USER"               # Variable expansion
$> export MY_VAR=value              # Set variable
$> cd ~                              # Change directory
$> cat < file.txt                   # Input redirection
$> echo content >> file.txt         # Append output
$> cat << EOF                       # Here-document
> input line
> EOF
$> exit 0                            # Exit shell
```

---

## Tech Stack & Dependencies

- **Language:** C (C99 standard compliant)
- **Compiler Flags:** `-Wall -Wextra -Werror`
- **Libraries:** `libreadline`, `termios`
- **Core System Calls:** `fork`, `execve`, `wait/waitpid`, `pipe`, `dup2`, `unlink`, `signal`

---

## Instructions

### Requirements

- A C compiler (`gcc` or `clang`)
- `make`
- `readline` library installed on your system
- A Unix-like operating system (Linux or macOS)

### Compilation

Build the `minishell` executable from the project root:

```bash
make
```

Useful Makefile targets:
- `make`: Builds the executable.
- `make clean`: Removes object files.
- `make fclean`: Removes object files and the executable.
- `make re`: Performs a complete clean rebuild.

### Execution

Run the interactive shell:

```bash
./minishell
```

---

## Project Structure

```text
src/
├── parsing/          # Syntax analysis & tokenization
├── execution/        # Command execution & pipes
├── builtin/          # Built-in command implementations
├── utils/            # String manipulation, environment handling
└── memory/           # Memory management utilities
```

---

## Execution Flow

1. **Input Reading**: Reads user input using `readline` and appends non-empty inputs to history.
2. **Lexing & Parsing**: Breaks the raw input string into tokens, handles quotes, expands `$VAR`, and generates an executable command structure.
3. **Execution**:
   - Checks if the command is a built-in or external program.
   - Creates necessary pipes (`pipe()`) and redirects file descriptors (`dup2()`).
   - Forks child processes (`fork()`) and executes binaries (`execve()`).
4. **Cleanup**: Closes file descriptors, frees memory allocations, and collects child exit statuses.

---

## Technical Details

- **Custom Memory Cleanup**: Systematic resource freeing to prevent memory leaks across all child and parent branches.
- **Robust Parsing**: Validation of syntax errors (e.g., unclosed quotes, invalid pipe placements) before execution.
- **Multiple Pipelines**: Dynamically allocated array of file descriptors to support $N$ chained commands.
- **Heredoc Handling**: Creates temporary files or pipes to store input line-by-line until the delimiter string is matched.

---

## AI Usage Disclosure

AI tools were used in a limited and supervised capacity during this project, primarily for:
- Refining README formatting and structure.
- Reviewing documentation regarding POSIX signal behavior and process execution edge cases.

All core logic, architecture, code implementation, and debugging were performed directly by the project authors.

---

## Notes

This repository is an educational project created for the 42 school curriculum and is not intended for production environments.
