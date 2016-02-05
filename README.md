# doll

Simple glue code that can be used to send arbitrary commands to nodes. Nodes can be added manually or retrieved from the list of puppet agents known to a puppet master. It adds the functionality to filter by hostname patterns or by facter facts as well as to ignore certain hosts. 

The original motivation was to have a simple tool that can trigger puppet agent runs on a specific subset of all puppet agents.

## setup

Redefine variables in `conf/doll.cfg` according to your environment. You'll need password-less (pubkey auth) access as ``$PUPPETUSER`` on ``$PUPPETMASTER`` and password-less access as ``$REMOTEUSER`` on managed agents. You'll need [parallel-ssh](https://code.google.com/p/parallel-ssh/) installed as well. There's a very basic check you can run afterwards.
```bash
$ doll check
/usr/bin/parallel-ssh
3.5.1
 All tests passed.
```

## usage

### add nodes

Load all puppet agents known to the configured puppet master in `conf/doll.cfg` .
```bash
$ doll load-puppet-agents
```
Or simply add nodes manually.
```bash
$ doll add super.special
```
Reset list of nodes.
```bash
$ doll clear
```

### list all nodes
View all known nodes.
```bash
$ doll list
```

### filter nodes
Filter hostnames using regular expressions,
```bash
$ doll filter .*dev 
```
or regarding certain facter values. You could also allow multiple legal values.
```bash
$ doll factfilter osfamily RedHat
$ doll factfilter processorcount 1 2 4
```
Every filter action alters the doll list permanently until it gets either cleared oder loaded again. 

### trigger actions
Trigger a puppet onetime puppet agent run on all nodes in the list,
```bash
$ doll trigger-puppet 
```
or run an arbitrary command on each node, like restarting a service.
```bash
$ doll run <CMD>
```

### display puppet information
Show the last puppet run's summary for each node.
```bash
$ doll puppet-summary
``` 
Show the puppet agent version on each node.
```bash
$ doll puppet-version
```

### ignore nodes
Permanently add hostname (or regular expression) to ignore list.
```bash
$ doll ignore <PATTERN>
```
Show ignored nodes.
```bash
$ doll list-ignored
```
Reset ignore list.
```bash
$ doll clear-ignored
```

