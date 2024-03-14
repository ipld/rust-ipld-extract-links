**Deprecated**: This crate was moved into [`ipld-core`] as module, hence please use `ipld-core` >= v0.3.2 instead.

IPLD extract links
==================

[![Crates.io](https://img.shields.io/crates/v/ipld-extract-links.svg)](https://crates.io/crates/ipld-extract-links)
[![Documentation](https://docs.rs/ipld-extract-links/badge.svg)](https://docs.rs/ipld-extract-links)

This crate allow extracting links (CIDs) from `serde_ipld_dag*` formats like [`serde_ipld_dagcbor`] or [`serde_ipld_dagjson`].

Usage
-----

The link extractor is independent of the deserializer, hence you need to create one first. Therefore this example depends on [`serde_json`] as well as [`serde_ipld_dagjson`].

```rust
use cid::Cid;
use ipld_extract_links::ExtractLinks;
use serde::Deserialize;

pub fn main() {
    let slice = br#"{"hello": "world!", "other": {"/": "bafkreibme22gw2h7y2h7tg2fhqotaqjucnbc24deqo72b6mkl2egezxhvy" }}"#;
    let mut json_deserializer = serde_json::Deserializer::from_slice(slice);
    let deserializer = serde_ipld_dagjson::Deserializer::new(&mut json_deserializer);
    let extracted_links = ExtractLinks::deserialize(deserializer).unwrap().into_vec();
    assert_eq!(
        extracted_links,
        vec![
            Cid::try_from("bafkreibme22gw2h7y2h7tg2fhqotaqjucnbc24deqo72b6mkl2egezxhvy").unwrap(),
        ]
    );
}

```

License
-------

Licensed under either of

 * Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

[`serde_ipld_dagcbor`]: https://crates.io/crates/serde_ipld_dagcbor
[`serde_ipld_dagjson`]: https://crates.io/crates/serde_ipld_dagjson
[`ipld-core`]: https://crates.io/crates/ipld-core
[`serde_json`]: https://crates.io/crates/serde_json
