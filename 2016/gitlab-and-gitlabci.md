# Gitlab and GitlabCI

> Published on 2016-08-16

My company currently uses Github and Jenkins to handle our source code and build
our artifacts/docker images, respectively. We\'ve had to adjust our build system
over time as we grow, now running a total of three build agents with 8 cores per
agent, and \~20GB of RAM per host.

As a result, we can have as many as 20 builds and/or tests running
simultaneously and there is very little \"rhyme or reason\" to our build
schedule save that they almost always happen between 9am and 5pm PST.

Seeing that we were spending \$\$\$\$ to run these always-on build-agents and
store a *limited* number of repositories, I decided to look at what it would
take to create a dynamic build system.

## The Runners Up

- Jenkins plugins for docker, kubernetes, etc
- Travis CI (and Cirlce CI, Drone.io, \...) and other build-as-a-service
  platforms
- TeamCity
- Atlassian Bamboo

Jenkins plugins for docker-based scaling including builds via Kubernetes were
all in their *alpha* state or otherwise unsuitable for use with our environment.
The only other alternatiave with Jenkins would be for me to write my own plugin
to do exactly what we wanted for dynamic builder creation.

TravisCI and such are great for free/open-source systems, but for a private
company with our concurrent build requirements, they were a no-go. Too costly
with not enough concurrency for our day-to-day operations.

Similarly, TeamCity and Atlassian Bamboo (the commercial offerings) made all
their money around additional build agents. This would encourage us to keep our
number of agents small, which is the same situation we were in with Jenkins.

## Gitlab

Gitlab CE (Community Edition) came out ahead by having:

- Git hosting for unlimited private repos and users (saving us the \$450/month
  we were spending on Github private repositories)
- A CI system where the configuration is stored in the repository itself (giving
  Developers control and revision history)
- GitlabCI\'s configuration is in standard YAML (rather than a weird DSL like
  Jenkins)
- GitlabCI allows for docker-machine operation, creating machines as needed to
  accomplish its build objectives, then tearing them down (re: deleting them
  from our cloud provider) when idle. Also all operations happen within docker
  containers, meaning provisioning operations are not required for the runners.

Needless to say, I was impressed with it.

I had to patch docker-machine due to some non-standard configuration issues we
had in Google Compute Platform, but that only took a day to figure out. Once I
had the patched docker-machine installed on the Gitlab host, I configured the
runner to use docker-machine and launch a system with privleged containers.
After all that, we were in business!

The configuration YAMLs for most of our services are less than 50 lines, so it
didn't take long to get a few services migrated over.

## Caveats and Issues

Using git to commit results back into the repository is not possible (by
default) in a build. Runners have a read-only copy of the repository, but
Artifacts can be saved to the master (and are available for download after the
build). If you want to commit the results back in to the git repo, you\'ll have
to create a docker image with credentials built in.

Since runners are created and destroyed regularly, docker images should be
optimized for size (i.e. use Alpine linux as a base instead of Debian) and one
should have a mirror if the docker registry isn\'t on-network. Reducing the
transfer requirements will greatly accelerate builds.

## Dogfooding and Impressions

One of the teams nearby is almost always willing to try the \"new stuff\" first
and give feedback / input, so I have them using Gitlab for their current sprint.
So far, they are happy with it and excited about the automatic testing and build
operations.