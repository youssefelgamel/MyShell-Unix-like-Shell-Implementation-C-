# myShell

A custom Unix shell implemented in C as part of an Operating Systems course project. The shell supports command execution, I/O redirection, pipes, background processes, a command history system, and built-in commands.

---

## Project Structure

```
myShell/
├── myShell.c       # Main shell loop and command dispatcher
├── builtins.c      # Built-in commands: cd, pwd
├── builtins.h
├── history.c       # Command history management
├── history.h
├── signals.c       # Signal handling setup and restore
├── signals.h
└── Makefile       
```

---

## Features

### Command Execution
Forks a child process and uses `execvp()` to run any external command available on the system. Unrecognized commands produce a clear error message.

### Built-in Commands

| Command | Description |
|---------|-------------|
| `cd [path]` | Change the current working directory. `cd` alone goes to `$HOME`; `cd -` returns to the previous directory. |
| `pwd` | Print the current working directory using `getcwd()`. |
| `exit` | Terminate the shell. |
| `history` | Display a numbered list of previously entered commands (up to 100). |
| `!n` | Re-run command number `n` from history. |

### I/O Redirection
Supports both output redirection (`>`) and input redirection (`<`) using `open()`, `dup2()`, and `close()`.

```sh
myShell> ls > output.txt
myShell> sort < unsorted.txt
```

### Pipes
Connects two commands via a pipe using `pipe()`, `fork()`, and `dup2()`, so the standard output of the first command feeds into the standard input of the second.

```sh
myShell> ls | grep .c
```

### Background Execution
Append `&` to any command to run it in the background. The shell prints the background PID(s) and immediately returns to the prompt.

```sh
myShell> sleep 10 &
[Background PID]: 4821
```

### Signal Handling
The shell itself ignores `SIGINT` (Ctrl+C) and `SIGTSTP` (Ctrl+Z) so it cannot be accidentally interrupted. Child processes have their default signal behavior restored before `exec`.

### Command History
All entered commands are stored in a circular buffer (up to 100 entries). Use `history` to view them and `!n` to replay any entry by number.

---

## Building

```bash
gcc -o myShell myShell.c builtins.c history.c signals.c
```

Or with a Makefile:

```bash
make
```

---

## Usage

```bash
./myShell
```

The prompt `myShell> ` will appear. Type any command as you would in a standard shell.

---
