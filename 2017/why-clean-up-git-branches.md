# Why clean up git branches

>Published on 2017-09-07

## Problem

When encountering issues like:

```sh
Cloning repository...
Cloning into '/builds/username/repo_name'...
fatal: pack is corrupted (SHA1 mismatch)
fatal: index-pack failed
ERROR: Job failed: exit code 1
```

## Solution

Run `git gc --aggressive` and/or `git repack -a -d -f --depth=50 --window=250`
on the origin repository. If you are using a system like Gitlab / GitlabCI,
manually trigger [the Housekeeping routine][1].

## Background

Central repositories can have git index corruption and other slowness operations
due to fragmentation and high numbers of objects in a git repository. By
deleting merged branches, using only lightweight tags for references (instead
of, say release branches), and squashing merges into a single commit, we reduce
the total number of objects git is required to manage.

Normally, git will automatically collect unused references, but the incremental
GC operations don't regularly go back and rebuild or repack the repository data.
These manual invocations of git gc/repack will resolve the issue.

[1]:https://docs.gitlab.com/ee/administration/housekeeping.html