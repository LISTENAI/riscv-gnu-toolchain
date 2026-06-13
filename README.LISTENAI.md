# LISTENAI RISC-V GNU Toolchain (macOS)

Prebuilt bare-metal (newlib) RISC-V GNU toolchain for macOS, based on the
[Nuclei](https://github.com/riscv-mcu/riscv-gnu-toolchain) `nuclei/2025.10`
toolchain. Released for both Apple Silicon (arm64) and Intel (x86_64).

The feature set is kept in line with the official Nuclei toolchain: GDB is
built without Python scripting, NLS, or lzma/zstd debug-section support, so the
two builds are identical and depend on as little as possible.

## Runtime dependencies (Homebrew)

`gcc` and `binutils` are self-contained. Only `gdb` links two
[Homebrew](https://brew.sh) libraries (gmp and mpfr), so install them before
using the debugger:

```sh
brew install gmp mpfr
```

Without them, `gdb` fails to start with a `Library not loaded` (dyld) error.
(`gdb` also uses the curses library, but that ships with macOS, so nothing else
is needed.)

| component | Homebrew libraries it links |
|-----------|-----------------------------|
| `gcc`, `g++`, `cc1`, … | none — self-contained |
| `as`, `ld`, `objdump`, … (binutils) | none — self-contained |
| `gdb` | `gmp`, `mpfr` |

This works on a Mac with Homebrew at the standard prefix (`/opt/homebrew` on
Apple Silicon, `/usr/local` on Intel); `brew install` puts the libraries where
the binaries look for them, so the same command works on both architectures.

## Usage

```sh
tar xzf riscv-gnu-toolchain-macos-<arch>-<version>.tar.gz
export PATH="$PWD/riscv-gnu-toolchain-macos-<arch>-<version>/bin:$PATH"
riscv64-unknown-elf-gcc --version
```

## Provenance

- Based on Nuclei `nuclei/2025.10`.
- All submodules track the upstream Nuclei (`riscv-mcu`) forks; binutils and gdb
  are built `--with-system-zlib` to use the macOS system zlib (the bundled copy
  no longer compiles under clang's C23 default).
