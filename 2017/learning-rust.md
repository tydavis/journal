# Learning Rust

>Published on 2017-10-24

I'm in the process of [learning Rust][1] to build my systems-language skills.
While at first my brain outright rejected the format and design of rust, I am
now working my way through [Rust By Example][2] and the results are dramatically
different. Finally, it's starting to make sense, and the benefits of the
language design are becoming more apparent (especially from my perspective as an
SRE).

I've been learning Go for the last few years, which makes Rust an immensely
foreign design at first. *Let? Mut? Traits? So much syntactic sugar… **where are
my goroutines**?!* Go was designed to fix long compile times, threading
problems, and in general provide an "easy onboarding experience" for developers
through simple design and having a lot of "batteries included" libraries. Rust,
on the other hand, is trying to fix the problems with C/C++ as a systems
language, focusing on code safety as a top concern.

I haven't yet built anything using Rust -- I still fall back to Go first -- but
I do find [ripgrep][3] and [fd][4] are two incredibly helpful (and crazy-fast)
programs worth installing everywhere. Once I start building my own solutions in
Rust, I'll write a follow-up post with better insights.

[1]:https://www.rust-lang.org/
[2]:https://rustbyexample.com/
[3]:https://github.com/BurntSushi/ripgrep
[4]:https://github.com/sharkdp/fd
