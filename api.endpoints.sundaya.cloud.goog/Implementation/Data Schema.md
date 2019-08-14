# Data Schema
---

### Device Datasets - Metamodel
---

Device data consists of two types of datasets for trackable items:

- **monitoring data** - time-series datasets collected through a recurring schedule. Typically the data consists of device metrics streamed from a device controller (BBC) or device gateway (EHub) in near-real-time. The data is appended to a persistent log and is never modified. 

- **transaction data** - these datasets contain business transactions including data inserts and updates. The data is changed (created or modified) based on a business events such as procurement, installation, and servicing. The data is typically sent through the Sales Portal app when a transaction is completed, or periodically (e.g. twice a day) through a data file for batch update. As the data is replicated and therefore latent, it can not be used in time-critical processes which depend on data consistency, such as real-time analytics or time-window based data aggregation.

- **master data** - the core data needed to uniquely define the other datasets, includign data and metadata for customers, sites, partners, suppliers, personnel, products and services. This includes product metadata from vendors. Data in this dataset is changed very infrequently.

![Devices metamodel](../images/DevicesMetamodel.png)
![PMS Data](../images/PMSData.png)
![PMS Composite](../images/PMSComposite.png)
![MPPT Data](../images/MPPTData.png)
![Inverter Data](../images/InverterData.png)


