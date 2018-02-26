# Get on the Upgrade Treadmill

> Published on 2017-03-07

Always upgrade your software. Upgrading now means less pain later.

Recently my company upgraded from [Apache Kafka][1] v0.8 to the latest available
version (v0.10.2). With it came wire protocol changes, two separate clients were
consolidated into only one, Kafka now stores offsets in itself rather than
[Apache Zookeeper][2], and a dozen other changes that also needed to be adjusted
in our golang services.

One solid outage-into-the-evening later, I've vowed two things:

## Always Upgrade Your Software

If you use the latest libraries, the latest software, and constantly keep your
systems up-to-date, the incremental changes end up becoming smaller and smaller
as new point-versions come out. This get easier as everyone learns to "ride the
wave" and make systems smaller. Constant upgrades also suggest (though they do
not *require*) a trend toward smaller services or [microservice
architectures][3] in order to keep the upgrades small in scope.

## Test Everything

- If there isn't a test for a block of code, make one. If that code operates
  against a remote service, build a mock with expectations (or [contracts][4])
  defined. If that's too much, and you have sufficient resources, run a
  stripped-down copy of the service locally. If you can't do that, at least
  consult the documentation and generate a mock out of supposed API docs.
- Always generate your own test data. Don't expect someone else to make the test
  data for you.

[1]:https://kafka.apache.org/
[2]:https://zookeeper.apache.org/
[3]:https://martinfowler.com/articles/microservices.html
[4]:https://en.wikipedia.org/wiki/Design_by_contract
