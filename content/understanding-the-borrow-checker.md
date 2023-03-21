+++
title = "Understanding the borrow checker"
description = "Understanding the Rust's borrow checker."
date = 2023-02-21
tags = ["rust"]
+++

To learn Rust one of the most challenging things I had to deal with was the borrow checker... It used
to drive me crazy. Every time I used to compile something or do a `cargo check` the borrow checker was
angry.

<!-- more -->

Rust becomes insanely simpler once you understand the borrow checker and what is it good for. As you may
know Rust is a *safe language*, and this is hugely owed to the borrow checker.

In Rust every memory location is guranteed to be in either one of these two scenarios:

1. There can exist **many** references to the data with a **read-only**  permission (immutable).
2. There can be **only one** reference to the data with a **writable** permission (mutable).

This very simple but elegant design choice is what gives Rust a powerful super power over other languages,
but like every other thing in life there are cons as well. The obvious one being you can not do *magical*
pointer operations without relying on `unsafe` code blocks.
