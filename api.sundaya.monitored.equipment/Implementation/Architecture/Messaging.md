# Messaging
---

The messsaging platform implements multiple broker technologies and a common message flow framework implemented by all platform services, both in the cloud and the edge.

The topics and targets are summarised below.

Topic                   | Source                   | Subscription/ Target          | Description 
---                     | ---                      | ---                   | --- 
`monitoring.pms`<br>`monitoring.mppt`<br>`monitoring.inverter` | [dataset/pms](/docs/api.sundaya.monitored.equipment/0/routes/devices/dataset/pms/post)<br>[dataset/mppt](/docs/api.sundaya.monitored.equipment/0/routes/devices/dataset/mppt/post)<br>[dataset/inverter](/docs/api.sundaya.monitored.equipment/0/routes/devices/dataset/inverter/post) | [analytics.pms_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/pms_monitoring)<br>[analytics.mppt_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/mppt_monitoring)<br>[analytics.inverter_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/inverter_monitoring) | streams monitoring data into `analytics` repository. 
`system.feature`        | [api/features](/docs/api.sundaya.monitored.equipment/0/routes/api/features/get) | `env.active.features` | propogates feature toggle changes to each service.


---

### Messaging framework
A particular message broker can be made exclusively active through a service configuration change (and corresponding deployment of the broker). 

The choice of broker and repository will depend mostly on its hosting cost and message volumes, and is expected to change in each environment as the platform evolves and more devices are brought online.

The messaging framework targets the following brokers and storage repositories for the cloud and edge.

Class                         | Cloud                          | Edge                  | Description 
---                           | ---                            | ---                   | --- 
`Publisher`<br>`Subscriber`   | `NATS`, `PubSub`<br>`Kafka`  | `KubeMQ`<br>`Redis`   | Class wrappers for each different **Message Broker**. 
`Storage`                     | `BigQuery`, `GCS`<br>`Bigtable`, `Datastore` | `Redis`<br>`Bitsy` | Class wrapper for each different **Repository**.
`Producer`<br>`Consumer`      | [Pms](/docs/api.sundaya.monitored.equipment/0/routes/devices/dataset/pms/post), [Mppt](/docs/api.sundaya.monitored.equipment/0/routes/devices/dataset/mppt/post)<br>[Inverter](/docs/api.sundaya.monitored.equipment/0/routes/devices/dataset/inverter/post), [Feature](/docs/api.sundaya.monitored.equipment/0/routes/api/features/get) |  | Class wrappers for each different **Dataset**.


---

### Message flow

![Message brokers](/images/message-broker.png)

Messages are processed by the framework based on the following sequence of interactions.

1. **consumer** - instantiates a **subscriber** and provides a _listen_ endpoint for it to callback whenever there is an event.

2. **subscriber** - receives messaging events through the Message Broker queue or API and invokes the **consumer** callback _listen_ endpoint. 

3. **consumer** - will _validate_ and _analyse_ the incoming message, then invoke the _produce_ method on the **producer**.

4. **producer** - will _transform_ the message into the application specified format for the target queue, then calls the _publish_ method on the **producer**

5. **publisher** - will normalise the message into the broker specified format, then use a client library to deliver it to the Message Broker.


---


### Message brokers


#### Kafka

Kafka is implemented in a multi-node cluster. The standard cluster is configured for high availability with  3 masters and N workers: 

- Worker nodes run __Kafka__ on port `9092`. 
- Master nodes run __Zookeeper__ on port `2181`, and do _not_ run brokers.

Topics are divided into `N` __partitions__ and have `f+1` __replicas__. 

The cluster is able to tolerate `f` failures with `f+1` replicas. 

Partition size will be reconfigured during operation based on the number of consumer groups; and message volumes and cardinality of keys used for producing messages.

The __default__ configuration has 3 partitions and 2 replicas per topic.

The following __topics__ are created at startup.

      `monitoring.pms:3:2`, `monitoring.mppt:3:2`, `monitoring.inverter:3:2`

- **kafka publishers** - With Kafka as the message broker the platform's publishers are Kafka producers. These are implemented with KafkaJS. 

   The producers require __acknowledgement__ from the leader only, acknowledgement from replicas is not required. 

   The default max number of __retries__ per call is 8.

   Each node in the producer cluster is given a unique __client id__, suffixed with a 4 digit random number:

      e.g. `producer.<nnnn>`

   The message key, which determines the partition, is the dataset id. 

      e.g. `pms.id`, `mppt.id` or `inverter.id`

- **kafka subscribers** With Kafka as the message broker the platform's Subscribers are actually Kafka consumers. These are implemented by KafkaJS. 

   The number of nodes in the consumer cluster must be less than or equal to the number of topic partitions.

   Each node in the consumer cluster is given a unique __client id__, suffixed with a 4 digit random number.

      e.g. `consumer.<nnnn>`

   Each node has multiple consumer listeners, one for each of the folowing monitoring __topics__.

      e.g. `monitoring.pms`, `monitoring.mppt`, `monitoring.inverter`

   Consumer listeners on all nodes share one of the following __group ids__, depending on which topic they listen to:

      e.g. `group.monitoring.pms`, `group.monitoring.mppt`, `group.monitoring.inverter`

---
