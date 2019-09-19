# Message Broker
---

### Brokers

The Message Broker is implemented with a multi-node Kafka cluster. The standard cluster is configured for high availability with  3 masters and N workers: 

- Worker nodes run Kafka on port `9092`. 
- Master nodes run Zookeeper on port `2181`, and do _not_ run brokers.

Topics are divided into N partitions and have f+1 replicas. The default is 3 partitions and 2 replicas. The cluster is able to tolerate f failures with f+1 replicas. Partition size should be reconfigured during operation based on the number of consumer groups; and message volumes and cardinality of keys used for producing messages.

The following topics are created at startup, by default with 3 partitions and 2 replicas per topic.

  `monitoring.pms:3:2`, `monitoring.mppt:3:2`, `monitoring.inverter:3:2`

### Producers

The producers require acknowledgement from the leader only, acknowledgement from replicas is not required. 
The default max number of retries per call is 8.

The default producer client id is:

 `clientid.apihost`.

The message key, which determines the partition, is the dataset id. e.g. `pms.id`, `mppt.id` or `inverter.id`.   

### Consumers

The number of consumer groups must be less than or equal to the number of topic partitions.

Each node in the consumer cluster runs the same 'consumers' container process, which has multiple consumer listeners, one for each of the monitoring topics.

   `monitoring.pms`, `monitoring.mppt`, `monitoring.inverter`

The consumers in all nodes share the following group ids, depending on which topic they listen to:

   `group.monitoring.pms`, `group.monitoring.mppt`, `group.monitoring.inverter`

The client id of each 'consumers' container process is suffixed with a 4 digit random number e.g. `clientid.consumer.<nnnn>` to give cluster process a unique id.

---
