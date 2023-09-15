+++
title = "Pull requests are bad"
description = "Pull requests are bad and here is why."
date = 2023-09-14
tags = ["software-engineering"]
+++

Not always.

But more often than you would think, pull requests are not the greatest way
to handle software development. I will go as far as saying sometimes pull-request driven
development is the worst way to move a team forward.

<!-- more -->

# Intro

Let's take a step back and realize that relative to how long programming has been around,
pull requests are indeed a new addition to the table. People have created software long
before *this tool* existed, and some still go without them. Of course, these small little
bubbles of text and nicely rendered code diffs are solving an issue.

My goal by writing this post is telling you that like any other tooling, a premature use
of a solution to a problem that you do not have can turn back to bite you. And that pull-requests
are also just that - *a tool* - nothing more. The same way you would not bring an `is-prime` dependency
to your project unless you need the functionality, you shouldn't deploy pull requests by default just
because they exists.

# When does it make sense to use it?

I can not go through the comprehensive list of every time it does make sense to use
pull requests as that list would be a long one. But if I was forced to describe most
items on that list with as little word as possible, it would be:

> When your codebase is not split up into proper modules. Or when person/module ratio is high.

And historically pull requests do exist to create a queue of concurrent changes being made to
the same piece, of course as soon as a merge happens, other might have to resolve merge conflicts.
This goes as far back as to the grandfather of pull requests. The mailing lists filled with git patches.

But every minute spent on rebasing to resolve merge conflicts is a wasted engineering hour, and you ideally
want to setup and environment that reduces that as much as possible.

Using pull request is similar to wrapping the entire repository in a `Mutex`.

# What else can we do?

Use a `Vec<Mutex<CodeScope>>` instead of a single `Mutex`. And what does that mean?

Setup your project properly in the beginning. Treat different root directories as different
modules in the code. Define the borders and what visible unit each module will encompass.

This way you can have scopes in the codebase that can be implemented internally however you
want, as long as they meet a pre-defined interface. Let your module `B` import `x` from `A`,
even if `x` is not implemented yet.

This way you can now give out ownership of these modules to different people. And that is
easier when the `people/module` ratio is low. The more modules you have to give to people,
the more sparse the code ownership matrix you have, the easier this becomes.

Now, the question is do you trust your team members or not? If not, ...well... you shouldn't
have hired them. But if you do, you just let everyone do their job simultaneously, now there
is more than one `Mutex` that can be locked at a time. Resulting in less synchronization and
less CPU cycles wasted waiting to acquire the hold on the codebase.

# What about code review?

Peer programming, periodic reviews can help. Also personally I am more in favour of periodic code
audits rather than one PR at a time review processes.
