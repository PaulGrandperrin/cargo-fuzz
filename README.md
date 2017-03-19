# Cargo-fuzz

Commandline wrapper for using libFuzzer. Easy to use, no need to recompile LLVM!


libFuzzer needs LLVM sanitizer support, so this is x86-64 Linux-only for now. This also needs a nightly since it uses some unstable commandline flags.

This crate is currently under some churn -- in case stuff isn't working, please reinstall it (`cargo install cargo-fuzz -f`), and delete the cloned `libfuzzer-sys` folder in the `fuzz/` folder. Rerunning `cargo fuzz --init` after moving your `fuzz` folder and updating this crate may get you a better generated `fuzz/Cargo.toml`. Expect this to settle down soon.

## Installation

```sh
$ cargo install cargo-fuzz
```

## Usage

First, set up your project for fuzzing:

```sh
$ cd /path/to/project
$ cargo fuzz init
```

This will create a `fuzz` folder, containing a fuzzing script called `fuzzer_script_1` in the
`fuzzers/` subfolder. It is generally a good idea to check in the files generated by `init`.

libFuzzer is going to repeatedly call the `go()` function in the fuzzer script with a byte buffer
`data` of length `size`, until your program hits an error condition (segfault, panic, etc). Write
your `go()` function to hit the entry point you need.

You can add more fuzz target scripts via `cargo fuzz add name_of_script`. There
is a `Cargo.toml` in the `fuzz/` folder where you can add dependencies.


To fuzz a fuzz target, run:

```sh
$ cd /path/to/project
$ cargo fuzz run fuzzer_script_1 # or whatever the target is named
```

Then, wait till it finds something! More complex invocations are available as well. Consider
looking at `cargo fuzz --help`, `cargo fuzz run --help` and others.

## Trophy case

🏆 🏆 🏆 🏆 🏆 🏆

 - [toml-rs panic](https://github.com/alexcrichton/toml-rs/issues/152)
 - unicode-segmentation: [grapheme boundary correctness](https://github.com/unicode-rs/unicode-segmentation/issues/19), [word boundary correctness](https://github.com/unicode-rs/unicode-segmentation/issues/20)
 - image: [1](https://github.com/PistonDevelopers/image/issues/622), [2](https://github.com/PistonDevelopers/image/issues/623), [3](https://github.com/PistonDevelopers/image/issues/624), [4](https://github.com/PistonDevelopers/image/issues/625)
 - inflate: [arithmetic overflow](https://github.com/PistonDevelopers/inflate/issues/14)
 - capnproto-rust: [Multiple bugs, including a memory safety bug](https://dwrensha.github.io/capnproto-rust/2017/02/27/cargo-fuzz.html)
 - hyper: [arithmetic overflow](https://github.com/hyperium/hyper/pull/1076)
 - libpnet: [arithmetic overflow](https://github.com/libpnet/libpnet/pull/250)
 - quick-xml: [arithmetic overflow](https://github.com/tafia/quick-xml/issues/53)
 - svgparser: [arithmetic overflow, bound checking panic, incorrect result](https://github.com/RazrFalcon/libsvgparser/commit/4742f16e834445a682a0a4db62600d275a457390), [endless loop](https://github.com/RazrFalcon/libsvgparser/commit/c55d9a7d4d1e83f405be2e7bfddea89f579f6fc9)
 - num: [panic on `BigInt` parsing](https://github.com/rust-num/num/issues/268)
 - [httpdate panics](https://pyfisch.org/blog/fuzzing-all-crates/): "no character boundary" and arithmetic overflow
 - vobsub: [invalid slice 1](https://github.com/emk/subtitles-rs/commit/20e430105b1fc02aa135788ba150a0dd49a7d1ef), [2](https://github.com/emk/subtitles-rs/commit/46df766dd22cb6a04a534611f08c23903e58746c), [3](https://github.com/emk/subtitles-rs/commit/f2f5309aa8173dfec4bb5816950d718a1ac669c2), [shift overflow](https://github.com/emk/subtitles-rs/commit/5d3364b96389d90deac0f024a57660951b7e1dd6), [arithmetic overflow](https://github.com/emk/subtitles-rs/commit/3afdb7e1c5e786e88653253243648dd9d49983f2)
 - uuid: [index out of bounds](https://github.com/rust-lang-nursery/uuid/pull/81)
 - flac: [index out of bounds](https://github.com/sourrust/flac/issues/11)
 - ntp: [panic caused by unwrap on invalid input](https://github.com/JeffBelgum/ntp/commit/f23ded23c26a5326dae249905d298e8c5f51d371)
 - pulldown-cmark: [Overflow ParseIntError](https://github.com/google/pulldown-cmark/issues/49)
 - bson: [multiple bugs, inclung arithmetic overflow](https://github.com/zonyitoo/bson-rs/issues/64)
 - jpeg-decoder: [arithmetic overflow](https://github.com/kaksmet/jpeg-decoder/issues/69)
