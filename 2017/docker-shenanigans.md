# Docker Shenanigans

>Published on 2017-11-02

I've been working with docker since [v1.6.0 was "new."][1] I like docker. I
think it has many good qualities. What I don't appreciate is how badly we
developers, devops, and ops-persons abuse it.

## Docker is not a VM

A virtual machine has virtual hardware, an internal operating system (which can
be entirely different from the host), and runs *the entire Operating System*
including its own memory management, software updates, user accounts, etc.

Docker containers, by contrast, share a kernel and otherwise acts like a
FreeBSD/Solaris [jail][2] for the process(es) inside, with a single user: root.

Docker leverages multiple technologies, acting as a frontend: [kernel control
groups][3] (cgroups), [Overlay][4] ([COW][5]) Filesystems, and [kernel
namespaces][6]. As such, the host configuration (e.g. kernel version) matters
significantly more to a docker host than a VM host. This means Docker can pack
more containers on a single host than most hypervisors can pack VMs, with
dramatically less overhead per container.

## Stop making oversized docker images

Docker containers are designed such that the application defined in the CMD or
ENTRYPOINT directive is the [init process][7] for the container. The technology
behind docker (above) means you need *just enough* to run the intended
application. For most applications, that "just enough" can be as little as a
single binary, or a dozen supporting libraries, or a JVM installation.

In order to make this "just enough" container, there's a specific method
called [the Builder Pattern](/view/gluecode/2017/docker-builder-pattern)
that leverages container impermanence to build an application with
minimal dependencies in the final product image. It's so powerful, you
don't even need the language tools installed on your machine to build!

Make your production (deployment) containers as small as you reasonably
can. It makes *everything* easier.

## Stop trying to "hack" docker to make it go faster. Fix your architecture / code instead

Docker is not a panacea. It enables some very useful and secure
optimizations for developers and operations/infrastructure teams alike.
Its general-purpose design means it works "by default" for most
architectures and applications as well.

If you've done everything to optimize the container, and moving an application
into docker causes it to perform *worse* than when running natively, take a long
look at the code and its architecture. Even traditional monoliths like
[mysql][8] & [postgresql][9] perform well in containers.

[1]:https://github.com/moby/moby/releases?after=v1.7.0-rc2
[2]:https://en.wikipedia.org/wiki/FreeBSD_jail
[3]:https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt
[4]:https://docs.docker.com/engine/userguide/storagedriver/
[5]:https://en.wikipedia.org/wiki/Copy-on-write
[6]:https://en.wikipedia.org/wiki/Linux_namespaces
[7]:https://en.wikipedia.org/wiki/Init
[8]:https://hub.docker.com/_/mysql/
[9]:https://hub.docker.com/_/postgres/
[10]: