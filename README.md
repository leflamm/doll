
# doll

Simple glue code that can be used to send arbitrary commands to e. g. ``puppet agents`` managed by a ``puppet master`` for e. g. triggering puppet runs or restart services.

## disclaimer

Rather alpha, k?

## setup

Redefine variables in `conf/doll.cfg` according to your environment. You'll need password-less (pubkey auth) access as ``$PUPPETUSER`` on ``$PUPPETMASTER`` and password-less access as ``$REMOTEUSER`` on managed agents. You'll need [parallel-ssh](https://code.google.com/p/parallel-ssh/) installed as well. There's a very basic check you can run afterwards.
```bash
$ doll check
/usr/bin/parallel-ssh
3.5.1
 All tests passed.
```

## usage

Load list of puppet agents managed by puppet master. I'll call them dolls from now on.
```bash
$ doll load
```
```bash
$ doll list
app.dev
app.live
app.pre
balancer.dev
balancer.live
balancer.pre
log.dev
log.live
log.pre
web.dev
web.live
web.pre
```
Filter dolls with pattern regarding their names...
```bash
$ doll filter .*dev
app.dev
balancer.dev
log.dev
web.dev

```
... or regarding certain facter values.
```
$ doll factfilter osfamily RedHat
```
You could also allow multiple legal values.
```
$ doll factfilter processorcount 1 2 4
```
Every (fact)filter action alters the doll list permanently until it gets either cleared oder loaded again. This means multiple filter values within the same filter call are combined via an OR-operation. Hence, multiple filter commands are an AND-combination.
```bash
$ doll list
app.dev
balancer.dev
log.dev
web.dev
```
Maybe add another one manually.
```bash
$ doll add super.special
```
```bash
$ doll list
app.dev
balancer.dev
log.dev
web.dev
super.special
````
Send simple command like triggering pupper agent
```bash
$ doll run "puppet agent --onetime --no-daemonize"
[1] 11:15:39 [SUCCESS] app.dev
[2] 11:16:13 [SUCCESS] balancer.dev
[3] 11:16:54 [SUCCESS] log.dev
[4] 11:17:38 [SUCCESS] web.dev
[5] 11:18:06 [SUCCESS] super.special
```
... or restart a service
```bash
$ doll run "service ntpd restart"
app.dev: Shutting down ntpd: 
app.dev: [  OK  ]
app.dev: 
app.dev: Starting ntpd: 
app.dev: [  OK  ]
app.dev: 
[1] 11:17:54 [SUCCESS] app.dev
balancer.dev: Shutting down ntpd: 
balancer.dev: [  OK  ]
balancer.dev: 
balancer.dev: Starting ntpd: 
balancer.dev: [  OK  ]
balancer.dev: 
[2] 11:17:54 [SUCCESS] balancer.dev
...
```
You can ignore dolls permanently so they will be excluded from any run.
```
$ doll ignore app.dev
$ doll listignores
app.dev
$ doll clearignores
```

## buzz

puppet master agent pssh parallel-ssh orchestrate simple command send service restart non-declarative action trigger on-demand kick run facter filter hostname filter

