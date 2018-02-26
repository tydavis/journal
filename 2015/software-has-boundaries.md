# Software has boundaries; respect them

>Published on 2015-12-18

This post should be classified as a rant, and it's been brewing for a while.

All software has boundaries. When a system is built, the boundaries are defined
and the end user (often) cannot change them. This doesn't mean we can't have
dynamic software that does a great many flexible things (especially with
embedded scripting languages like Lua) but it does mean that there are
invariants in that program.

What I cannot understand is why a great many engineers look at a system which
they themselves didn't build and expect it to do exactly what they want. "It
should work *this* way" is often heard and that statement is entirely unhelpful.
Systems don't change just because you want them to. Computers are dumb and do
exactly what they are told and those programs may not allow you any form of
customization. That said, if you didn't write the code for a given system, then
you need to either adapt yourself to the system or find something else.

End Users understand this. They know they are given a box-within-a-box, full of
constraints, and they will work day and night to find ways to make that box
sing. What makes it any different for Engineers?

None of this is to say that You, the engineer / software developer / DevOps
person / IT guy, can't write your own version of the software sitting in front
of you. Until you do, *especially* if you're dealing with a must-use system at
work, take the software/platform/thing for what it is and learn it. The more you
fight it, the harder it is for the rest of us, to no one's benefit.
