# Minishell

This project has been created as part of the 42 curriculum by "norabino" - Noé RABINOVICI and "lucmansa" Lucas Mansart.

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
./minishell                         # Start the shell
$> ls -la | grep .c | wc -l         # Pipes
$> echo "Hello $USER"               # Variable expansion
$> export MY_VAR=value              # Set variable
$> cd ~                             # Change directory
$> cat < file.txt                   # Input redirection
$> echo content >> file.txt         # Append output
$> cat << EOF                       # Here-document
> input line
> EOF
$> exit 0                           # Exit shell
