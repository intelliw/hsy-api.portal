# Edge Cloud
---

The Edge cloud gateway allows edge devices to connect and publish data to the cloud, and for managed services to be provided at the edge.

It provides the following features:

- High availaiblity and load balancing in a small-footprint single-board compute cluster.

- Rolling updates and managed CI/CD from cloud to all edge devices (using _Kubernetes/K3s_).

- Cloud services can be redeployed at the edge based on HTTPS/gRPC endpoints and lightweight message broker (see below, compared to [Cloud messaging] 

- Service-mesh orchestratioon and traffic management, certificate management, and monitoring (using _Traefik_/_Containous_).

- Optional high-volume data collection by field staff through mobile App over BLE.

- Write coalescing with compression techniques to reduce traffic volume.

- Device authentication at the edge through on-device, hardware-key signed JWT.

- Web interface on device, for device commissioning and gateway administration. 

- VPN connectivity to cloud through a secure IPSec VPN gateway.

- In-memory graph database and in-memory stream processing, for ML prediction events (using _Bitsy_/_Hazelcast_) 


![Edge cloud](../images/edge-cloud.png)


## Write Coalescing

The edge-cloud implements write coalescing with compression techniques, to simplify device logic and to reduce bytes transmitted in the bandwidth-constrained route from edge to cloud.

The following describes the write coalescing implementation by **devices**, **edge-cloud gateway**, and **cloud API endpoint**.

- **devices** - Devices will sample and transmit single datapoints to the edge cloud through a simple recurring schedule. 

    The schedule interval can be configured for each datapoint depending on its sensitivity.

    Each datapoint represents a single data element in the complete API message schema.

    No further logic needs to be implemented on-device. This makes it possible and easy to implement dynamic logic for traffic optimisation in the edge cloud. 


- **endpoint** - The edge-cloud endpoint will limit the received messages to 'change data capture'.

    This means: if a value has changed it is stored in the transmit buffer and if it has not changed it is discarded.

    A change occurs when a value exceeds an allowed tolerance compared to its previous changed/stored value.

    Change tolerances can be set for each datapoint, as a percentage or an absolute quantity.

        
- **message broker** - Changed values are stored for later transmission in a 'incremental change message' transmit queue.

    Multiple incremental change messages are combined and written once to the Cloud API.

    Each incremental change message has a dedicated sub path in the Cloud API endpoint.

    Incremental change message transmission can be configured to be triggered by number of changes, or time interval.


    When the edge-cloud stores a change it also updates that element in its 'complete message' singleton.  

    - the edge-cloud host overlays the changed value onto a complete message persisted as a singleton.

    - the singleton structure contains all elements and complies with the message schema specified for the parent API endpoint.

    - the complete message is timestamped and sent to the cloud as a heartbeat message.

    - a heartbeat message is sent to the parent API endpoint before combined incremental change `messages are sent to each sub path. 

- **cloud API endpoint** - The Cloud API endpoint provides a sub path for each incremental change message.

    The endpoint host combines the incremental change message with the hearbeat message to form a complete message.

    One of each incremental change message type (if present) is combined with a heartbeat message in a time window.

    The time window is defined by the set of unique incremental change messages (one of each type) and the first heartbeat received (in event time) within that window.

    Multiple complete message will be produced if multiple incremental change messages of a given type existed in a time window.

    The combined messages are replayed to the cloud API host and stored as complete time-series data records.

---
