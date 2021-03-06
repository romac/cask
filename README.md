# Cask

A fast key-value store written in Rust. The underlying storage system is a log-structured hash table
which is inspired by [bitcask](https://github.com/basho/bitcask/).

[![Build Status](https://travis-ci.org/andresilva/cask.svg?branch=master)](https://travis-ci.org/andresilva/cask)
[![Crates.io](https://img.shields.io/crates/v/cask.svg?maxage=2592000)](https://crates.io/crates/cask)
[![License](https://img.shields.io/dub/l/vibe-d.svg)](https://raw.githubusercontent.com/andresilva/cask/master/LICENSE)

[API Documentation](http://andresilva.github.io/cask)

* * *

**WARNING**: ⚠️ Please do not trust any valuable data to this yet. ⚠️

## Installation

Use the [crates.io](http://crates.io/) repository, add this to your Cargo.toml along with the rest
of your dependencies:

```toml
[dependencies]
cask = "0.7.0"
```

Then, use `Cask` in your crate:

```rust
extern crate cask;
use cask::{CaskOptions, SyncStrategy};
```

## Usage

The basic usage of the library is shown below:

```rust
extern crate cask;

use std::str;
use cask::{CaskOptions, SyncStrategy};
use cask::errors::Result;

fn main() {
    if let Err(e) = example() {
        println!("{:?}", e);
    }
}

fn example() -> Result<()> {
    let cask = CaskOptions::default()
        .compaction_check_frequency(1200)
        .sync(SyncStrategy::Interval(5000))
        .max_file_size(1024 * 1024 * 1024)
        .open("cask.db")?;

    let key = "hello";
    let value = "world";

    cask.put(key, value)?;

    let v = cask.get(key)?;
    println!("key:{},value:{}", key, str::from_utf8(&v.unwrap()).unwrap());

    cask.delete(key)?;
    Ok(())
}
```

## TODO

- [X] Basic error handling
- [X] Merge files during compaction
- [X] Configurable compaction triggers and thresholds
- [X] Documentation
- [ ] Tests
- [ ] Benchmark
- [ ] Handle database corruption

## License

cask is licensed under the [MIT](http://opensource.org/licenses/MIT) license. See `LICENSE` for
details.
