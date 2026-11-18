# Building the scanner core on Windows

How to build the scanner core's Windows x64 libraries (`libclamav.dll`,
`libclammspack.dll`, `libclamunrar.dll`, `libclamunrar_iface.dll`) from this
repository. These are the exact settings of the released builds; each release
names its tag of this repository. For other platforms and options, see
ClamAV's [INSTALL.md](INSTALL.md).

## Toolchain

- Visual Studio 2022 C/C++ (MSVC 14.51), CMake and Ninja
- Rust (`cargo` and `rustc`; 1.99.1 for tag `v3`)

## Libraries

Built separately and used as DLLs:

| Library | Version | DLL |
|---|---|---|
| OpenSSL | 3.6.5 | `libcrypto-3-x64.dll`, `libssl-3-x64.dll` |
| json-c | 0.18 | `json-c.dll` |
| libxml2 | 2.15.0 | `libxml2.dll` |
| PCRE2 | 10.46 | `pcre2-8.dll` |
| bzip2 | 1.0.8 | `libbz2.dll` |
| zlib | 1.3.1 | `zlib.dll` |
| Pthreads4w | 3.0.2 | `pthreadVC3.dll` |

They come from a [Mussels](https://github.com/Cisco-Talos/Mussels) install (the
dependency builder ClamAV documents for Windows); any build of the same
versions works.

## Configure and build

From a Visual Studio x64 developer prompt, in a build folder next to this
source tree, with `<deps>` the folder the libraries above are installed to:

```
cmake ..\wsdk-scanner-core -G Ninja ^
  -DCMAKE_BUILD_TYPE=Release ^
  -DENABLE_LIBCLAMAV_ONLY=ON -DENABLE_TESTS=OFF -DENABLE_APP=OFF ^
  -DCMAKE_PREFIX_PATH=<deps> -DOPENSSL_ROOT_DIR=<deps> ^
  -DJSONC_INCLUDE_DIR=<json-c headers> -DJSONC_LIBRARY=<json-c import library> ^
  -DLIBXML2_INCLUDE_DIR=<libxml2 headers> ^
  "-DCMAKE_C_FLAGS_RELEASE=/O2 /Ob2 /DNDEBUG /Zi" ^
  "-DCMAKE_CXX_FLAGS_RELEASE=/O2 /Ob2 /DNDEBUG /Zi" ^
  "-DCMAKE_SHARED_LINKER_FLAGS_RELEASE=/INCREMENTAL:NO /DEBUG /OPT:REF /OPT:ICF /PDBALTPATH:%_PDB%"
cmake --build .
```

`JSONC_INCLUDE_DIR`, `JSONC_LIBRARY` and `LIBXML2_INCLUDE_DIR` are set
explicitly because ClamAV's CMake doesn't find them otherwise. The compiler and
linker flags only add debug information (PDB files); `/PDBALTPATH` keeps the
build machine's path out of the DLLs.

The build produces the four DLLs, each with its PDB. Released builds are signed
afterwards; signing doesn't change the code.
