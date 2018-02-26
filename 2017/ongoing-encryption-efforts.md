# Ongoing encryption efforts

> Published on 2017-03-23

I have been using a [hosts file override][1] for years to cut the ads and "crap"
out of my internet browsing experience. I recently discovered a [better list][2]
and have been using it for a few weeks. I'm getting far more "*you have an
adblocker installed*" warnings from websites I frequent, so it's definitely
working better.

Now the Senate has passed [laws that permit ISPs to sell my data to
advertisers][3] and I'm ready to call [Game Over \[warning: explicit\]][4] on my
internet access.

I'm looking at purchasing a [Samsung Chromebook Plus][5] but with the Chromebook
comes the inability to install an [encrypting DNS proxy][6] and Google doesn't
[seem interested][7] in supporting DnsCrypt with their public servers.

What they do provide is [DNS over HTTPS][8] which can be somewhat useful if
we're running our own DNS servers at home, but by providing an HTTPS endpoint,
**Chrome and its extensions** could bypass local DNS and make HTTPS-based DNS
requests on their own in order to avoid being spoofed (or to hide additional DNS
requests).

Many things to research here.

[1]:http://someonewhocares.org/hosts/
[2]:https://github.com/StevenBlack/hosts
[3]:https://arstechnica.com/tech-policy/2017/03/senate-votes-to-let-isps-sell-your-web-browsing-history-to-advertisers/
[4]:https://youtu.be/dsx2vdn7gpY
[5]:https://www.amazon.com/Samsung-Chromebook-Convertible-Laptop-XE513C24-K01US/dp/B01LZ6XKS6/
[6]:https://dnscrypt.org/
[7]:https://groups.google.com/forum/#!topic/public-dns-discuss/rmZTtPAV430
[8]:https://developers.google.com/speed/public-dns/docs/dns-over-https
