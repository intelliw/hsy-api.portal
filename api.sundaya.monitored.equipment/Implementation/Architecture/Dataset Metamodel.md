# Dataset Metamodel
---

Trackable devices are assocaited with four datasets, described below. 


Data for a _trackable_ item is used to track device performance in real-time, and to trace problems and trends, including predictions.
 
![Devices metamodel](/images/DevicesMetamodel.png)
### Dataset Types

- **timeseries** - the `timeseries` dataset includes _telemetry_ and _status_ data streaming in from field devices. 

The dataset is infinite and unordered. Typically this dataset is streamed through a device controller (BBC) or a device gateway (EHub) in near-real-time. 

However some data may arrive late, often due to poor connectivity seen in remote locations, or when a data backlog is sent after the system is offline due to maintenance etc. 

 Data rows are append-only and never modified. This dataset is used mainly for anaytics (OLAP) in operational dashboards. The data is stored in a relational format and can be accessed through SQL queries.

- **alignment** - the `alignment` dataset contains aggregates of time-series data, aligned to data windows such as 'periods': for example energy data totals for a week. This dataset is produced by parallel stream processors for low latency and high throughput. The data is stored in a denormalised column-database for fast access by API services and transactional systems (OLTP).

- **reference** - the `reference` dataset contains _master_ data for customers, suppliers, personnel, sites, products and services. Typically this dataset changes very infrequently. Data is inserted and updated through Apps and the web tier when a transaction is completed, or periodically (e.g. twice a day) through a batch data file exported from a stand-alone system, such as the ERP system. The reference data is stored as sheets or JSON in a document database. 

- **system** - the `sytstem` dataset contains configuration data for the data management platform, including data needed for security, traceability, and data provenance. It includes scripts and data for provisioning and commissioning devices.

### Trackable Devices (devices, components, assemblies)

Trackable devices are composites made of the following types:

- **component** - a device component which needs to identified and tracked is considered to be a trackable item. This includes device controllers and MOSFET boards. 

- **device** - a device is an item which provides core functionality in the energy management domain. These include inverters, batteries and appliances. 

- **assembly** - an assembly can contain devices, components, and sub-assemblies. These items are typically assembled or logically configured before shipment. They include battery cases and cabinets. 

---

### Partitioning and Clustering

All `timeseries` dataset Tables are partitioned based on the `time_event` field, into daily segments, to reduce cost and improve performance. 

Queries require a mandatory predicate filter (a WHERE clause) for the `time_event` attribute to limit the number of partitions scanned, as shown in this example.

```sql
SELECT 	pms_id, pack.id, cell.vcl, cell.vch, cell.dvcl
FROM `sundaya.timeseries.pms`
WHERE time_event BETWEEN '2020-02-08' AND '2019-02-12'
AND pms_id IN ('PMS-01-002', 'PMS-01-002')
```

`timeseries` dataset tables are further clustered based on the contents of the primary key column (`pms_id`, `mppt_id`, `inverter_id`).

To optimize performance and cost queries should use an expression that filters on the clustered key column, as shown in the above SQL example.
