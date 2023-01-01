# Debugging Rust

A collection of notes for debugging [**Rust**](https://www.rust-lang.org/) applications and tests.

---

## With the Rust toolchain (on the command line)

### Tools

The **Rust** toolchain includes:
- `rust-lldb` for debugging with [**LLDB**](https://lldb.llvm.org/)
- `rust-gdb` for debugging with [**GDB**](https://www.gnu.org/software/gdb/)

### Process

1. Build the executable using the `cargo build` option `--message-format json`:

   ```bash
   # build application
   cargo build --message-format json
   # or build tests
   cargo test --message-format json --no-run
   ```
    
   The full path of the built executable appears in the `"executable"` value of the output.
   The executable will typically be in the project's `target/debug` folder but may be elsewhere, depending on the configuration.

2. Debug the executable using the preferred tool:

   ```bash
   # LLDB
   rust-lldb path/to/executable
   # or GDB
   rust-gdb path/to/executable
   ```

   Refer to the debugging tool help option `-h` for additional usage, including how to provide arguments to the executable.

---

## With Visual Studio Code

### Extensions

The following [**Visual Studio Code**](https://code.visualstudio.com/) extensions are used here:

- [**rust-analyzer** (rust-lang.rust-analyzer)](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer) for development with **Rust** *(supersedes deprecated ~~[**Rust** (rust-lang.rust)](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust)~~)*
- [**CodeLLDB** (vadimcn.vscode-lldb)](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb) for debugging with **LLDB**

### Launch Configurations

Define [launch configurations](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations) to build and run or debug an application or tests in the file `.vscode/launch.json`.

Create launch configurations for debugging by selecting *Run* → *Add configuration...* → *LLDB*.

The following example will build and debug an application or tests:

```text
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

Here the path of the executable is (automatically) extracted from the build step output of the `cargo` tool.

Note that inline comments are allowed within the **JSON** formatted launch configurations.
