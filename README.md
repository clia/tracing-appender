# clia-tracing-appender

This is a personal temporary tracing-appender, support symlinking latest log file and local offset time format.

It has no relation to [tracing-appender](https://crates.io/crates/tracing-appender).

## Usage example

Cargo.toml:

```toml
clia-local-offset = "0.1"
time = { version = "0.3", features = ["macros"] }
tracing-subscriber = { version = "0.3", features = ["time", "local-time"] }
tracing-appender = { package = "clia-tracing-appender", version = "0.2" }
```

Rust:

```rust
use time::macros::format_description;
use tracing_appender::non_blocking::WorkerGuard;
use tracing_subscriber::fmt;
use tracing_subscriber::fmt::time::OffsetTime;

let file_appender = tracing_appender::rolling::hourly(directory, file_name);
let (file_writer, guard) = tracing_appender::non_blocking(file_appender);

let offset = clia_local_offset::current_local_offset()
    .expect("Can not get local offset!");
let timer = OffsetTime::new(
    offset,
    format_description!("[year]-[month]-[day] [hour]:[minute]:[second].[subsecond digits:3]"),
);

tracing_subscriber::fmt()
    .with_timer(timer)
    .with_writer(file_writer)
    .init();
```

## Changelog

Version 0.2.5

- Merge `tracing-appender` v0.1.x branch date 2022-10-22.
- Change usage of `clia-time` to `clia-local-offset`, simplify the example.
