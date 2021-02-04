# Debugging Rust

A collection of notes for debugging [**Rust**](https://www.rust-lang.org/) applications and tests.

---

## Command Line with Rust Toolchain

The **Rust** toolchain includes:
- `rust-lldb` for debugging with [**LLDB**](https://lldb.llvm.org/)
- `rust-gdb` for debugging with [**GDB**](https://www.gnu.org/software/gdb/)

1. Compile using the `cargo` tool, with the `--message-format=json` option to output information including the executable path

    ```bash
    # Compile application
    cargo build --message-format=json
    # Compile tests
    cargo test --no-run --message-format=json
    ```

    The executable path is typically found beside the `executable` tag in the final line of the output.

    See `cargo` tool help for additional compile options:
    ```bash
    cargo build -h
    cargo help build
    cargo test -h
    cargo help test
    ```

2. Debug the compiled executable using the appropriate debugging tool

    ```bash
    # Debug with LLDB
    rust-lldb -f path/to/executable
    # Debug with GDB
    rust-gdb path/to/executable
    ```

    See debugging tool help for additional options (e.g. passing arguments, using core files, attaching to processes, etc...):
    ```bash
    # Help for LLDB
    rust-lldb -h
    # Help for GDB
    rust-gdb -h
    ```

---

## Visual Studio Code

### Extensions

- [**Rust** (rust-lang.rust)](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust) for development with **Rust**
- [**CodeLLDB** (vadimcn.vscode-lldb)](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb) for debugging with **LLDB**

### Configuration

To setup debugging configurations in **Visual Studio Code**, navigate to *Run* -> *Open Configurations*.

The following configurations defines a two-step debugging process (corresponding to the command line method above):
1. Compile using the `cargo` tool
2. Debug the compiled executable (determined from the output of the `cargo` tool)

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "lldb",
            "request": "launch",
            "name": "Debug application",
            // The cargo tool compiles the application
            "cargo": {
                // Arguments for the cargo tool
                "args": [
                    "build",
                ],
            },
            // Arguments for the application
            "args": [],
            // Environment variables for the application
            // e.g. "RUST_LOG": "trace"
            "env": {},
            "cwd": "${workspaceRoot}",
        },
        {
            "type": "lldb",
            "request": "launch",
            "name": "Debug tests",
            // The cargo tool compiles the tests
            "cargo": {
                // Arguments for the cargo tool
                "args": [
                    "test",
                    "--no-run",
                ],
            },
            // Arguments for the application
            // e.g. "--nocapture", or a filter to run specific tests
            "args": [],
            // Environment variables for the application
            // e.g. "RUST_LOG": "trace"
            "env": {},
            "cwd": "${workspaceRoot}",
        },
    ]
}
```

### References

- [Visual Studio Code Documentation - Debugging](https://code.visualstudio.com/docs/editor/debugging)
- [Comment on rust-lang/cargo issue #6821, "How to debug Cargo tests (with CLI or IDE)"](https://github.com/rust-lang/cargo/issues/6821#issuecomment-479983260)
- [Answer on Stack Overflow, "Step by step interactive debugger for Rust?"](https://stackoverflow.com/a/52273254)
- [Forrest Smith, "How to Debug Rust with Visual Studio Code"](https://forrestthewoods.com/blog/how-to-debug-rust-with-visual-studio-code/)