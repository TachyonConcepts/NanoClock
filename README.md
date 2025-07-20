# 🕒 nano-clock

**Ultra-fast date/time formatting for HTTP headers and system profiling.**  
Written in Rust, sprinkled with inline assembly, optimized for the lowest latency possible.

---

## 🔥 Features

- Returns current date/time in `(i32, u8, u8, u8, u8, u8, u8)` tuple format  
  (`year, month, day, hour, minute, second, weekday`)
- Blazing-fast formatting of HTTP date headers (`Date: ... GMT`) in preallocated buffers
- Optional inline assembly (`nano_clock_asm`) for even lower latency
- Includes both `time()` (wall-clock) and `clock_gettime(CLOCK_MONOTONIC)` for monotonic measurements
- Zero allocations, no `String`, no `chrono`, no nonsense.

---

## 📦 Example

```rust
use nano_clock::{nano_clock, nano_http_date};

fn main() {
    let (y, m, d, h, min, s, w) = nano_clock();
    println!("Now: {y:04}-{m:02}-{d:02} {h:02}:{min:02}:{s:02} (weekday: {w})");

    let mut buf = [0u8; 35];
    unsafe {
        nano_http_date(&mut buf, false);
    }
    println!("{}", std::str::from_utf8(&buf).unwrap());
}
