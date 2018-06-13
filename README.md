# markov [![Build Status][ci-badge]][ci] [![Crates.io][cr-badge]][cr] [![Docs][doc-badge]][doc] [![Built with Spacemacs][bws]][sm]

[ci-badge]: https://travis-ci.org/aatxe/markov.svg
[ci]: https://travis-ci.org/aatxe/markov
[cr-badge]: https://img.shields.io/crates/v/markov.svg
[cr]: https://crates.io/crates/markov
[doc-badge]: https://docs.rs/markov/badge.svg
[doc]: https://docs.rs/markov
[bws]: https://cdn.rawgit.com/syl20bnr/spacemacs/442d025779da2f62fc86c2082703697714db6514/assets/spacemacs-badge.svg
[sm]: http://spacemacs.org

A generic implementation of a [Markov chain](https://en.wikipedia.org/wiki/Markov_chain) in Rust. It
supports all types that implement `Eq`, `Hash`, and `Clone`, and has some specific helpers for
working with `String` as text generation is the most likely use case. You can find up-to-date,
ready-to-use documentation online [on docs.rs][doc].

__Note__: This fork is an attempt to generalize a little bit this implementation focusing on the
next points:

- Remove `None` as a valid token value and the assumption that a chain starts and terminates with it.
- Expose the possibility to feed the chain based on the previous step and the next one.
- Implement iterators for each step of the markov chain instead of runs.
- Take an instance of an RNG as a parameter instead of using the thread one.
- Add an implementation of the new generalized chain compatible with the current one.

My purpose is to have an implementation capable of:

- Plugging the chain into a stream of data.
- Generating a continuos stream of data from the chain.
- Predict or measure the probability of the next step given a current state.


## Examples ##

With Strings: 
```rust,no_run
extern crate markov;

use markov::Chain;

fn main() {
    let mut chain = Chain::new();
    chain.feed_str("I like cats and I like dogs.");
    println!("{:?}", chain.generate_str());
}
```

With integers:
```rust,no_run
extern crate markov;

use markov::Chain;

fn main() {
    let mut chain = Chain::new();
    chain.feed(vec![1u8, 2, 3, 5]).feed(vec![3u8, 9, 2]);
    println!("{:?}", chain.generate());
}
```

Chains have iterators (both infinite and sized!):
```rust,no_run
extern crate markov;

use markov::Chain;

fn main() {
    let mut chain = Chain::new();
    chain.feed_str("I like cats and I like dogs.");
    for line in chain.iter_for(5) {
        println!("{:?}", line);
    }
}
```

Chains can be higher-order:
```rust,no_run
extern crate markov;

use markov::Chain;

fn main() {
    let mut chain = Chain::of_order(2);
    chain.feed_str("I like cats and I like dogs.");
    for line in chain.iter_for(5) {
        println!("{:?}", line);
    }
}
```

## Contributing ##
Contributions to this library would be immensely appreciated. It should be noted that as this is a
public domain project, any contributions will thus be released into the public domain as well.
