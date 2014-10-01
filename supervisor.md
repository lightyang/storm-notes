Supervisor
==
assignment-id: supervisor id.
assignment: a map from port to [storm-id, executors] pair

Three recurring events:
- [Heartbeat] (#heartbeat)
- [synchronize-supervisor] (#sync-supervisor)
- [sync-processes] (#sync-processes)

<a name="heartbeat"></a>
Heartbeat
--
Heartbeats to [/supervisors/{supervisor-id}] (zookeeper.md#supervisor)

<a name="sync-supervisor"></a>
(make-)synchronize-supervisor
--
1. Read assignments from zk and find out what are new.
2. Download code for new assignments.
3. Update assignment in LocalState.
4. Remove unused code.

<a name="sync-processes"></a>
sync-processes
--
1. Find out existing workers from worker heartbeat directory.
2. Shut down invalid workers, which includes:
  - not-started: no heartbeats.
  - timed-out: heartbeat timeout.
  - disallowed: not approved (e.g. killed but still running) or no matching assignment (e.g. the assignment has changed due to rebalancing).
3. mkdirs for new workers.
4. Update approved-workers in LocalState.
5. Launch new workers.

LocalState
--
Supervisor uses Localstate, a persistent K/V store class that writes data to the disk, to
support recovery on restart.
