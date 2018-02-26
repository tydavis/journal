# Blog on a diet

> Published on 2017-05-05

Every time I think I've found a suitable means of running my blog (e.g.
[Hugo][1], [Ghost][2], etc), I end up reading or reviewing *something else*
compelling enough to warrant implementation.

I'm an optimizer by trade and by desire -- I love efficiency and get a thrill
out of playing the [bicycle-riding hippie to everyone else's SUV-driver][3].
When it comes to this blog, I've been looking for the simple, easy, and fast
implementation that also delivers *just* what I want to me (and any incidental
readers -- Thank you!). Since I started [moving to a Chromebook][4], I've had to
rule out local static site generators like Hugo and Pelican, and because I want
to use my own domain with TLS certificates (particularly those from [Let's
Encrypt][5]) and the latest HTTP2 technology, I have to run my own server with
software like [Caddy][6].

Practically everyone I know swears by [Markdown][7] format (which requires a
parser/generator to create the real HTML) but it looked easy. Even the uncool
Enterprise kids were doing it, and so I tried it. The hard part was that I knew
HTML already and ended up googling syntax more than I wanted, and nothing ever
came out 100% as I imagined it would in my head.

Fast-forward a couple of years. About a month ago, I read [this talk][8] by
[Maciej Cegłowski][9]. This was it -- I had found my answer, and it was "roll up
your sleeves and get to work."

Much like my use of Golang, Perl, and Linux, I'm a fan of anything you can
produce or make better with mere effort. In this case, writing HTML was a little
work, but it got me exactly what I wanted.

[1]:https://gohugo.io/
[2]:https://ghost.org/
[3]:http://idlewords.com/talks/website_obesity.htm
[4]:life-in-chrome.md
[5]:https://letsencrypt.org/
[6]:https://caddyserver.com/
[7]:https://en.wikipedia.org/wiki/Markdown
[8]:http://idlewords.com/talks/website_obesity.htm
[9]:http://idlewords.com/about.htm
