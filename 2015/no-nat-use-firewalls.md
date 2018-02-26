# Don't worry about NAT, just use firewalls

>Published on 2015-12-14

I hear again and again about how NAT is a "best practice" and should be used as
the base of network design. When working in the Google Cloud (and Amazon EC2),
publicly routeable IPv4 addresses are provided to each machine "for free" within
Google's IPv4 allocation block. When this is first brought up, everyone reacts
defensively and cautiously (good job, security teams -- you finally got that
message out). Once it is fully explained that having an external IP address does
not inherently reduce the security of a machine, people tend to see it as a
benefit.

Stack Overflow threads actually addressed my points much better than I had
myself, so here is one snippet and one full-length response regarding NAT...

While [this page][1] is addressing IPv6, the same information applies:

> We can argue the merits of NAT, the end-to-end principle, and security until
> we're blue in the face -- and many have -- but the reality is that NAT does
> not provide any real network security. Worse yet, it actually prevents many
> security measures and provides an additional attack surface for your network.

[And this ServerFault page][2] regarding IPv6 and "NAT being a thing of the
past" actually addresses a network with all IPv4 addresses publicly accessible:

> First and foremost, there is nothing to fear from being on a public IP
> allocation, so long as your security devices are configured right.
>
> > What should I be replacing NAT with, if we don't have physically separate
> > networks?
>
> The same thing we've been physically separating them with since the 1980's,
> routers and firewalls. The one big security gain you get with NAT is that it
> forces you into a default-deny configuration. In order to get any service
> through it, you have to explicitly punch holes. The fancier devices even allow
> you to apply IP-based ACLs to those holes, just like a firewall. Probably
> because they have 'Firewall' on the box, actually. A correctly configured
> firewall provides exactly the same service as a NAT gateway. NAT gateways are
> frequently used because they're easier to get into a secure config than most
> firewalls.
>
> > I hear that IPv6 and IPSEC are supposed to make all this secure somehow, but
> > without physically separated networks that make these devices invisible to
> > the Internet, I really can't see how.
>
> This is a misconception. I work for a University that has a /16 IPv4
> allocation, and the vast, vast majority of our IP address consumption is on
> that public allocation. Certainly all of our end-user workstations and
> printers. Our RFC1918 consumption is limited to network devices and certain
> specific servers where such addresses are required. I would not be surprised
> if you just shivered just now, because I certainly did when I showed up on my
> first day and saw the post-it on my monitor with my IP address. And yet, we
> survive. Why? Because we have an exterior firewall configured for default-deny
> with limited ICMP throughput. Just because 140.160.123.45 is theoretically
> routeable, does not mean you can get there from wherever you are on the public
> internet. This is what firewalls were designed to do. Given the right router
> configs, and different subnets in our allocation can be completely unreachable
> from each other. You do can do this in router tables or firewalls. This is a
> separate network and has satisfied our security auditors in the past.
>
> > There's no way in hell I'll put our billing database (With lots of credit
> > card information!) on the internet for everyone to see.
>
> Our billing database is on a public IPv4 address, and has been for its entire
> existence, but we have proof you can't get there from here. Just because an
> address is on the public v4 routeable list does not mean it is guaranteed to
> be delivered. The two firewalls between the evils of the Internet and the
> actual database ports filter out the evil. Even from my desk, behind the first
> firewall, I can't get to that database. Credit-card information is one special
> case. That's subject to the PCI-DSS standards, and the standards state
> directly that servers that contain such data have to be behind a NAT
> gateway\[1\]. Ours are, and these three servers represent our total server
> usage of RFC1918 addresses. It doesn't add any security, just a layer of
> complexity, but we need to get that checkbox checked for audits.
>
> The original "IPv6 makes NAT a thing of the past" idea was put forward before
> the Internet boom really hit full mainstream. In 1995 NAT was a workaround for
> getting around a small IP allocation. In 2005 it was enshrined in many
> Security Best Practices document, and at least one major standard (PCI-DSS to
> be specific). The only concrete benefit NAT gives is that an external entity
> performing recon on the network doesn't know what the IP landscape looks like
> behind the NAT device (though thanks to RFC1918 they have a good guess), and
> on NAT-free IPv4 (such as my work) that isn't the case. It's a small step in
> defense-in-depth, not a big one. The replacement for RFC1918 addresses are
> what are called Unique Local Addresses. Like RFC1918, they don't route unless
> peers specifically agree to let them route. Unlike RFC1918, they are
> (probably) globally unique. IPv6 address translators that translate a ULA to a
> Global IP do exist in the higher range perimeter gear, definitely not in the
> SOHO gear yet. You can survive just fine with a public IP address. Just keep
> in mind that 'public' does not guarantee 'reachable', and you'll be fine.
>
> 1: The PCI-DSS standards changed in October 2010, the statement mandating
> RFC1918 addresses was removed, and 'network isolation' replaced it.

Key Points here: "The two firewalls between the evils of the Internet and the
actual database ports filter out the evil." Also "just keep in mind that
'public' does not guarantee 'reachable' and you'll be fine."

To reiterate the response, having a default-deny firewall (which the major
cloud-provider firewalls are) and a cautious approach to opening ports or
whitelisting traffic is sufficient to accomplish the same goals as NAT, with the
only exception of "getting around a small network allocation."

If you have a small fixed number of IP addresses, NAT is your only easy way of
getting more addresses than you have. Otherwise, it's unnecessary.

[1]:http://www.internetsociety.org/deploy360/blog/2015/01/ipv6-security-myth-3-no-ipv6-nat-means-less-security/
[2]:http://serverfault.com/a/184535