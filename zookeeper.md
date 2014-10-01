Zookeeper
==
Root path is /storm.

<a name="storm"></a>
Storm/Topology - /storms/{storm-id}
--
This corresponds to StormBase record.
+ storm-name
+ launch-time-secs
+ num-workers
+ component->executors: a map from spout/bolt id to the number of executors for that component
+ status (some fields are type-dependent):
  * type: may be :active, :inactive, :killed, :rebalancing or nil (the topo just gets created or
          removed)
  * fields for killed: kill-time-secs
  * fields for rebalancing: delay-secs, old-status, num-workers, executor-overrides

<a name="assignment"></a>
Assignment - /assignments/{storm-id}
--
+ master-code-dir: the directory containing code (jars, etc.) for this topo on the
                   master (nimbus) machine
+ node->host: a map from node-id to hostname, for routing
+ executor->node+port: a map from executor-id to node-id and port
+ executor->start-time-secs a map from executor-id to start-time (when it is assigned)

executor-id is [start-task-id end-task-id]. See Task Allocation (TODO).

<a name="supervisor"></a>
Supervisor heartbeat - /supervisors/{supervisor-id}
--
This corresponds to SupervisorInfo record.
+ time-secs
+ hostname
+ assignment-id
+ used-ports
+ meta: currently contains supervisor.slots.ports from storm config
+ scheduler-meta: supervisor.scheduler.meta from storm config
+ uptime-secs

<a name="worker"></a>
Worker heartbeat - /workerbeats/{storm-id}/{node-id}-{port}
--
+ storm-id
+ executor-stats: a map from executor-id to stats. This is also used to determine what executors
  are on this worker. See stats.clj.
+ uptime (in secs)
+ time-secs


/errors
--
