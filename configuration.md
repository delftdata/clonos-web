---
layout: default
title: Configuration
permalink: config
has_children: false
nav_order: 5
---

# Configuration

Clonos adds a number of configuration parameters on top of base Flink, which are described below.

## Deployment Parameters

### Standby

| parameter															 | Description														                                | Recommended			 |
|:---------------------------------------|:-----------------------------------------------------------------------|:-----------------|
| jobmanager.execution.failover-strategy | Sets the failover strategy for Clonos. Either "full" or "standbytask".	|	standbytask			 |
| jobmanager.execution.num-standby-tasks | Sets the number of standby tasks Clonos should maintain per task.				|	1								 |

### In-Flight log

| parameter															 | Description														                                | Recommended			 |
|:---------------------------------------|:-----------------------------------------------------------------------|:-----------------|
| taskmanager.inflight.type							 | Sets the type of inflight log. Either "inmemory" or "spillable"|spillable |
| taskmanager.inflight.spill.policy | Sets the spill strategy of the spillable inflight log. Either "eager", "availability".|	availability|
| taskmanager.infight.spill.availability-trigger | Sets the spill threshold of the availability strategy. A value of 0.4 triggers a spill when only 40% of network buffers are available.|	0.4 |
| taskmanager.inflight.spill.sleep | Interval in ms between checks of buffer availability |	100 |
| taskmanager.inflight.num-prefetch-buffers							 | Number of buffers used for pre-fetching during in-fligh replay. |	10|
| taskmanager.network.memory.sender-extra-buffers-per-channel | Sets the size of the in-flight log buffer pool. For each channel add this many buffers to it. |	10 |
| taskmanager.network.memory.sender-extra-buffers-per-gate | Sets the base size of the in-flight log buffer pool. |	200 |


### Causal Log Manager

| parameter															 | Description														                                | Recommended			 |
|:---------------------------------------|:-----------------------------------------------------------------------|:-----------------|
|  taskmanager.network.netty.determinantMemorySteal	| Percentage of network (off-heap) memory stolen from the network manager for the Causal Log Manager | 0.2 |
|  taskmanager.network.netty.determinantBufferSize | Size of buffers used to implement causal logs. If each TaskManager must track a large number of tasks, reducing this can help in the face of OOM errors. | 32768 |
|  taskmanager.network.netty.determinantBuffersPerJob | How many buffers each job is allocated for causal logging. | 100 |
|  taskmanager.network.netty.enableDeltaSharingOptimizations | Only disabled for debugging. Avoids sharing causal logs along non-causal paths. | true |
|  taskmanager.network.netty.determinantDeltaEncodingStrategy | Chooses the encoding strategy of causal logs on buffers. Either "hierarchical" or "flat". Hierarchical strategy is more compact. | hierarchical |


## Runtime Parameters

These parameters can be set at runtime by each stream processing job.

```java

// The granularity with which time is measured in Clonos.
// Increasing this value results in less nondeterminism at the cost of less granular computations.
// Between 1 and 10, this is mostly unnoticeable.
env.getConfig.setAutoTimeSetterInterval(5)

// The determinant sharing depth. Use -1 for full sharing.
env.getJavaEnv.setDeterminantSharingDepth(1)
```
