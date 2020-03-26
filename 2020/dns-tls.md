# TLS and DNS

>Published 2020-03-25

DNS and TLS are strongly associated for most persons. If the DNS
doesn't match the certificate, TLS connections fail, and you have an
outage.  The reality couldn't be further from the truth.

## TL;DR

DNS and TLS are not dependent on each other.

DNS only returns an IP address. TLS will secure a connection to any
IP address and you may request any certificate by name. *Normally*
this is the same as the DNS entry, but you can always explicitly ask
for a different certificate name.

## How does DNS work

DNS names are familiar: google.com, apple.com, gluecode.net -- these
addresses represent one of a collection of records (in particular [A
or AAAA records][4]) which point to the IP address(es) which can
honor the connection requests for "google.com" or whichever website
is being requested.

There's an important fact here: *DNS servers will return whatever IP
address they have referenced for your request.*

Google's DNS servers may have a different answer than Comcast's DNS
servers, which may differ from what your home router says. That's how
some DNS servers can "make your internet faster" - they [return IP
addresses of servers which are physically closer to you][7] among a
huge list of available servers.

What does this mean? Computer network connections operate only on
**IP addresses**, not DNS names.

## How does TLS work

[TLS][2] operates at a [level][3] below where most applications
operate. As we noted above, computers operate only on IP addresses.
[TLS records][6] are just a specially crafted type of IP packet.

[1]: https://en.wikipedia.org/wiki/Domain_Name_System
[2]: https://en.wikipedia.org/wiki/Transport_Layer_Security
[3]: https://en.wikipedia.org/wiki/Internet_protocol_suite
[4]: https://en.wikipedia.org/wiki/List_of_DNS_record_types
[5]: https://en.wikipedia.org/wiki/Transport_Layer_Security#Support_for_name-based_virtual_servers
[6]: https://en.wikipedia.org/wiki/Transport_Layer_Security#TLS_record
[7]: https://en.wikipedia.org/wiki/Domain_Name_System#Function
