# A Rust library for robust domain name parsing using the Public Suffix List

[![Build Status](https://travis-ci.org/rushmorem/publicsuffix.svg?branch=master)](https://travis-ci.org/rushmorem/publicsuffix) [![Latest Version](https://img.shields.io/crates/v/publicsuffix.svg)](https://crates.io/crates/publicsuffix) [![Docs](https://docs.rs/publicsuffix/badge.svg)](https://docs.rs/publicsuffix)

This library allows you to utilise the [Public Suffix List](https://publicsuffix.org) using [Rust](https://www.rust-lang.org).

## Dependancies

This library uses [hyper](http://hyper.rs) to fetch the public suffix list via HTTPS. As such you will need to have the following libraries on your system in order to build it:-
* gcc
* openssl
* make
* cmake
* zlib

## Examples

```rust
extern crate publicsuffix;

use publicsuffix::List;

let list = List::fetch()?;

// Using the list you can find out the root domain
// or extension of any given domain name
let domain = list.parse_domain("www.example.com")?;
assert_eq!(domain.root(), Some("example.com"));
assert_eq!(domain.suffix(), Some("com"));

let domain = list.parse_domain("www.食狮.中国")?;
assert_eq!(domain.root(), Some("食狮.中国"));
assert_eq!(domain.suffix(), Some("中国"));

let domain = list.parse_domain("www.xn--85x722f.xn--55qx5d.cn")?;
assert_eq!(domain.root(), Some("xn--85x722f.xn--55qx5d.cn"));
assert_eq!(domain.suffix(), Some("xn--55qx5d.cn"));

let domain = list.parse_domain("a.b.example.uk.com")?;
assert_eq!(domain.root(), Some("example.uk.com"));
assert_eq!(domain.suffix(), Some("uk.com"));

// You can also find out if this is an ICANN domain
assert!(!domain.is_icann());

// or a private one
assert!(domain.is_private());

// In any case if the domain's suffix is in the list
// then this is definately a registrable domain name
assert!(domain.has_known_suffix());
```
