Apache Storm learning notes
===========

## Daemons

### [Nimbus] (nimbus.md)
- The master node. There is only one nimbus running for one storm cluster.
- Manages topologies/storms.
- Handles all user operations: submit/kill topology, rebalance, etc.
- Topology code is uploaded to the nimbus machine and then downloaded by supervisors.
- Task scheduling.

### [Supervisor] (supervisor.md)
- Supervises worker daemons.
- Downloads code for topologies from nimbus.

## [Zookeeper Nodes] (zookeeper.md)
