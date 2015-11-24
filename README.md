
# doll

Simple glue code that can be used to send arbitrary commands to e. g. ``puppet agents`` managed by a ``puppet master`` for e. g. triggering puppet runs or restart services.

## disclaimer

Rather alpha, k?

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
Filter dolls with pattern.
```bash
$ doll filter .*dev
app.dev
balancer.dev
log.dev
web.dev
```
This is now permanent unless list gets cleared or loaded again.
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

## buzz

puppet master agent pssh parallel-ssh orchestrate simple command send service restart non-declarative action trigger on-demand kick run

