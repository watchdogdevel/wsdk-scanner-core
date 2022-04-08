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
That will make easy to make a scan using the return value of CreateFile or equivalent. The clamav version is 0.103.0

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

## Licensing
This project is licensed under the [Boost Software License - Version 1.0](https://github.com/watchdogdevel/wsdk-scanner-core/blob/master/LICENSE).
