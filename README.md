# minilog

`minilog` is a small, single-header C++ logging library. It provides level-based console logging, source-location-aware messages, `std::format` formatting, optional file output, and ANSI-colored output on Linux and macOS.

## Features

- Header-only library: include `minilog.hpp`.
- Type-checked `std::format`-style log messages.
- Call-site file and line capture through `std::source_location`.
- Runtime console log-level filtering.
- Optional append-mode file logging.
- ANSI colors for supported terminals on Linux and macOS.

## Requirements

- CMake 4.3.2 or newer for the included demo project.
- A C++ toolchain that accepts C++26 mode and supports the standard library features used by `minilog`, including `<format>`, `<source_location>`, and `std::chrono::zoned_time`.

## Quick start

Build and run the demo executable:

```sh
cmake -S . -B out/Release -DCMAKE_BUILD_TYPE=Release
cmake --build out/Release
./out/Release/main
```

The demo target is named `main` and is built from `main.cpp`.

## Use in your project

Copy `minilog.hpp` into your include path and include it from C++ source files:

```cpp
#include "minilog.hpp"

int main() {
  minilog::log_info("hello, the answer is {}", 42);

  minilog::set_log_level(minilog::log_level::trace);

  int answer = 42;
  MINILOG_P(answer);
}
```

If you use CMake, add the directory containing `minilog.hpp` to your target include paths:

```cmake
target_include_directories(your_target PRIVATE path/to/minilog)
```

## Logging API

`minilog` provides one function per log level:

```cpp
minilog::log_trace("message");
minilog::log_debug("message");
minilog::log_info("message");
minilog::log_critical("message");
minilog::log_warn("message");
minilog::log_error("message");
minilog::log_fatal("message");
```

Each logging function accepts a `std::format` format string followed by format arguments:

```cpp
minilog::log_warn("user {} failed after {} attempts", username, attempts);
```

`MINILOG_P(value)` logs a variable name and value at `debug` level:

```cpp
int count = 7;
MINILOG_P(count); // count=7
```

## Log levels

The available levels are ordered as follows:

```text
trace < debug < info < critical < warn < error < fatal
```

Console output is filtered by the current threshold. Messages whose level is greater than or equal to the threshold are printed. The default threshold is `info`.

Set the threshold at runtime:

```cpp
minilog::set_log_level(minilog::log_level::debug);
```

## File logging

Enable append-mode file logging at runtime:

```cpp
minilog::set_log_file("mini.log");
```

When a log file is configured, log messages are appended to that file. Console output remains controlled by the current log-level threshold.

## Environment variables

`minilog` can be configured before program startup with environment variables:

| Variable | Description |
| --- | --- |
| `MINILOG_LEVEL` | Initial console threshold. Accepted values: `trace`, `debug`, `info`, `critical`, `warn`, `error`, `fatal`. Unknown values use `info`. |
| `MINILOG_FILE` | Path to a file that receives appended log messages. |

Example:

```sh
MINILOG_LEVEL=debug MINILOG_FILE=mini.log ./out/Release/main
```

## Output format

Each log line contains the timestamp, source file, source line, level, and formatted message:

```text
<timestamp> <file>:<line> [<level>] <message>
```

Example:

```text
2026-05-04 19:52:56.775840203 CST main.cpp:4 [info] hello, the answer is 42
```

## Project layout

```text
CMakeLists.txt  CMake demo build configuration
main.cpp        Demo program
minilog.hpp     Header-only logging library
```

## License

No license file is included in this repository.
