# Platform Overview

---
# Device Data Metamodel
---

Device data consists of three dataset types for trackable devices, shown in the model below. 

A _trackable_ item provides datasets which allow the item to be tracked in real-time or through retrospective reports. The data is used to trace problems and trends in the item's supply chain, operational performance, maintenance, etc.
 
![Devices metamodel](../images/DevicesMetamodel.png)
### Dataset Types

- **monitoring data** - time-series datasets collected through a recurring schedule. Typically the data consists of device metrics streamed from a device controller (BBC) or device gateway (EHub) in near-real-time. The data is appended to a persistent log and is never modified. 

- **transaction data** - these datasets contain business transactions including data inserts and updates. The data is changed (created or modified) based on a business events such as procurement, installation, and servicing. The data is typically sent through the Sales Portal app when a transaction is completed, or periodically (e.g. twice a day) through a data file for batch update. As the data is replicated and therefore latent, it can not be used in time-critical processes which depend on data consistency, such as real-time analytics or time-window based data aggregation.

- **master data** - the core data needed to uniquely define the other datasets, includign data and metadata for customers, sites, partners, suppliers, personnel, products and services. This includes product metadata from vendors. Data in this dataset is changed very infrequently.

### Trackable Devices (devices, components, assemblies)

Trackable devices are composites made of the following types:

- **component** - a device component which needs to identified and tracked is considered to be a trackable item. This includes device controllers and MOSFET boards. 

- **device** - a device is an item which provides core functionality in the energy management domain. These include inverters, batteries and appliances. 

- **assembly** - an assembly can contain devices, components, and sub-assemblies. These items are typically assembled or logically configured before shipment. They include battery cases and cabinets. 



# /devices/dataset POST
---

The `/devices/dataset/{dataset}` path is for vendors and systems integrators to POST device data.

[http:/api.endpoints.sundaya.cloud.goog/devices/{dataset}](http:/api.endpoints.sundaya.cloud.goog/devices/dataset/pms)

- This route allows device **controllers** (e.g. Bus Bar Controller) and **gateways** (e.g. EHub Gateway) to accumulate and periodically send data from multiple monitored devices installed at a site.

### {dataset} parameter ###

The following types may be specified as the `{dataset}` parameter in a `/device` path.

dataset | Description
--- | --- | --- 
`pms` | Data from pack management systems (PMS) for cabinets, including monitored epacks, cells, and mosfets.
`mppt` | Data from Maximum Power Point Tracking (MPPTcontrollers, including connected PV strings, batteries, and DC loads.
`inverter` | Data from Inverter charge controllers, including connected pv strings, batteries, and AC loads.


### 'pms' Body parameter

The following snippet shows the structure of a `pms` dataset:

```json
{
  "datasets": [
    { "pms": { "id": "PMS-01-001" }, 
      "data": [
        { "time_local": "20190209T150006.032+0700",
          "pack": { "id": "0241", "dock": 1, "volts": "55.1", "amps": "-1.601", "temp": ["35.0", "33.0", "34.0"],
            "cell": { "open": [1, 6],
              "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.91"] },
            "fet": { "open": [1, 2], "temp": ["34.1", "33.5"] } }
        },
```

The dataset attributes are specified below. 

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | --- 
`pms.id` | - | string | - | The `pms.id` is configured for each site during pre-shipment. It is stored on each Ehub (or BBC); upto 4 EHubs (i.e. Cabinets) can share the same `pms.id`. The id is the foreign key for relationally joining the pms dataset to reference data in the cloud. As packs self identify they cab be hot-swapped on site without any need for re-configuring pack metadata.
`time_local` | - | datetime | RFC 3339 | The local time of the event which produced this data sample, represented with a mandatory `+/-` offset from UTC for the device's location, in compressed `ISO 8601/RFC3339` (YYYYMMDDThhmmss±hhmm).
`pack.id` | - | string | - | The pack identifier. This is the Case id which is laser engraved in human-readable form on the outside of the case. Initially (temporarily) the `pack.id` will be populated with the pack’s acquisition board hardware id, and converted to the Case id through a lookup on the server. In future the Case id will be directly stored in the acquisition board during pre-shipment configuration and no further data transformation will be required for determining the Pack id during data collection.
`pack.dock` | - | integer | 1-48 | The dock number (1 - 48) into which this pack is installed. The dock number covers 4 cabinets with 12 docks per cabinet in sequence. For example dock number 13 indicates that the pack is installed in dock 1 of cabinet B (the second cabinet).
`pack.volts` | volts | float | - | The voltage of this pack.
`pack.amps` | amps | float [+/-] | - | The current draw for this pack. The value is positive for charge current and negative when discharging. 
`pack.temp` | degC | float *(array)* | *array size == 3* | An ordered set of 3 temperature readings in degC, for the temperature of the cell-pack at its top, middle, and bottom. The array position of  each value is significant: the 1st value applies to the top of the pack, 2nd to the middle, and 3rd to the bottom. 
`cell.volts` | volts | float *(array)* | *array size == 14* | An ordered set of 14 Voltage readings for 14 cellblocks in this pack. Each value in the data array applies to a cell number based on its position in the array. For example the 2nd value in the data array is the data for the 2nd cell in the pack.
`cell.open` | *open/closed* | integer *(array)* | 1-14, *uniqueitems, array size 0-14* | An unordered set of unique ordinal numbers for cells  which are ‘open’ due to cell balancing. Must contain numbers from 1-14 as there are only 14 cells in a pack. For example a data value of [1,6] signifies that cell 1 and 6 are open (bypassed) to allow the other cells in the pack to charge and reach parity with this cell block. Typically the array will be empty to indicate that there is no cell balancing in effect.
`fet.temp` | degC | float | *array size == 2* | An ordered set of temperature readings for the two MOSFETS in this pack. The first is the input (CMOS) and the 2nd is the output (DMOS) MOSFET.
`fet.open` | *open/closed* | integer *(array)* | 1-2, *uniqueitems, array size == 2* | An unordered set of unique ordinal numbers for FETs which are ‘open’ due to pack balancing. The data values must be a number from 1-2 as there are only 2 FETS in each pack. For example a data value of [1,2] signifies that FETS 1 and 2 are both open.
 
- Attributes marked as '*required on change*' may be omitted if the value has not changed since the last successful post (a POST is successful if the API server responds with a 200 level reply).
All other attributes are mandatory and must be present.

### 'mppt' Body parameter

The following snippet shows the structure of a `mppt` dataset:

```json
{
  "datasets": [
    { "mppt": { "id": "IT6415AD-01-001" }, 
      "data": [
        { "time_local": "20190209T150006.032+0700",
          "pv": { "volts": ["48.000", "48.000"], "amps": ["6.0", "6.0"] },
          "batt": { "volts" : "55.1", "amps": "-1.601" }, 
          "load": { "volts": ["48.000", "48.000"], "amps": ["1.2", "1.2"] }
        },
```

The dataset attributes are specified below. 

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | --- 
`mppt.id` | - | string | - | Id of the MPPT charge controller. *(__Note__: a site can have any number of MPPT controllers, and each controller can have multiple PV strings and Loads)*.
`time_local` | - | datetime | RFC 3339 | The local time of the event which produced this data sample, represented with a mandatory `+/-` offset from UTC for the device's location, in compressed `ISO 8601/RFC3339` (YYYYMMDDThhmmss±hhmm).
`pv.volts` | volts | float *(array)* | *array size 1-4* | An ordered set of Voltage readings for PV strings connected to this MPPT. Each value in the data array applies to a numbered PV string based on its position in the array. For example the 2nd value in the data array is the data for the 2nd PV string. The array size depends on the number of PV strings. Presently upto 4 PV strings per controller are supported. In future each string will have its own dedicated controller.
`pv.amps` | amps | float *(array)* | *array size 1-4* | An ordered set of Current readings for PV strings (corresponding to voltage readings in `pv.volts`).  
`batt.volts` | volts | float | - | The voltage of the connected Battery.
`batt.amps` | amps | float [+/-] | - | The current for the connected Battery. The value is positive for charge current and negative when discharging.
`load.volts` | volts | float *(array)* | *array size 1-2* | An ordered set of Voltage readings for connected Load. Each value in the data array applies to a Load number based on its position in the array. For example the 2nd value in the data array is the data for the 2nd Load. The array size depends on the number of loads. Each load and its ordinal position must be declared in this API documentation. In a BTS installation Load 1 is the VSAT system and Load 2 is the BTS.
`load.amps` | amps | float *(array)* | *array size 1-2* | An ordered set of Current readings for connected Loads  (corresponding to values in `load.volts`).  

### 'inverter' Body parameter

The following snippet shows the structure of an `inverter` dataset:

```json
{
  "datasets": [
    { "inverter": { "id": "SPI-B2-01-001" }, 
      "data": [
        { "time_local": "20190209T150006.032+0700",
          "pv": { "volts": ["48.000", "48.000"], "amps": ["6.0", "6.0"] },
          "batt": { "volts" : "55.1", "amps": "-1.601" }, 
          "load": { "volts": ["48.000", "48.000"], "amps": ["1.2", "1.2"] }
        },
```

The dataset attributes are specified below. 

Attribute | Metric | Data | Optionality | Description
--- | --- | --- | --- | ---
`inverter.id` | - | string | - | Id of the Inverter charge controller. *(__Note__: a site can have any number of Inverter controllers, and each controller can have multiple PV strings and Loads)*.
`time_local` | - | datetime | RFC 3339 | The local time of the event which produced this data sample, represented with a mandatory `+/-` offset from UTC for the device's location, in compressed `ISO 8601/RFC3339` (YYYYMMDDThhmmss±hhmm).
`pv.volts` | volts | float *(array)* | *array size 1-4* | An ordered set of Voltage readings for PV strings connected to this Inverter. Each value in the data array applies to a numbered PV string based on its position in the array. For example the 2nd value in the data array is the data for the 2nd PV string. The array size depends on the number of PV strings. Presently upto 4 PV strings per controller are supported. In future each string will have its own dedicated controller.
`pv.amps` | amps | float *(array)* | *array size 1-4* | An ordered set of Current readings for PV strings (corresponding to voltage readings in `pv.volts`).  
`batt.volts` | volts | float | - | The voltage of the connected Battery.
`batt.amps` | amps | float [+/-] | - | The current for the connected Battery. The value is positive for charge current and negative when discharging.
`load.volts` | volts | float *(array)* | *array size 1-2* | An ordered set of Voltage readings for connected Loads. Each value in the data array applies to a Load number based on its position in the array. For example the 2nd value in the data array is the data for the 2nd Load. The array size depends on the number of loads. Each load and its ordinal position must be declared in this API documentation.
`load.amps` | amps | float *(array)* | *array size 1-2* | An ordered set of Current readings for connected Loads  (corresponding to values in `load.volts`).  


# /device/dataset GET
---

The `/device/{device-id}/dataset/{dataset}` path allows field engineers to monitor an individual device during operation.
 
 [http:/api.endpoints.sundaya.cloud.goog/device/{**device-id**}/dataset/{**dataset**}/period/week/20150204/1](http:/api.endpoints.sundaya.cloud.goog/device/BBC-PR1202-999/dataset/BBC-MPPT/period/week/20150204/1)

- This path provide a dedicated endpoint to retrive data for an individual **device** and dataset. 

### Path parameters

The following path parameters are required in device GET requests. If a path parameter is omitted it will be substituted as described.    

Parameter | Description 
--- | --- 
`device` | The device identifier. 
`dataset` | The dataset type (see section titled _{dataset} parameter_ above). 

### Query parameters
There are no query parameters for the `/device/dataset` route.
