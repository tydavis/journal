# Reasons why VPN in the Google Cloud environment is superfluous

>Published on 2015-12-10

## VPN's main functions are two-fold: encapsulation and encryption

VPN encapsulates the intended transmission from one network to another.
Encryption is used to hide the data from any systems in-flight.

Delivering data from one network to another has nothing to do with addresses --
routers will push packets regardless. Encryption is usable at all levels, can
exist in multiple layers, and can be strengthened or improved independently of
the service itself (e.g. use [ed25519 keys][1] VPN is an unnecessary addition to
the security model because the requisite pieces are already provided via
firewalls and encryption services like SSH and TLS.

## VPN requires "turning it on" when accessing resources

VPN adds another "switch" to flip when performing "real work," especially when
working from home or unfamiliar environments. Employees can, and will, forget to
enable the VPN when working, or will have trouble establishing a VPN tunnel from
a Starbucks when on-call. This can and does increase troubleshooting load,
including maintenance and support load from IT, even when everything is
functioning normally because the VPN adds cognitive overhead to an Engineer's
debugging process. Additionally, during high-stress situations employees
(including Management) may circumvent the VPN for the sake of expediency, which
removes its protection.The VPN provides a single point of failure -- accessing
systems over the office-bound VPN failed repeatedly while we tried to resolve
networking issues. For several days there was intermittent connectivity to
internal addresses in GCP, greatly hampering developer productivity.Using
external IP address access for Engineering operations means connections are
always the same, work successfully over the public internet, and can be
verified, every time. No one has to remember to turn on an External Interface in
order to access a service, they simply connect and are authenticated through.
When paired with multifactor authentication like and using hardware tokens like
the thoroughly-vetted-and-reviewed [Yubikeyw][2], systems are often more secure
than when accessed using an authenticated VPN connection.

## VPN provides a false sense of security

VPNs are usually evangelized as a reason for everyone to "rest easy" knowing
that they are operating within a perceived-controlled environment. This cannot
be further from the truth -- the networked environment is just as vulnerable (if
not more so) than one's own computer. Even worse, should any one employee's
laptop be compromised, VPN access is at the attacker's control and once through
the VPN they have free reign among the unencrypted internal services.\
In contrast, using multiple layers of authorization and encryption (RE:
Duo Security and TLS/SSH) mean that the connections are always secured
and can be limited in their scope. Should one find that a given app or
host is misbehaving, they can change authentication credentials and
audit the offending part instead.

## Specifying multiple layers of firewalls is more restrictive by default than VPN

VPNs tend to be set up such that each endpoint provides access to the rest of
the network as a sort of "high ground." There are no real limits or tests of
traffic from the VPN into the rest of the network. Some VPNs provide VLAN
isolation, but these VPNs are meant to allow engineering work to happen, and so
often provide access directly to the most desirable targets.Using multiple
layers of firewalls instead means that traffic is constantly checked, both on
initial ingress and further attempts to contact / connect with other instances.
This means that a person or system which isn't supposed to contact the database
cannot contact the database despite otherwise having access.

## Responses

### Isn't using a VPN considered a best-practice

It is, but only due to historical issues which have nothing to do with current
technology.The reasons for using VPNs are tied to old-school business models
(i.e. datacenter-to-datacenter and office-to-office connectivity), old software
which does not support encryption, IPv4 limitations, and enshrined "Security
Best Practices":

- NAT helped reduce the issue of publicly routable IPv4 address scarcity/cost,
  which meant leveraging more private networks ([RFC1918][3])
- Offices would have a server-closet, or a datacenter location, storing all of
  their data. That system would live in one centralized location, prompting the
  question of how to access data from another branch office without using the
  public internet.
- [https://en.wikipedia.org/wiki/Timeline*of*file\_sharing][4]
- Encryption was either expensive, slow, or expressly forbidden from leaving the
  country. This meant that being able to account for secured transmissions
  between two points within the country was extremely valuable. (This is a
  guarantee that public internet routes [could not necessarily enforce][5].)
- Multifactor authentication was seen as unnecessary for all but "Top Secret
  Government stuff" and when software companies provided things like "dongles"
  using their own authentication methods, they were circumvented or exploited,
  or usable on a single type of system (no interoperability).

None of these issues apply today:

- IPv4 addresses are still scarce, but Google Compute Platform and
  other cloud providers will provide public IPs to hosts for free. One
  is not charged for their existence or use (unless an address is
  specifically reserved and goes unused).
- Modern systems use a variety of strong encryption methods to secure
  transfers like TLS and SSH.
- [https://en.wikipedia.org/wiki/AES_instruction_set][6]
- TLS provides strong encryption and can work over any TCP connection,
  cheaply. Network transfer is also equally cheap.
- Multifactor authentication is easily added to most systems and is
  implemented using publicly tested implementations, interoperable
  with any platform.

For all of these reasons, we don't need VPN if we can encrypt and
authenticate our services. None of the technology is stopping us.

### Google provides VPN services, so why would they provide it if it isn't needed

Google works with many companies which have security requirements that mandate
the use of a VPN. (See reasons above.) If they did not provide "feature parity"
with those legacy systems, they wouldn't be able to work with the larger
companies that give them lots of money in hosting costs. In short, it costs them
almost nothing to offer it, and they give their clients the ability to check a
box in their requirements documentation for security audits. [Details here.][7]

Google uses things like the [Yubikey][8] and authenticating-gateways to secure
their networks since [they transitioned to a zero-trust design][9] after the
detection of an [APT][10] in 2009. If one of the largest and most-exposed
companies is using a zero-trust model, one would assume it succeeds where a
perimeter-defense design failed.

### What about risk of exposure? What about the additional "attack surface"

Port 22 (SSH) is open to the world on all GCP instances, as SSH is their only
means of access. Botnets are constantly trying to access those systems with
common accounts (e.g. admin, cisco, etc) and yet none of them get in. They are
refused precisely because password-based logins are not used (unless someone
explicitly does so). In the past, using SSH keys was believed considerably more
effort than it was worth -- "just use a password" -- until enough systems were
attacked due to weak passwords. During a recent port scan of an in-use
production network, the only ports publicly accessible to an outside IP address
were the following:

- port 22 -- for SSH.
- port 80, 443 -- for 14 specifically identified webservers.

Otherwise, the firewall denied anything not explicitly approved. The added fact
that the only hosts which responded to cleartext requests were explicitly and
specifically designed to do so should say something about the security of the
setup.

[1]:http://security.stackexchange.com/questions/50878/ecdsa-vs-ecdh-vs-ed25519-vs-curve25519
[2]:https://www.yubico.com/products/yubikey-hardware/yubikey4/
[3]:https://en.wikipedia.org/wiki/Private_network
[4]:https://en.wikipedia.org/wiki/Timeline_of_file_sharing
[5]:http://www.networkworld.com/article/2272520/lan-wan/six-worst-internet-routing-attacks.html
[6]:https://en.wikipedia.org/wiki/AES_instruction_set
[7]:https://cloud.google.com/security/
[8]:https://www.yubico.com/
[9]:https://beyondcorp.com/
[10]:https://en.wikipedia.org/wiki/Operation_Aurora