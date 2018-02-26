# Tech Choices and Pitfalls for DevOps

>Published on 2016-03-16

DevOps encompasses ideas which fly in the face of the last few decades of
systems design. Similarly, adoption of cloud-based architectures also disrupt
the last few decades of intuition and "common sense."

Below is a clarification of technologies, reasons for their adoption and usage,
why their avoidance is preferable. Interspersed are snippets regarding pitfalls
with respect to purely virtual (cloud) architectures and their usage.

------------------------------------------------------------------------

## Containers - Docker

Let's talk about [docker][1]. Actually, let's talk about *containerization*.

Containers are a godsend to engineering as a whole. With a container, all code,
library dependencies, and other "must be set up" requirements are handled in a
single, atomic, blob of data for deployment. The only things that a container
*can't* handle are dependent services (e.g. external databases).

As a consequence, those containers are nearly everything one needs to deploy new
software. If docker (or a container runtime) is installed, use the facilities
provided to download the container and start it. Suddenly, your
Java/NodeJS/Go/Python/Rust/C++/Wordpress machination is up-and-running without
having to install anything.

Docker is the de-facto format and runtime for managing containers, though there
are new contenders for container creation and management, like [rkt][2].
Considering rkt uses the open-container-initiative's [specifications][3],
whereas Docker has already started to diverge from that, it\'s a question about
whether rkt may be a better choice going forward.

If you aren't using containers of some type yet, you are missing out.

> UPDATE -- 2016/04/17 -- Docker v1.11 uses the OCI contianer specification.

## Configuration Management - Ansible

There is a lot of discussion across the internet about [Puppet][4], [Chef][5],
and [SaltStack][6], but the real winner is [Ansible][7]. Ansible does everything
that the docker container *can\'t* handle, like basic systems provisioning.

What makes Ansible the winner here? Puppet, Chef, and even Salt require
bootstrapping of some kind -- some kind of Agent installed on minions that let
them communicate with their Master server. Ansible has none of that. Working
purely over python & ssh, Ansible's default is to connect to a box using SSH and
perform actions without leaving a trace that it was there.

What about code changes? What about half-completed operations? Those are handled
with Assertions in an ansible playbook. When things aren't the way your playbook
expects them to be, Ansible will complain, LOUDLY, and refuse to do it. No
automatic attempts at resolving the issue or starting things in the background
(*unless you programmed it that way*), just a not-so-quiet failure and the
reasons for said failure.

You might ask "what about audit trails and other security functions." Since
Ansible runs over SSH, whoever has Ansible access has SSH access, meaning they
are limited by what they could do if they were to remotely connect to the
machine and run the commands themselves. Thus Ansible does not provide an
*additional* route into a box, but automation over an existing access method. To
avoid granting all and sundry SSH access, find ways to push data away from the
box and feed it into things like centralized logging platforms, provide web
access to diagnostic systems, and so on. And if you want an audit trail for the
changes to playbooks, follow [the best practices][8] and build your
infrastructure immutably.

## Scripting and Development - Go (and others)

It really doesn't matter so much what language you choose, so long as you
understand said language. If, however, you're in a "green field" situation or
have the room to really dig in and adopt a new language, give a shot. It's very
much a "[blue-collar language][9]" in the same vein as Java -- one can write
themselves out of a problem, without needing to be "clever." This is the
explicit design of the language:

> The key point here is our programmers are Googlers, they're not researchers.
> They're typically, fairly young, fresh out of school, probably learned Java,
> maybe learned C or C++, probably learned Python. They're not capable of
> understanding a brilliant language but we want to use them to build good
> software. So, the language that we give them has to be easy for them to
> understand and easy to adopt. -- Rob Pike

And...

> It must be familiar, roughly C-like. Programmers working at Google are early
> in their careers and are most familiar with procedural languages, particularly
> from the C family. The need to get programmers productive quickly in a new
> language means that the language cannot be too radical. -- Rob Pike

Bash, python, perl, scala, java, clojure, ruby, or any other language will do
just fine. Go wins this contest by being built for productivity and by
permitting statically compiled executables (and cross-compilation without a new
toolchain) for great portability.

## Cloud Provider - Google Compute Platform (Digital Ocean)

Cloud providers are expensive compared to buying one's own hardware for simple
things, but the benefits of adaptability and no need to negotiate colocation
contracts. Based on the latest information I can gather from the time of this
post (March 2016), Google Compute Platform provides approximately \~20% savings
over other providers for sufficiently large deployments. Depending on your use
case, you can save more (or none at all) depending on your needs.

Google Compute Platform is also undeniably Google, meaning you can be forced
into doing things The Google Way, but there is great simplicity in its design
and many provided services make for a compelling place to put your startup.

For small, personal, or green-field projects, [Digital Ocean][10] has lately
been the best-of-breed among providers and integrates nicely with many
provisioning systems ([like Ansible][11]).

## Changes when working with cloud providers

One important change to note is that Engineers, when considering things like
logging or caching information, must recognize that the "X is cheap"
philosophies don't necessarily apply. As an example, yes, disk is cheap, but
SSD-scale attached storage costs over four times the spindle-disk equivalent on
Google Compute Platform. More so, both storage types have a maximum throughput
where adding more storage *does not* grant ever more throughput or IOPS. So this
is with all other resources in any given cloud environment.

Similarly, one must build applications with an eye toward recovery and
adaptation. Network reliability is not 100% and is usually around a
[98][12]-[99][13]% SLA. By definition, this means that a network (or only part
of it) could be unreachable for well over ten minutes of cumulative time in a
day. To combat this, one must do things like permit retries when submitting
information to an API, or include in your code, and things like reconnecting to
databases. Always [try to implement backoff code][14] to avoid the "thundering
herd" problem when those systems do reconnect.

Finally, if a system is built with all of these aspects in mind, use (free!)
[encryption][15] and authentication for all endpoints.
Single-factor authentication *might* be sufficient when communicating between
servers, but for user-level access, adopt 2FA if you mean to do anything
remotely sensitive. Yes, this includes something as simple as storing a user\'s
address.

## More to come

I sincerely hope I haven't put anyone off with this, and I am anxious to have
more suggestions and feedback as I am exposed to more of the "soft bits" toward
being in DevOps. To mangle a friend's phrases, "technology choices are a very
*junior* concern. No one cares. Focus on the business objectives, because that's
all that really matters."

[1]:https://www.docker.com/
[2]:https://coreos.com/rkt/
[3]:https://www.opencontainers.org/about
[4]:https://puppetlabs.com/
[5]:https://www.chef.io/chef/
[6]:http://saltstack.com/
[7]:https://www.ansible.com/
[8]:http://docs.ansible.com/ansible/playbooks_best_practices.html
[9]:http://blog.paralleluniverse.co/2014/05/01/modern-java/
[10]:https://www.digitalocean.com/
[11]:http://docs.ansible.com/ansible/digital_ocean_module.html
[12]:http://uptime.is/98
[13]:http://uptime.is/99
[14]:https://blog.gopheracademy.com/advent-2014/backoff/
[15]:https://letsencrypt.org/