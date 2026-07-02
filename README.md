# peg and leg

`peg` and `leg` are tools for generating recursive-descent parsers.

## Building and Testing with Meson

This project is built using the Meson build system, providing unified building and testing across Unix-like systems (Linux, BSDs), macOS, and Windows.

### Prerequisites

You will need:
- A C compiler (GCC, Clang, MSVC, etc.)
- [Meson](https://mesonbuild.com/)
- [Ninja](https://ninja-build.org/)

### Quick Start

1. **Configure the build directory:**
   ```bash
   meson setup build
   ```

2. **Compile the executables and examples:**
   ```bash
   meson compile -C build
   ```

3. **Run the test suite:**
   ```bash
   meson test -C build
   ```

---

## Bootstrapping and Regenerating Source Files

`peg` and `leg` are self-hosting compiler tools. This means the parser compilers are defined in their own grammar files ([src/peg.peg](file:///home/desiderantes/CLionProjects/peg/src/peg.peg) and [src/leg.leg](file:///home/desiderantes/CLionProjects/peg/src/leg.leg)) and compile themselves.

To break the chicken-and-egg bootstrap cycle, a pre-compiled version of the C parsers ([src/peg.peg-c](file:///home/desiderantes/CLionProjects/peg/src/peg.peg-c) and [src/leg.c](file:///home/desiderantes/CLionProjects/peg/src/leg.c)) is tracked in the repository.

If you modify the grammar definitions (`.peg` or `.leg`) in `src/`, you can use the newly compiled local binaries to regenerate and update these bootstrap source files inside the `src/` directory by running:

```bash
meson compile -C build new
```

This will run the built compiler executables on their respective grammar sources and write the regenerated C files back to the source tree.

---

## Configuration Options

You can configure options during setup or using `meson configure`.

### Emacs Support
An option is provided to install the Emacs editor mode (`leg-mode.el`):
* Enable Emacs Lisp installation (defaults to `false`):
  ```bash
  meson setup build -Demacs=true
  ```

---

## Installation

To install `peg`, `leg`, the `peg.1` man page, and optional files (like the Emacs mode) to your system:
```bash
meson install -C build
```
*(You may need `sudo` depending on your configured `prefix`.)*

To uninstall the files, you can use:
```bash
ninja -C build uninstall
```

---

## Compatibility Layers

Local implementations of `getopt()` and `basename()` are provided in the `win/` directory. Meson automatically detects and links these compatibility layers when compiling on Windows.
