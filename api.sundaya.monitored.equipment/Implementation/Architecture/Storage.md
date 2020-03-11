# Storage


Storage is provided by multiple cloud-native repositories based on the the principle of [polyglot persistance](https://martinfowler.com/bliki/PolyglotPersistence.html) and heterogenous data mnanagement requirements for each dataset.

---

#### Data metamodel

The data metamodel defines high-level relationships between _Repository_ technologies and the _Content_ model.

![Data metamodel](/images/dataset-metamodel.png)

- **Repository** - The best storage solution for each dataset will be based on trade-offs in characteristics such as cost, mutability, scale, and velocity; which vary significantly in each case. 

- **Content** - Similarly the content model (schema and data separability) is defined by the storage technology and its constraints on critical data access requirements of intended applications.


---

# Repositories

Data is stored in five technologically differentiated repositories as depicted in the metamodel above.

Data is ingested and initially stored into three repositories (_monitoring_, _analytics_, _reporting_), and combined through joins with relatively static (master) data in two secondary repositories (_reference_, _system_). 

_Repository_ archetypes and their abbreviated menmonics and qualifiers (which are used in [naming conventions](/docs/api.sundaya.monitored.equipment/0/c/Getting%20Started/Start%20Here/Standards%20&%20Conventions)) are listed below:

Repository          | Repository Mnemonic       | Qualifier     | Qualifier Name
---                 | ---                       | ---           | ---
**monitoring**      | `mon`                     | `std`         | _Stackdriver_
**analytics**       | `any`                     | `bq`          | _BigQuery_
**reporting**       | `rpt`                     | `bt`<br>`ds`  | _Bigtable_<br>_Datastore_   
**nearline**        | `nl`                      | `gcs`         | _Google Cloud Storage_
**graph**           | `gr`                      | `jg`<br>`fs`  | _JanusGraph_<br>_Firestore_


- **monitoring** - The _monitoring_ repository is a transient data store for streaming device data, and is used to monitor field devices in real time.<br><br>Data is streamed into an API endpoint by device controllers (BBC) or a device gateways (EHub) in near-real-time.<br><br>Once received at the endpoint the raw data is logged and held in the logging subsystem, and is available for monitoring devices in the **OI dashboard**.<br><br>The data is produced by a rolling appender and purged after about 6 weeks.<br><br>

- **analytics** - Datasets in the _analytics_ repository track device performance over time, and enable problem tracing and trend analysis, including predictions.<br><br>Data is consumed from the streaming queue and stored in a relational format. The data may be joined with other datasets and accessed through SQL queries for anaytics (OLAP) in the **BI dashboard**.<br><br>Data rows are append-only and never modified.

- **reporting** - The _reporting_ repository contains periodic aggregates of data aligned to time windows: for example energy data totals for a week.<br><br>The datasets are produced with low latency and high throughput from the streaming queue, by parallel processors.<br><br>However some data may arrive late, often due to poor connectivity seen in remote locations, or when a data backlog is sent after the system is offline due to maintenance etc. The stream processors are able to align these late-arriving data with previously processed time windows.<br><br>The data is stored in a denormalised wide-column database for fast access by **API** and other application services and transactional systems (OLTP).

- **nearline** - The _nearline_ data is typically held in cloud storage or other file system, and contains _rerference_ data for customers, suppliers, personnel, sites, products and services.<br><br>This dataset is expected to change very infrequently. Data is inserted and updated through Apps and the web tier when a transaction is completed, or periodically (e.g. twice a day) through a batch data file exported from stand-alone systems, such as the ERP system.<br><br>The reference data is stored as sheets or JSON documents.

- **graph** - The _graph_ dataset contains graph-like information about _customer_ and _operations_ entities and their relationships, such as the sales and service network.<br><br>The dataset is materialised from content and links held in sheets. The data is stored in the same wide-column database cluster used for _reporting_ data.<br><br> _graph_ data is retrieved using graph query language in the **Sales portal** implementation.

---

# Content 

_Content_ types and their abbreviated menmonics and qualifiers are listed below:

Contant             | Content menmonics | Qualifier                     | Qualifier Name
---                 | ---               | ---                           | ---
**telemetry**       | `tel`             | `pms`<br>`mppt`<br>`inv`      | _Pack Management System_<br>_Maximum Power Point Tracker_<br>_Inverter_ 
**status**          | `sts`             | `dev`                         | _Device_
**event**           | `evt`             | `kpi`                         | _Key Performance Indicator_
**energy**          | `eng`             |                               |     
**system**          | `sys`             |                               |    


- **telemetry** - _telemetry_ data consists of sensor data about the monitored devices and environment. The data is read-only/ append-only. The data is sent in one of the following methods<br><br>1. As a complete dataset sent at a frequent interval, including unchanged data.<br>2. As a partial dataset for _change-data-capture_, and data transmission only when a change is detected.<br><br>_telementry_ data is processed using [write coalescing](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Edge%20Cloud) to compresses and reduce write traffic from edge to cloud. 

- **status** - _status_ data describes the state of the monitoring equipment, not the business-functional data and environment.<br><br>The data is read-only /append-only.

- **event** - _event_ data is produced from telemetry and status data at the edge or in the cloud, based on rules and ML predictions.<br><br>Updateable configurations are used to select variables (features) which are impacted during data collection by rules to generate events.

- **energy** - _energy_ data contains _telemetry_ energy aggregates which are aligned with an epoch (starting millisecond) of a categorical period such as 'month', 'week', 'day' and 'timeofday'.<br><br>Canonical periods are described in the [energy API](/docs/api.sundaya.monitored.equipment/0/c/Getting%20Started/API%20Overview/Energy%20API) overview page.

- **system** - The _system_ dataset contains configuration data for the platform, including paramters needed for security, traceability, and data provenance.<br><br>It includes script parameters and configuration data for provisioning and commissioning devices.

---

# Datasets 

The following table enumerates all datasets present in the solution according to stereotypes in the above metamodel, and their primary application. 

Dataset | Repository | Content | Application
--- | --- | --- | ---
[pms_telemetry](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/pms_telemetry)<br>[mppt_telemetry](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/mppt_telemetry)<br>[inverter_telemetry](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/analytics/inverter_telemetry) | `monitoring`, <br>`analytics` | `telemetry`, `status` | `OI dashboard`<br>`BI dashboard`
[period](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/reporting/period) | `reporting` | `energy`, `telemetry` | `Energy API`
[agent_operations](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/graph/agent_operations) | `graph` | `customer`, `operations` | `Agent portal`
[site](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/reference/site)<br>[installation](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/reference/installation)<br>[pms_pack](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/system/pms_pack)<br>[source](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Datasets/reference/source) | `reference` | `customer`, `system` |

---





---

### Analytics dataset partitioning

All `analytics` dataset tables are partitioned based on the `time_event` field, into daily segments, to reduce cost and improve performance. 

Queries require a mandatory predicate filter (a WHERE clause) for the `time_event` attribute to limit the number of partitions scanned, as shown in this example.

```sql
SELECT 	pms_id, pack.id, cell.vcl, cell.vch, cell.dvcl
FROM `sundaya.analytics.pms_telemetry`
WHERE time_event BETWEEN '2020-02-08' AND '2019-02-12'
AND pms_id IN ('PMS-01-002', 'PMS-01-002')
```

`monitoring` dataset tables are further clustered based on the contents of the primary key column (`pms_id`, `mppt_id`, `inverter_id`).

Queries should filter on the clustered key column as shown in the above SQL example as this improves performance and reduces cost.

---

