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

But how does this translate to code? If you've looked at any codebase written in Rust, you might have
already seen stuff like `&[u8]` or `&mut [u8]`, and this is exactly where you see the above logic being
translated to code.

The first expression is a reference to an slice of data, and so is the second one. However there is a 
huge difference between the two, in the first expression you can only do any operation that does not
change (mutate) data, while you can call methods on the mutable reference that *... well ...* mutate
and change the data.

Let's look at some more code to understand this.

```rust
fn print_length(a: &[u8]) {
    println!("len(a) = {}", a.len());
}
```

The following function tries to take a slice and print the size of the slice, and you can see we're
getting an input of `&[u8]` and not `&mut [u8]`, the reason for this is because this function does
not require changing any data in the slice. Now let's try to write another function that write zero
to the entire slice.

```rust
fn write_zero(a: &[u8]) {
    for i in 0..a.len() {
        a[i] = 0;
    }
}
```

Now if we try to run `cargo check` on this piece of code, we will see this error pop up:

```
`a` is a `&` reference, so the data it refers to cannot be written
```

Which basically means: since `a` is a immutable reference (denoted by `&`), it can not be modified.
To fix this we can try and change `a` to be a mutable reference and try the code again:

```rust
fn write_zero(a: &mut [u8]) {
    for i in 0..a.len() {
        a[i] = 0;
    }
}
```

Now you can see that changing the reference into a mutable reference fixes the function. And as you
also checked this is all done during the compilation of the program, meaning that the Rust compiler
performs a static type analysis on the code and prevents the code from getting compiled if it violets
the borrow checker.

If you're interested in more complex topics and how you can skip this step in compilation and do similar
safety checks during the program's runtime, stay tuned for the next post.
