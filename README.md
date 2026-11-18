<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://watchdog.dev">
    <img src="/docs/logo.png" alt="Logo" width="40%" height="40%">
  </a>

  <h3 align="center">WSDK Scanner Core</h3>

  <p align="center">
    A simple interface, rich signature format antivirus SDK with cloud signature server database
  </p>
</p>

Clamav windows HANDLE type scan improvement. In this version of libclamav, cl_scan function is extended to accept HANDLE type of variables.
That will make easy to make a scan using the return value of CreateFile or equvielent. The clamav version is 1.5.4.

## Installation
Please replace current libclamav installation with wsdk scanner core directory.

### Usage
```cpp
const char *virname;
unsigned long size = 0;
struct cl_scan_options options;

const char *path = "PLACE PATH HERE";
HANDLE handle = CreateFile(path,
                           GENERIC_READ | GENERIC_WRITE,
                           0,
                           NULL,
                           CREATE_NEW,
                           FILE_ATTRIBUTE_NORMAL,
                           NULL);   
int result = cl_scanhandle(handle, path, &virname, &size, engine, &options);
```

## Building
[BUILD-WINDOWS.md](BUILD-WINDOWS.md) is the exact recipe for the Windows build.

## Licensing
WSDK Scanner Core is a modified version of [ClamAV](https://github.com/Cisco-Talos/clamav) and, like ClamAV, is licensed
under the **GNU General Public License, version 2** ([COPYING.txt](COPYING.txt)); the licenses of the code ClamAV
includes are in [COPYING](COPYING). ClamAV is Copyright (C) Cisco Systems, Inc. and its contributors.

The code WatchdogDevelopment LLC added (the `cl_scanhandle` and `cl_scanhandle_callback` scan entry points) may also be
used on its own under the [Boost Software License 1.0](LICENSE). That does not change the license of the rest: the
scanner core as a whole, and anything built from it, is GPL-2.0.
