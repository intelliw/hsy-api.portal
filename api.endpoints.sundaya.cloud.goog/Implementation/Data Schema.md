# Device Data Metamodel
---

Device data consists of three dataset types for trackable items, shown in the model below. 

A _trackable_ item provides datasets which allow the item to be tracked in real-time or through retrospective reports. The data is used to trace problems and trends in the item's supply chain, operational performance, maintenance, e.t.c.
 
  ![Devices metamodel](../images/DevicesMetamodel.png)

### Dataset Types

- **monitoring data** - time-series datasets collected through a recurring schedule. Typically the data consists of device metrics streamed from a device controller (BBC) or device gateway (EHub) in near-real-time. The data is appended to a persistent log and is never modified. 

- **transaction data** - these datasets contain business transactions including data inserts and updates. The data is changed (created or modified) based on a business events such as procurement, installation, and servicing. The data is typically sent through the Sales Portal app when a transaction is completed, or periodically (e.g. twice a day) through a data file for batch update. As the data is replicated and therefore latent, it can not be used in time-critical processes which depend on data consistency, such as real-time analytics or time-window based data aggregation.

- **master data** - the core data needed to uniquely define the other datasets, includign data and metadata for customers, sites, partners, suppliers, personnel, products and services. This includes product metadata from vendors. Data in this dataset is changed very infrequently.

### Trackable Items (devices, components, assemblies)

Trackable items are composites made of the following types:

- **component** - a device component which needs to identified and tracked is considered to be a trackable item. This includes device controllers and MOSFET boards. 

- **device** - a device is an item which provides core functionality in the energy management domain. These include inverters, batteries and appliances. 


# PMS Data
---

A PMS system consists of 1-4 Cabinets with 1 Busbar in each Cabinet.

Each Busbar docks 4-12 **Cases**.

- Case and Pack data are related through a shared **id**.
- Pack data includes the Busbar **dock** number.

A Case contains 14 **Cell Blocks**, 1 **Fet Board**, and 1 **Acqu. Board**. 

PMS data consists of two distinct datasets, _Monitoring_ data and _Transaction/Master_ data, which are joined through a relationship as shown in the model below:

  ![PMS Data](../images/PMSData.png)

### Monitoring Data

PMS monitoring data is schedule-driven (e.g. every 5 seconds), is append-only, and is time-series data.

The PMS recurring dataset consists of data from:
- 4-48 **Packs**

- 14 **Cells** per Pack

- 2 **Fets** per Pack

The dataset is collected through a recurring schedule and appended to a time-series log. 

Entities in the Monitoring Dataset are linked to Master data through a shared **Pack id** and **Case Id** respectively. 

### Master/Transaction Data 

Unlike PMS monitoring data, master data is event-driven, has inserts and updates, and is infrequently changed.

The PMS master and transaction data includes Cases and Cabinets (Ehub or BBC) assemblies, including a record of changes when components are procured, or swapped.

A **Case** contains:
14 **Cell Blocks**
1 **Acqu. Board**
1 **Fet Board** containing 2 **Fets**

A **Cabinet** contains:
1 **EHub**
1 **Busbar** with 4-12 **Cases**

A Case links to Pack data through a shared ID. The human readable ID is stored on each Case's Acqu. Board.

A PMS can have 1-4 Cabinets. If there are multiple Cabinets the same PMS id will be shared and stored on each Cabinet's EHub.

### Key Identifiers

The PMS dataset depends on two identifiers, the rest of the dataset consists entirely of data needed for monitoring and analytics.

**Pack id**

- The **Pack id** _is_ the **Case id** (laser engraved in human-readable form on outside of case).

- Initially (temporarily) the **Pack id** will be populated with the **Acqu Board id** and converted through a lookup to the **Case id** on the server.

- In future the **Case Id** will be directly stored on the **Acqu Board** during pre-shipment configuration. No further data transformation will be required for determining the Pack id during data collection.

**PMS id**

- The **PMS id** is configured for each site during pre-shipment configuration.

- It is stored on each **Ehub** (or BBC); upto 4 EHubs (i.e. Cabinets) can have the same PMS id.

- It allows Packs to be hot-swapped on site without any re-configuration needed.










![PMS Composite](../images/PMSComposite.png)
![MPPT Data](../images/MPPTData.png)
![Inverter Data](../images/InverterData.png)


