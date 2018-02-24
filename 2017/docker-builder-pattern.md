# Docker Builder Pattern

>Published on 2017-11-02

The Docker Builder Pattern is a highly useful pattern for leveraging docker
containers to generate artifacts and then package those artifacts in a
runtime-only image. This pattern minimizes production container sizes,
accelerating deployment while reducing incidents of broken dependencies,
conflicting build libraries, and permits centralized control of build tools.

>Examples below include both Windows and Linux/OSX equivalent commands.

## Example Problem

Check out the code from [https://github.com/tydavis/hello-world-docker][1] and
make sure you [have Docker installed][2] and running.

### Build the Binary

If you don't have the Go compiler installed, don't worry! *You're not going to
need it.*

Make sure your shell is in the *hello-world-docker* directory and execute the
following command:

>For both Linux and Windows (Powershell)

```bash
docker run -v ${PWD}:/go/src/github.com/tydavis/hello-world-docker \
-w /go/src/github.com/tydavis/hello-world-docker -it golang:alpine \
/bin/sh -c "CGO_ENABLED=0 go build "
```

Let me walk you through this command:

1. "-v …" binds [the current working directory][3] to the relevant location
    inside the container (the part after the colon : )
1. "-w …" [sets the container's current working directory][4] to your mounted
    directory
1. "-it" grants you an [interactive terminal connection][5]
1. "golang:alpine" is the latest set of Go compiler and utilities built on top
    of [Alpine Linux][6]. Since we don't need extra utilities like the race
    detector or other glibc-exclusives, it's a safe choice.
1. `/bin/sh -c "CGO\_ENABLED=0 go build"` -- this command disables dynamic
    linking, which creates a statically-linked binary.

What did we actually do? We just created a Go binary without installing
*anything* to our machine.

### Build the Docker image

Now, with the binary created, it's just another file to docker. We can build our
"production" image with:

```bash
docker build -t hello-world-docker:1 .
```

If you dig into the Dockerfile, you'll see that we start with [the scratch
layer][7]. That means the only thing in this container is the binary. Let's look
at the image size:

```text
tydavis@utils:~/go/src/github.com/tydavis/hello-world-docker$ docker images
REPOSITORY           TAG                 IMAGE ID            CREATED         SIZE
hello-world-docker   1                   fc2081dda00b        10 seconds ago  2.03MB
golang               alpine              6e8378057093        7 days ago      269MB
tydavis@utils:~/go/src/github.com/tydavis/hello-world-docker$ du -h hello-world-docker
2.0M    hello-world-docker
```

## What about [Multi-Stage Builds][8]

Multi-stage builds take the wrong approach.

Without using mount-points, users have been ADDing or COPYing their entire
codebase into the container image via a *docker build* command, then using
*docker cp* to extract the resulting artifacts. As we can see above, this is
(usually) a fundamentally flawed way of building artifacts using a container.

The multi-stage concept takes this further down the "wrong" path, encouraging
this same copy-code-into-image mindset and providing an unnecessary function to
discard the image inline during build process. As we see above, one does not
have to build the tools/compiler container every time, meaning
artifact-build-time is significantly faster than the multi-stage process, even
with build layer caching.

## Conclusion

If one is using a language that permits generating artifacts (Go[lang], Java/JVM
languages, C/++, etc) then copying code into the image will unnecessarily bloat
the result. One should be using the builder pattern instead.

Conversely, if using something like Python, Ruby, or another interpreted
language, then copying into the image may be the only solution due to runtime
environment requirements.

[1]:https://github.com/tydavis/hello-world-docker
[2]:https://www.docker.com/get
[3]:https://docs.docker.com/engine/reference/commandline/run/#mount-volume--v-read-only
[4]:https://docs.docker.com/engine/reference/commandline/run/#set-working-directory--w
[5]:https://docs.docker.com/engine/reference/commandline/run/#examples
[6]:http://containertutorials.com/alpine/get_started.html
[7]:https://docs.docker.com/develop/develop-images/baseimages/
[8]:https://docs.docker.com/develop/develop-images/multistage-build/
