# FastFile Linker

Command line tool for stitching Call of Duty assets into fastfiles for NX1. The linker consumes a CSV manifest, streams the referenced assets into an intermediate `.ffraw`, and then emits the final fastfile container (`.ffm` by default, `.ff` when compiled with `PROTO`).
Loads rawfile entries as well as localized string tables generated from `.str` files.

`build.sh` — example cross-compilation script targeting Win32 via `i686-w64-mingw32-gcc`.
`raw_test.csv`, `patch.csv` — sample manifests alongside the assets they reference.

## Building
The project targets a C99 toolchain and has no external runtime dependencies beyond the standard library.

```sh
# native build (Linux/macOS)
gcc -std=c99 -O2 -Wall -Wextra -o linker linker.c miniz.c

# cross-compile for Windows (matches build.sh)
i686-w64-mingw32-gcc -static -static-libgcc -static-libstdc++ -O2 -Wall -Wextra -o linker.exe linker.c miniz.c
```

You can also run `./build.sh` to reproduce the Windows build configuration used by the author.

## Usage
```
./linker patch.csv
```
The basename of the CSV (`patch` in the example) determines the names of the output files (`patch.ffraw` and `patch.ffm`).
Supported asset type strings currently include `rawfile` and `localize_entry`; new handlers can be added in `asset_handlers` inside `linker.c`.

## Notes
- The codebase is still experimental; expect incomplete error handling and additional asset types to require bespoke serializers.