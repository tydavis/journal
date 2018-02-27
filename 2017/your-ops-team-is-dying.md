# Your Ops team is dying

>Published on 2017-08-03

Operations teams which don't evolve are dying off, and that's a good
thing.

## Infrastructure is a matter of services, service providers, and your budget

Most companies don't buy actual hardware servers these days. Cloud
Providers make it easy for services to leverage the provider's economies
of scale and rent compute time and services. As such, your Operations
team isn't building networks, hooking up servers, or doing anything
except laying down the services on which your business will run. Also,
teams will take advantage of managed services like on-event functions
(Cloud Functions, AWS Lambda), or databases, meaning there will be less
for your operations team to worry about.

## Developers must be responsible for managing and deploying their code

Developers know their code, and how to scale it. Without a lot of
documentation or runbooks, the Operations team knows little beyond "turn
it off and on again" to resolve a 3am pager alert.

When you empower your Developers, they also take those responsibilities
seriously. [This article makes some really good
points][1]
about this very issue, and how Operations can't necessarily handle the
code-related bug in a service.

## Focus on Simplicity and small teams for fast feature deployment

Operations usually scales with the rate of Developers, but it's a matter
of ratios: some shops have a 10:1 rate of Developers to Operations
personnel, while others reach 50:1 or higher.

I've personally worked on a team where the ratio was 60:1. After six
months of work at (re)building the infrastructure, providing
documentation, and campaigning for developers to be their own tier-1
on-call, I was moved to Tier-2 on-call. For the next year, I didn't get
a single pager duty alert, despite being on-call for most of it.

If teams can be [kept small][2] and [the architecture kept as simple as
possible][3], shipping features and resolving bugs will be inherently faster
than any large-scale organization structure. The (possibly apocryphal) Amazon
"two pizza" limit applies here: if you can't feed the *entire* team with an
order of two large pizzas (16 slices), the team is too big.

### Recap

Operations teams need to focus on infrastructure and applications which
let Developers create, deploy, and scale their designs without
involvement from the Operations team. At best, the Operations team will
be invisible -- no one interacts with them, the infrastructure doesn't
fail, and the architecture is simple enough to understand and exploit
without calling them in.

When the Operations team builds the right kind of infrastructure, you
don't need a ratio of 10:1 -- you have a team small enough to be fed by
**one** pizza or, ideally, **no pizza**. Your developer teams leverage
provider-optimized services, keep the architecture simple, and
understand the weak points of the architecture just like your Operations
team did...

### Post Script

For those who might think this is alarmist, or oversimplifying the role
of an Operations team in an organization, let me clarify: this is my
job. I've been working in an operations role for over a decade and seen
radical efficiency and delivery (regardless of Product success) where
companies follow this mindset. When companies have large-scale
operations and IT organizations, that's when inefficiencies, delays, and
the "ops vs devs" culture wars start. The future is having a group of
Engineers, a focus on simplicity, and a problem to solve.

[1]:https://medium.com/@copyconstruct/the-death-of-ops-is-greatly-exaggerated-ff3bd4a67f24
[2]:http://firstround.com/review/Why-Yammer-believes-the-traditional-engineering-organizational-structure-is-dead/
[3]:http://www.effectiveengineer.com/blog/hidden-costs-that-engineers-ignore
