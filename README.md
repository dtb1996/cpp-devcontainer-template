# C++ Development Environment

A reusable C++ development environment built around **VS Code Dev Containers**, **Ubuntu 24.04**, **CMake**, **Ninja**, and the **Clang tooling suite**.

The project is designed for C++ practice and development with a focus on a clean, reproducible development environment and integrated testing.

## Development Environment

The development environment runs inside a Docker-based VS Code Dev Container.

### Toolchain

* Ubuntu 24.04
* C++20
* GCC
* Clang 18
* CMake
* Ninja
* GDB
* LLDB
* Git
* Python 3
* pkg-config

### Clang Tooling

* **clangd** — C++ language server and IDE features
* **clang-format** — automatic code formatting
* **clang-tidy** — static analysis and code-quality checks

The container uses a non-root `dillon` user with UID/GID `1000`, matching the development host.

## Project Structure

```text
.
├── .devcontainer/
│   ├── Dockerfile
│   └── devcontainer.json
├── .vscode/
│   └── settings.json
├── include/
├── src/
│   └── main.cpp
├── tests/
│   └── main_test.cpp
├── build/
├── CMakeLists.txt
├── CMakePresets.json
├── .clang-format
├── .clang-tidy
├── .clangd
└── .gitignore
```

The `build/` directory contains generated CMake/Ninja files and should not be committed.

## CMake Workflow

The project uses CMake presets for consistent configuration.

### Configure

Configure the Debug build:

```bash
cmake --preset debug
```

Configure the Release build:

```bash
cmake --preset release
```

### Build

Build the Debug configuration:

```bash
cmake --build --preset debug
```

Build the Release configuration:

```bash
cmake --build --preset release
```

If nothing has changed, Ninja will report:

```text
ninja: no work to do.
```

## Testing Workflow

Tests are run through **CTest**.

Run the Debug test suite:

```bash
ctest --preset debug
```

A successful run should report:

```text
100% tests passed, 0 tests failed
```

After making code changes, the normal workflow is:

```bash
cmake --build --preset debug
ctest --preset debug
```

When adding new tests, place them in the `tests/` directory and register them through CMake.

## Clang Workflow

### clangd

`clangd` provides:

* Code completion
* Go-to-definition
* Find references
* Diagnostics
* Semantic highlighting
* Inlay hints
* Background indexing
* clang-tidy integration

CMake generates a compilation database at:

```text
build/debug/compile_commands.json
```

A repository-level symlink makes it available at:

```text
compile_commands.json
```

This allows clangd to understand the project's actual compiler flags, include paths, and C++ standard.

To manually verify clangd:

```bash
clangd --check=src/main.cpp
```

A successful check ends with:

```text
All checks completed, 0 errors
```

### clang-format

Formatting is configured through `.clang-format`.

VS Code runs formatting automatically when a file is saved:

```json
"editor.formatOnSave": true
```

The clangd extension is used as the default formatter.

To manually check formatting without modifying a file:

```bash
clang-format --dry-run --Werror src/main.cpp
```

To view the formatted output:

```bash
clang-format src/main.cpp
```

### clang-tidy

clang-tidy provides static analysis and additional C++ diagnostics.

For a simple manual check:

```bash
clang-tidy src/main.cpp -- -std=c++20
```

The project also enables clang-tidy through the clangd configuration.

## Typical Development Loop

For normal development, most work should happen directly through VS Code.

1. Open the project in the Dev Container.

2. Edit C++ source files.

3. Save - clang-format runs automatically.

4. Fix clangd/clang-tidy diagnostics as appropriate.

5. Build:

   ```bash
   cmake --build --preset debug
   ```

6. Run tests:

   ```bash
   ctest --preset debug
   ```

For a clean rebuild:

```bash
rm -rf build
cmake --preset debug
cmake --build --preset debug
ctest --preset debug
```

## Current Status

The development environment currently provides:

* Reproducible containerized C++ environment
* C++20 toolchain
* CMake + Ninja
* Debug and Release configurations
* clangd integration
* Automatic clang-format
* clang-tidy static analysis
* GDB/LLDB debugging
* CTest-based testing
* VS Code integration
