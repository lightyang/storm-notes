Nimbus
==
Worker slots: workers.

Task Allocation/Assignments
--
1. Read all topology info from zookeeper and jars.
2. Read existing assignments from zookeeper.
  - Exclude the assignment for rebalancing storm: workers in the assignment will be reassigned and
    it will be recomputed.
3. Compute topology->executor->note+port map.
  1. Compute topology->executors map for current topologies. E.g. (for one topology):  
     { Spout1: (2 tasks, 1 executor), Bolt1: (6 tasks, 2 executors) } ->  
     [(1, 2), (3, 5), (6, 8)]
  2. Update executor heartbeats.
  3. Compute alive executors using heartbeat info. Executors currently assigned to the rebalancing
     storm are considered alive automatically.
  4. Compute dead ports from dead executors. They cannot be used in this round of assignment.
  5. Construct SchedulerAssignment objects from existing assignments info with only alive executors
     included. These objects are to be used by the scheduler API.
  6. Construct SupervisorDetails to be used by the scheduler API - basically what ports are
     available on a machine.
  7. Ask the scheduler to compute the new assignment.
4. Construct assignments.
  1. Compute node->host map. If a supervisor is lost (we know the worker is alive), use node->host
     info in the previous assignment for the same topology.
  2. Add start-time to reassigned executors.
5. Update zookeeper, only for assignments that have changed.
6. Notify INimbus about assigned slot changes.

Topology State Transition
--
