# Debugging Rust

A collection of notes for debugging Rust.

---

## Visual Studio Code

### Extensions

- [**Rust** (rust-lang.rust)](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust) for development with **Rust**
- [**CodeLLDB** (vadimcn.vscode-lldb)](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb) for debugging with **LLDB**

### Configuration

To setup debugging configurations in **Visual Studio Code**, navigate to *Run* -> *Open Configurations*.

The following configurations define a two-step debugging process:
1. The `cargo` tool is used to compile the code
2. The output of the compile step is run in the debugger

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
            // Arguments for the application here
            // e.g. "--nocapture", or a filter to run specific tests
            "args": [],
            // Environment variables for the application here
            // e.g. "RUST_LOG": "trace"
            "env": {},
            "cwd": "${workspaceRoot}",
        },
    ]
}
```

### References

- [Comment on rust-lang/cargo issue #6821, "How to debug Cargo tests (with CLI or IDE)"](https://github.com/rust-lang/cargo/issues/6821#issuecomment-479983260)
- [Answer on Stack Overflow, "Step by step interactive debugger for Rust?"](https://stackoverflow.com/a/52273254)
- [Forrest Smith, "How to Debug Rust with Visual Studio Code"](https://forrestthewoods.com/blog/how-to-debug-rust-with-visual-studio-code/)