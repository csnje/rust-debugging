# Debugging Rust

A collection of notes for debugging [**Rust**](https://www.rust-lang.org/) applications and tests.

---

## Using the Rust Toolchain on the Command Line

### Tools

The **Rust** toolchain includes:
- `rust-lldb` for debugging with [**LLDB**](https://lldb.llvm.org/)
- `rust-gdb` for debugging with [**GDB**](https://www.gnu.org/software/gdb/)

### Process

1. Build with `cargo` using the option `--message-format json` to output messages that include the path of the compiled executable:

    ```bash
    # Build application
    cargo build --message-format json
    # Build tests
    cargo test --message-format json --no-run
    ```

    The path of the executable is given by the value associated with the `executable` tag in the output.

2. Debug the compiled executable using the preferred debugging tool:

    ```bash
    # Debug with LLDB
    rust-lldb path/to/executable
    # Debug with GDB
    rust-gdb path/to/executable
    ```

    Refer to the debugging tool help option `-h` for additional usage, including how to provide arguments to the executable being debugged.

---

## Using Visual Studio Code

### Extensions

The following extensions are used here:

- [**Rust** (rust-lang.rust)](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust) for development with **Rust**
- [**CodeLLDB** (vadimcn.vscode-lldb)](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb) for debugging with **LLDB**

### Launch Configurations

To create and modify the [**Visual Studio Code**](https://code.visualstudio.com/) [launch configurations](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations) file, navigate to *Run* -> *Open Configurations* (or *Run* -> *Add Configuration...* on first use).

The following defines a two-step debugging process corresponding to the command line technique described above that will:
1. Compile using the `cargo` tool (defined by the `cargo` tag), and
2. Debug the compiled executable using information extracted (automatically) from the output of the `cargo` tool

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
                // A filter discriminates the application in a workspace (if needed)
                //"filter": {
                //    "name": "myapp",
                //    "kind": "bin",
                //},
            },
            // Arguments for the application
            "args": [],
            // Environment variables for the application
            // e.g. { "RUST_LOG": "trace", etc... }
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
                // A filter discriminates the application/library in a workspace (if needed)
                //"filter": {
                //    "name": "mylib",
                //    "kind": "lib",
                //},
            },
            // Arguments for the tests
            // e.g. [ "--nocapture", "testfilter", etc...]
            "args": [],
            // Environment variables for the tests
            // e.g. { "RUST_LOG": "trace", etc... }
            "env": {},
            "cwd": "${workspaceRoot}",
        },
    ]
}
```

Note that inline comments are allowed within the **json** formatted launch configuration file in **Visual Studio Code**.

---

## References

- [Comment on rust-lang/cargo issue #6821, "How to debug Cargo tests (with CLI or IDE)"](https://github.com/rust-lang/cargo/issues/6821#issuecomment-479983260)
- [Comment on rust-lang/cargo issue #8525, "\`cargo test --no-run\` doesn't appear to produce a binary"](https://github.com/rust-lang/cargo/issues/8525#issuecomment-662116135)
- ["Debugging Rust with rust-lldb"](https://dev.to/bmatcuk/debugging-rust-with-rust-lldb-j1f)
- ["How to Debug Rust with Visual Studio Code"](https://forrestthewoods.com/blog/how-to-debug-rust-with-visual-studio-code/)
- [Answer on Stack Overflow, "Step by step interactive debugger for Rust?"](https://stackoverflow.com/a/52273254)
