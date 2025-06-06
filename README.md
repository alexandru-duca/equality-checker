# QuiZX: a quick Rust port of PyZX

[![pypi][]](https://pypi.org/project/quizx/)
[![py-version][]](https://pypi.org/project/quizx/)
[![crates][]](https://crates.io/crates/quizx)
[![msrv][]](https://github.com/zxlang/quizx)
[![rs-docs][]](https://docs.rs/quizx)

  [pypi]: https://img.shields.io/pypi/v/quizx
  [py-version]: https://img.shields.io/pypi/pyversions/quizx
  [crates]: https://img.shields.io/crates/v/quizx
  [msrv]: https://img.shields.io/crates/msrv/quizx
  [rs-docs]: https://img.shields.io/docsrs/quizx?label=rust%20docs

[PyZX](https://github.com/zxlang/pyzx) is a Python library for quantum circuit optimisation and compiling using the [ZX-calculus](https://zxcalculus.com). It's great for hacking, learning, and trying things out in [Jupyter](https://jupyter.org/) notebooks. However, it's written to maximise clarity and fun, not performance.

This is a port of some of the core functionality of PyZX to the [Rust](https://www.rust-lang.org/) programming language. This is a modern systems programming language, which enables writing software that is very fast and memory efficient.

The library is available both as a Rust crate and a Python package. See the [Python Changelog](https://github.com/zxcalc/quizx/blob/master/pybindings/CHANGELOG.md) and the [Rust Changelog](https://github.com/zxcalc/quizx/blob/master/quizx/CHANGELOG.md) for the latest updates.

See the [CONTRIBUTING.md](https://github.com/zxcalc/quizx/blob/master/CONTRIBUTING.md) file for detailed instructions for building the libraries from source and contributing to the project.
Pull requests are welcome!

## A bit about performance

As a very anecdotal example of the performance difference, the program `spider_chain` builds a chain of 1 million green spiders and fuses them all. In PyZX, you can fuse all the spiders in a ZX-diagram as follows:

```python
from pyzx.basicrules import *

success = True
while success:
    success = any(fuse(g, g.edge_s(e), g.edge_t(e)) for e in g.edges())
```

In QuiZX, the Rust code is slightly more verbose, but similar in spirit:
```rust
use quizx::basic_rules::*;

loop {
    match g.find_edge(|v0,v1,_| check_spider_fusion(&g, v0, v1)) {
        Some((v0,v1,_)) => spider_fusion_unchecked(&mut g, v0, v1),
        None => break,
    };
}
```

On my laptop, the PyZX code takes about 98 seconds to fuse 1 million spiders, whereas the QuiZX code takes 17 milliseconds.

## License

This project is licensed under Apache License, Version 2.0 ([LICENSE][] or http://www.apache.org/licenses/LICENSE-2.0).
