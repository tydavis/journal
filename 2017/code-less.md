# Code Less

> Published on 2017-03-04

There are a few core pillars of action and design which I've followed to great
success over the years. They are as follows...

## Always question your instinct to abstract

If you are in a SysAdmins/DevOps role, you need to constantly question the
systems you are building. Question **every single** abstraction or proxy you
design. Do you really need to store secrets in Vault, or can you use instance
roles/tags/built-in functions of your provider? Do you really need Terraform, or
Puppet, when you can accomplish the same with a single Ansible playbook YAML
file? (Or better yet, let the application(s) do the configuration!)

This is a common refrain in development circles -- a lot of those I've worked
with don't like frameworks and abstractions unless they're absolutely necessary.
IT/DevOps/Ops/SysAdmins need to start adopting the same approaches, and when you
do, you end up with a simpler and more reasonable system.

As an anecdote, I came across a program that wraps docker and docker-compose,
injecting credentials as an added layer directly into the image build process,
because developers aren't allowed to have non-dev environment credentials. The
better solution is to have each environment inject the credentials into
containers **at runtime** and remove any option to embed credentials into the
image at all. In this way, the docker image remains the same across environments
(facilitating troubleshooting and reliable testing), yet credentials never cross
boundaries.

## Do everything in your power to remove abstractions

Do you really need a load-balancer in front of a proxy that communicates with an
application? What if I told you [you don\'t need a VPN][1]? As a sysadmin, every
extra piece of infrastructure in your environment is another point of failure.
Every wrapper shell script, every time you use docker-compose instead of plain
docker commands, every instance of make instead of a POSIX-compliant shell (
[ash][2]) script ends up being one more barrier to a new-hire onboarding
process, or one more hurdle for migrating to a new platform.

## Prefer static programs to interpreted/dynamic systems

When possible, use systems that have no external dependencies, no local
dependencies, and/or are statically-linked (at compile time). So, given C vs a
Java/Python/Ruby program, prefer the C program. When possible, build Go (golang)
programs as statically linked rather than relying on dynamic libraries.

By building static packages of software, one can guarantee (or have a high
degree of confidence) that the program will still be useful when the system is
in a broken state.

Anecdotally, I recently ruined the installation of on my CentOS 7 host because I
performed a system-wide `pip install --upgrade ...`

## Static Systems (Continued)

While we're talking about static binaries, let's also consider [immutable
servers][3]. Building your environment so that you do not change things
in-place, but instead build entirely new infrastructure to support the new
deployment, while keeping the old one unchanged will sidestep a lot of issues
with updating/upgrading any part of the environment.

## Document your research and decisions

This is probably the most important pillar, because without it, everything you
do is subject to decay. By writing down your experiences, decisions,
difficulties and (especially) the business decisions surrounding the final
implementation, you not only present a consistent and reliable narrative, but
you have the ability to point out changes in circumstance when promoting a new
solution to an old/existing problem.

When faced with a problem and you have potential solutions, narrate the problem,
present the options, and send that to the stakeholders. If you "have no
stakeholders," then *You and everyone who may possibly come after you* are the
stakeholders.

## Who is this guy

I'm a SysAdmin in an SRE/DevOps role. I've been in this kind of role for my
entire career (10+ years); since before "devops" was a term. As such, I
understand software engineering, and can communicate with developers "on their
level," but my bread-and-butter is managing systems and infrastructure. I like
writing code, and I love developing smart systems. What makes me happiest is
simplifying, teaching, and automating myself out of work. I hope that these
pillars give you a solid framework to build your own successful career.

[1]:../2015/no-nat-use-firewalls.md
[2]:https://en.wikipedia.org/wiki/Almquist_shell
[3]:https://martinfowler.com/bliki/ImmutableServer.html
