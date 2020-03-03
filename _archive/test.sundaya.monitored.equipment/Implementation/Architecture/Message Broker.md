# Message Broker
---

### Brokers

The Message Broker is implemented with a multi-node Kafka cluster. The standard cluster is configured for high availability with  3 masters and N workers: 

- Worker nodes run __Kafka__ on port `9092`. 
- Master nodes run __Zookeeper__ on port `2181`, and do _not_ run brokers.

Topics are divided into `N` __partitions__ and have `f+1` __replicas__. 

The cluster is able to tolerate `f` failures with `f+1` replicas. 

Partition size will be reconfigured during operation based on the number of consumer groups; and message volumes and cardinality of keys used for producing messages.

The __default__ configuration has 3 partitions and 2 replicas per topic.

The following __topics__ are created at startup.

  `monitoring.pms:3:2`, `monitoring.mppt:3:2`, `monitoring.inverter:3:2`

### Producers

The producers require __acknowledgement__ from the leader only, acknowledgement from replicas is not required. 

The default max number of __retries__ per call is 8.

Each node in the producer cluster is given a unique __client id__, suffixed with a 4 digit random number:

   e.g. `producer.<nnnn>`.

The message key, which determines the partition, is the dataset id. 

   e.g. `pms.id`, `mppt.id` or `inverter.id`.   

### Consumers

The number of nodes in the consumer cluster must be less than or equal to the number of topic partitions.

Each node in the consumer cluster is given a unique __client id__, suffixed with a 4 digit random number.
   e.g. `consumer.<nnnn>`.

Each node has multiple consumer listeners, one for each of the folowing monitoring __topics__.

   `monitoring.pms`, `monitoring.mppt`, `monitoring.inverter`

Consumer listeners on all nodes share one of the following __group ids__, depending on which topic they listen to:

   `group.monitoring.pms`, `group.monitoring.mppt`, `group.monitoring.inverter`


---
