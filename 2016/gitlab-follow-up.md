# Gitlab Follow-Up

>Published on 2016-08-29

The team which tested Gitlab for the duration of their sprint had such good
feedback during and after the sprint, word-of-mouth prompted other teams to
migrate ahead of schedule and start testing it for themselves. This brought up a
few new data points and some lessons-learned.

## NodeJS and Alpine

Running NodeJS 4.4.7 and npm 3.10.5, somehow our particular installations are
incurring an approximately 70-second delay in *npm install* times when running
on Alpine 3.4 instead of Debian Jessie. We still haven\'t narrowed down where
this slowdown occurs yet, but my feeling is we are \"doing something wrong\" as
other NodeJS shops have reported [significant][1] [gains][2] when running on
Alpine.

Also, Java and other languages work fine in Alpine. NodeJS is our only
difficulty right now.

## Cache server needs a Delete button

While this has more to do with the [Minio project][3] than anything, we\'ve had
some incidents where the cache zip archive was either corrupted and \"killed\"
the build or it grew so large (4GB+) that it doubled our build time. Especially
in the case of a corrupted zip, the build did *not* continue without the cache
files and fully stopped the build with an error message.

**To the Gitlab CI folks:** this is *not* how you treat a cache!

The only fix we could come up with was to delete the cache zip and perform a
fresh build, but this was not easily accomplished by the developers, especially
since they weren\'t able to issue a delete command either from Gitlab or on the
Minio web interface. Instead, we had to go in to the host itself and remove the
cache file directly from the disk.

## Versioning is an interesting discussion

For a very long time we have used [semantic versioning][4] at my company, and
the developers wanted a way to tag builds when successful, but the fact is that
runner\'s git checkouts are read-only, there are no arbitrary parameters which
can be passed to a build, and builds can happen in various orders (with various
build numbers/iterations) decided by the runner based on available resources.
And then there\'s the whole argument about how to *enforce* SemVer changes,
because people inadvertently break the `major.minor.patch` protocols all the
time and there is no way to correct or confirm it in the code.

As such, we had to take a long look at how we versioned our software and our
artifacts. The first thing we noted is that we don\'t have a lot of shared
libraries which *require* semantic versioning. Instead, we just need a way to
determine a (fuzzy) magnitude of change and a means of sorting newer vs older
artifacts.

(We do this all the time in Golang!) So with a bit of discussion we stumbled on
a particularly easy and human-readable solution:

```bash
BUILD_VERSION=`git log --oneline | wc -l|tr -d ' '`-`git rev-parse HEAD|head -c 10`
```

This produces a version of the format `<commit count>-<unique hash>` satisfying
the (again, fuzzy) magnitude of change, and providing an easy way of determining
newness (barring any incidents of force-push rebase operations). Version
`4123-2f1df15079` is going to be newer than `4007-fc036c4ee0`, sorts properly
when using most sorts, and (due to the hash value) guarantees that if versions
are produced independently (say by two different developers) they will not have
the same version.

I have yet to get a lot of traction with this scheme, as the Cult of SemVer is
strong, but for offices where the product is not a code library it doesn't make
a lot of sense to use SemVer.

[1]:http://odino.org/minimal-docker-run-your-nodejs-app-in-25mb-of-an-image/
[2]:https://blog.risingstack.com/minimal-docker-containers-for-node-js/
[3]:https://www.minio.io/
[4]:http://semver.org/
