Claxon
======

A FLAC decoding library in Rust.

[![Build Status][ci-img]][ci]
[![Crates.io version][crate-img]][crate]
[![Documentation][docs-img]][docs]

Many media players crash on corrupted input (not FLAC in particular). This is
bad, the decoder should signal an error on invalid input, it should not crash.
I suspect that this is partly due to the fact that most decoders are written in
C. I thought I'd try and write a decoder in a safe language: Rust. Video codecs
can be quite complex, and nowadays CPU decoding is not all that common any more.
Therefore, I decided to first try and write a decoder for an audio codec that I
love and use on a daily basis: FLAC.

Example
-------
The following example computes the root mean square (RMS) of a FLAC file:

```rust
let mut reader = claxon::FlacReader::open("testsamples/pop.flac").unwrap();
let mut sqr_sum = 0.0;
let mut count = 0;
for sample in reader.samples() {
    let s = sample.unwrap() as f64;
    sqr_sum += s * s;
    count += 1;
}
println!("RMS is {}", (sqr_sum / count as f64).sqrt());
```

More examples can be found in the examples directory. For a simple example
of decoding a FLAC file to wav with Claxon and [Hound][hound], see
[decode_simple.rs](examples/decode_simple.rs). A more efficient way
of decoding requires dealing with a few details of the FLAC format.
See [decode.rs](examples/decode.rs) for an example.

Performance
-----------

These are the times to decode 5 real-world FLAC files to wav, average and
standard deviation of 11 runs, normalized to version 1.3.2 of the [reference
implementation][ref-flac]. Measurements were done on a Skylake i7.

| Decoder | Time        |
| ------- | ----------- |
| Claxon  | 1.13 ± 0.03 |
| libflac | 1.00 ± 0.03 |

License
-------
Claxon is licensed under the [Apache 2.0][apache2] license. It may be used in
free software as well as closed-source applications, both for commercial and
non-commercial use under the conditions given in the license. If you want to
use Claxon in your GPLv2-licensed software, you can add an [exception][except]
to your copyright notice.

[ci-img]:    https://travis-ci.org/ruuda/claxon.svg?branch=master
[ci]:        https://travis-ci.org/ruuda/claxon
[crate-img]: https://img.shields.io/crates/v/claxon.svg
[crate]:     https://crates.io/crates/claxon
[docs-img]:  https://img.shields.io/badge/docs-online-blue.svg
[docs]:      https://docs.rs/claxon
[hound]:     https://github.com/ruuda/hound
[ref-flac]:  https://git.xiph.org/?p=flac.git
[apache2]:   https://www.apache.org/licenses/LICENSE-2.0
[except]:    https://www.gnu.org/licenses/gpl-faq.html#GPLIncompatibleLibs
