# Why encrypting everything on the internet makes sense

> Published on 2017-03-30

## TL;DR

Start using [HTTPSEverywhere][1] and support the websites that make a point of
delivering your data in a way that protects **you**.

------------------------------------------------------------------------

Since [the House voted to destroy Privacy rules governing ISPs][2], I've been
reading a lot of discussion on the subject and trying to find ways to keep me
and mine under a nice veil of privacy. I have also [encountered commentary][3]
about how streaming media (like Netflix) *shouldn't* be secured, because:

> There are things which don't need encryption and movie streaming is one of
> \[them\]. We don't need the extra power wasted in our world as datacenters are
> power hungry monsters. Use encryption for what its designed for. Protecting
> confidential data.
>
> In the end every Netflix user is going to pay the extra bill for this and this
> is a waste of resources in every possible way.

**It's not.** All of your activity should be encrypted in ways that cannot be
decrypted or tracked and here's why:

Radio and other broadcast media are sent into the ether and broadcasters have no
idea exactly *who* is listening. You can build your own crystal radio kit,
listen to a station, and the broadcaster *has no idea*.

Contrast this with the Internet: every single device on the internet must
*request* data in order to be *sent* data. Even fancy things like
[multicasting][4] still require nodes to Join or Leave the network. Each system
on the internet is in *constant*, **identifiable** communication with other
computers in its network.

As such, due to the way Internet Service Providers (ISPs) work, they have the
potential to completely track and control your communications unless they're
100% encrypted. The only way to end such invasive, dangerous, and *wrong*
actions by ISPs and other entities is to use encryption from end-to-end.

I'll leave you with a quote from [Bruce Schneier][5] (emphasis mine):

> Last week, revelation of yet another NSA surveillance effort against the
> American people has rekindled the privacy debate. Those in favor of these
> programs have trotted out the same rhetorical question we hear every time
> privacy advocates oppose ID checks, video cameras, massive databases, data
> mining, and other wholesale surveillance measures: \"If you aren't doing
> anything wrong, what do you have to hide?\"
>
> Some clever answers: \"If I'm not doing anything wrong, then you have no cause
> to watch me.\" \"Because the government gets to define what's wrong, and they
> keep changing the definition.\" \"Because you might do something wrong with my
> information.\" My problem with quips like these \-- as right as they are \--
> is that they accept the premise that privacy is about hiding a wrong. It's
> not. **Privacy is an inherent human right, and a requirement for maintaining
> the human condition with dignity and respect.**
>
> \[ . . . \]
>
> Watch someone long enough, and you'll find something to arrest \-- or just
> blackmail \-- with. Privacy is important because without it, **surveillance
> information will be abused:** to peep, **to sell to marketers** and to spy on
> political enemies \-- whoever they happen to be at the time.
>
> Privacy protects us from abuses by those in power, even if we're doing nothing
> wrong at the time of surveillance.

[1]:https://www.eff.org/https-everywhere
[2]:https://arstechnica.com/tech-policy/2017/03/isps-and-fcc-chair-ajit-pai-celebrate-death-of-online-privacy-rules/
[3]:https://arstechnica.com/security/2015/04/it-wasnt-easy-but-netflix-will-soon-use-https-to-secure-video-streams/
[4]:https://en.wikipedia.org/wiki/Multicast
[5]:https://www.schneier.com/blog/archives/2006/05/the_value_of_pr.html
