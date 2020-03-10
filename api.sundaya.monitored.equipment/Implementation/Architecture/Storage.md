# Storage
---

Storage is provided by multiple cloud-native repositories based on the the principle of [polyglot persistance](https://martinfowler.com/bliki/PolyglotPersistence.html) and heterogenous data mnanagement requirements for each dataset.

- **Repository** - The best storage solution for each dataset should be based on trade-off analysis of characteristics such as cost, mutability, scale, and velocity, which vary significantly in each case. 

- **Contant** - Similarly the content model (schema and data separability) is defined by the storage technology and its constraints on critical data access requirements of intended applications.

---

### Data metamodel

The data metamodel below reflects the high-level relationship between repository technologies and the content model.

![Data metamodel](/images/dataset-metamodel.png)


---


### Content 

The following table enumerates all datasets present in the solution according to stereotypes in the above metamodel, and their primary application. 


Dataset | Repository | Content | Application
--- | --- | --- | ---
[pms_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/pms_monitoring)<br>[mppt_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/mppt_monitoring)<br>[inverter_monitoring](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/inverter_monitoring) | `monitoring`, <br>`analytics` | `telemetry`, `status` | `OI dashboard`<br>`BI dashboard`
[period](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/reporting/period) | `reporting` | `telemetry`, `period` | `Energy API`
[agent_operations](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/graph/agent_operations) | `graph` | `customer`, `operations` | `Agent portal`
[site](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/reference/site)<br>[installation](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/reference/installation)<br>[pms_pack](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/system/pms_pack)<br>[source](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/reference/source) | `reference` | `customer`, `system` |



The **Content** types are listed and described below.


- **telemetry** - this is the sensor data sent from devices to applications about the monitored environment. This data is read-only and is sent in one of the following methods.

    1. As a complete dataset sent at a frequent interval, including unchanged data.

    2. As `change-data-capture` where data is transmitted only when a change is detected.  This uses [write coalescing](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Edge%20Cloud) to compresses and reduce write traffic from edge to cloud.
    
- **status** - status information describes the state of the data collection equipment, not the business-functional environment. This information can be read/write and can also be updated, but usually not frequently.
 
- **event** - events are produced from telemetry and status data at the edge, based on rules or predictions. Rule-based events select variables from the data according to configurable parameters. Predicted events are based on features in the data which are applied to downloaded ML models. 

- **period** - period data is aligned with the epoch (starting millisecond) of categorical periods such as 'month', 'week', 'dayt' and 'timeofday'. Canonical periods are described in the [energy API](/docs/api.sundaya.monitored.equipment/0/c/Getting%20Started/API%20Overview/Energy%20API) overview page.

- **system** - the _system_ dataset contains configuration data for the data management platform, including data needed for security, traceability, and data provenance. 

    It includes script parameters and configuration data for provisioning and commissioning devices.


---


### Repositories

Data from devices are ingested and stored in three technologically differentiated repositories (**monitoring**, **analytics**, **reporting**), and combined through joins with relatively static (master) datasets in two secondary repositories (**reference**, **system**). 

These were depicted in the model above and are described in more detail below.

- **monitoring** - the _monitoring_ repository is a transient data store for streaming device data, and is used to monitor field devices in real time. 

    Data is streamed into an API endpoint by device controllers (BBC) or a device gateways (EHub) in near-real-time. 

    Once received at the endpoint the raw data is logged and held in the logging subsystem, and is available for monitoring devices in the **OI dashboard**.
    
    The data is produced by a rolling appender and purged after about 6 weeks. 

- **analytics** - datasets in the _analytics_ repository track device performance over time, and enable problem tracing and trend analysis, including predictions.

    Data is consumed from the streaming queue and stored in a relational format. The data may be joined with other datasets and accessed through SQL queries for anaytics (OLAP) in the **BI dashboard**.
    
    Data rows are append-only and never modified. 

    
- **reporting** - the _reporting_ repository contains periodic aggregates of data aligned to time windows: for example energy data totals for a week.

    The datasets are produced with low latency and high throughput from the streaming queue, by parallel processors. 

    However some data may arrive late, often due to poor connectivity seen in remote locations, or when a data backlog is sent after the system is offline due to maintenance etc. The stream processors are able to align these late-arriving data with previously processed time windows.

    The data is stored in a denormalised wide-column database for fast access by **API producer** services and transactional systems (OLTP).


- **filesystem** - the _filesystem_ repository contains _rerference_ data for customers, suppliers, personnel, sites, products and services. 

    This dataset is expected to change very infrequently. Data is inserted and updated through Apps and the web tier when a transaction is completed, or periodically (e.g. twice a day) through a batch data file exported from stand-alone systems, such as the ERP system. 
    
    The reference data is stored as sheets or JSON in a document database.

- **graph** - the _graph_ dataset contains graph-like information about _customer_ and _operations_ entities and their relationships, such as the sales and service network.

    The dataset is materialised from content and links held in sheets. The data is stored in the same wide-column database cluster used for **reporting** data

    _graph_ data is retrieved using graph query language in the **Sales portal** implementation.


---

### Analytics dataset partitioning

All `analytics` dataset tables are partitioned based on the `time_event` field, into daily segments, to reduce cost and improve performance. 

Queries require a mandatory predicate filter (a WHERE clause) for the `time_event` attribute to limit the number of partitions scanned, as shown in this example.

```sql
SELECT 	pms_id, pack.id, cell.vcl, cell.vch, cell.dvcl
FROM `sundaya.analytics.pms_monitoring`
WHERE time_event BETWEEN '2020-02-08' AND '2019-02-12'
AND pms_id IN ('PMS-01-002', 'PMS-01-002')
```

`monitoring` dataset tables are further clustered based on the contents of the primary key column (`pms_id`, `mppt_id`, `inverter_id`).

Queries should filter on the clustered key column as shown in the above SQL example as this improves performance and reduces cost.

---
