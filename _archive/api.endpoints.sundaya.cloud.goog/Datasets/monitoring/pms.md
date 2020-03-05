# monitoring.pms
---

# Dataset Source

A PMS system consists of 1-4 Cabinets with 1 Busbar in each Cabinet.

Each Busbar docks 4-12 **Cases**.

- Case and Pack data are related through a shared **id**.
- Pack data includes the Busbar **dock** number.

A Case contains 14 **Cell Blocks**, 1 **Fet Board**, and 1 **Acqu. Board**. 

PMS data consists of two distinct datasets, _Monitoring_ data and _Transaction/Master_ data, which are joined through a relationship as shown in the model below:

![PMS System](../../images/PMSComposite.png)
### Monitoring Data

PMS monitoring data streams in as a time-series unbounded dataset. The data is schedule-driven (e.g. every 5 seconds) and is append-only.

Monitoring data is collected from:
- 4-48 **Packs**
- 14 **Cells** per Pack
- 2 **Fets** per Pack

The dataset is collected through a recurring schedule and appended to a time-series log. 

The Monitoring Dataset links to Master data through a shared key in **PMS id** and **Case Id** respectively. 

![PMS Data](../../images/PMSData.png)

### Transaction/Master Data 

Unlike PMS monitoring data, master data is event-driven, has inserts and updates, and is infrequently changed.

The PMS master and transaction data includes Cases and Cabinets (Ehub or BBC) assemblies, including a record of changes when components are procured, installed, or swapped.

A **Case** contains:
- 14 **Cell Blocks**
- 1 **Acqu. Board**
- 1 **Fet Board** containing 2 **Fets**

A **Cabinet** contains:
- 1 **EHub**
- 1 **Busbar** with 4-12 **Cases**

A Case links to Pack data through a shared ID. The human readable ID is stored on each Case's **Acqu. Board**.

A PMS can have 1-4 Cabinets. If there are multiple Cabinets the same **PMS id** will be shared and stored on each Cabinet's **EHub**.


### Identifiers

The PMS dataset depends on two identifiers, the rest of this dataset consists entirely of data needed for monitoring and analytics.

**Pack id**

- The **Pack id** _is_ the **Case id** (laser engraved in human-readable form on outside of case).

- Initially (temporarily) the **Pack id** will be populated with the **Acqu Board id** and converted through a lookup to the **Case id** on the server.

- In future the **Case Id** will be directly stored on the **Acqu Board** during pre-shipment configuration. No further data transformation will be required for determining the Pack id during data collection.

**PMS id**

- The **PMS id** is configured for each site during pre-shipment configuration.

- It is stored on each **Ehub** (or BBC); upto 4 EHubs (i.e. Cabinets) can have the same PMS id.

- It allows Packs to be hot-swapped on site without any re-configuration needed.

---

# Request Message

The following snippet shows the structure of a `pms` request:

```json
{
  "datasets": [
    { "pms": { "id": "PMS-01-001" }, 
      "data": [
        { "time_local": "20190209T150006.032+0700",
          "pack": { "id": "0241", "dock": 1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.91] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] } }
        },
```

### Message Attributes 

The pms request message attributes are specified below. 

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | --- 
`pms.id` | - | string | - | The `pms.id` is configured for each site during pre-shipment. It is stored on each Ehub (or BBC); upto 4 EHubs (i.e. Cabinets) can share the same `pms.id`. The id is the foreign key for relationally joining the pms dataset to reference data in the cloud. As packs self identify they cab be hot-swapped on site without any need for re-configuring pack metadata.
`time_local` | - | datetime | RFC 3339 | The local time of the event which produced this data sample, represented with a mandatory `+/-` offset from UTC for the device's location, in compressed `ISO 8601/RFC3339` (YYYYMMDDThhmmss±hhmm).
`pack.id` | - | string | - | The pack identifier. This is the Case id which is laser engraved in human-readable form on the outside of the case. Initially (temporarily) the `pack.id` will be populated with the pack’s acquisition board hardware id, and converted to the Case id through a lookup on the server. In future the Case id will be directly stored in the acquisition board during pre-shipment configuration and no further data transformation will be required for determining the Pack id during data collection.
`pack.dock` | - | integer | 1-48 | The dock number (1 - 48) into which this pack is installed. The dock number covers 4 cabinets with 12 docks per cabinet in sequence. For example dock number 13 indicates that the pack is installed in dock 1 of cabinet B (the second cabinet).
`pack.amps` | amps | float [+/-] | - | The current draw for this pack. The value is positive for charge current and negative when discharging. 
`pack.temp` | degC | float *(array)* | *array size 3* | An ordered set of 3 temperature readings in degC, for the temperature of the cell-pack at its top, middle, and bottom. The array position of  each value is significant: the 1st value applies to the top of the pack, 2nd to the middle, and 3rd to the bottom. 
`cell.volts` | volts | float *(array)* | *array size 14* | An ordered set of 14 Voltage readings for 14 cellblocks in this pack. Each value in the data array applies to a cell number based on its position in the array. For example the 2nd value in the data array is the data for the 2nd cell in the pack. The pack voltage is not included in the monitoring dataset: as cellblocks are connected in series the pack voltage is calculated later as the sum of all 14 cell voltages.
`cell.open` | *open/closed* | integer *(array)* | 1-14, *uniqueitems, array size 0-14* | An unordered set of unique ordinal numbers for cells  which are ‘open’ due to cell balancing. Must contain numbers from 1-14 as there are only 14 cells in a pack. For example a data value of [1,6] signifies that cell 1 and 6 are open (bypassed) to allow the other cells in the pack to charge and reach parity with this cell block. Typically the array will be empty to indicate that there is no cell balancing in effect.
`fet.temp` | degC | float | *array size 2* | An ordered set of temperature readings for the two MOSFETS in this pack. The first is the input (CMOS) and the 2nd is the output (DMOS) MOSFET.
`fet.open` | *open/closed* | integer *(array)* | 1-2, *uniqueitems, array size 2* | An unordered set of unique ordinal numbers for FETs which are ‘open’ due to pack balancing. The data values must be a number from 1-2 as there are only 2 FETS in each pack. For example a data value of [1,2] signifies that FETS 1 and 2 are both open.
 
- Attributes marked as '*required on change*' (if any) may be omitted if the value has not changed since the last successful post (a POST is successful if the API server responds with a 200 level reply).
All other attributes are mandatory and must be present.

### text/csv Data

Requests with _content-type_ of `text/csv` must provide UTF-8 `csv` data in the request body and include a header row.

The data must include a column for each attribute listed in the __Message Attributes__ table above, including a separate column for each array item: 

- Column names for array items should be the the attribute name followed by the array item number.

- For example `cell.volts.1`, `cell.volts.2` etc. to represent each of the 14 elements in the `cell.volts` array atttribute.

A valid __csv__ data sample including a header row and a data row is shown below. 

```csv
pms.id,time_local,pack.dock,pack.id,pack.dock,pack.amps,pack.temp.1,pack.temp.2,pack.temp.3,cell.volts.1,cell.volts.2,cell.volts.3,cell.volts.4,cell.volts.5,cell.volts.6,cell.volts.7,cell.volts.8,cell.volts.9,cell.volts.10,cell.volts.11,cell.volts.12,cell.volts.13,cell.volts.14,cell.open.1,cell.open.2,cell.open.3,cell.open.4,cell.open.5,cell.open.6,cell.open.7,cell.open.8,cell.open.9,cell.open.10,cell.open.11,cell.open.12,cell.open.13,cell.open.14,fet.temp.1,fet.temp.2,fet.open.1,fet.open.2
025,20190820T115900.000+0700,6,269,6,-1.3,33,32,31,3.793,3.796,3.78,3.788,3.797,3.795,3.792,3.796,3.788,3.779,3.795,3.795,3.788,3.793,,,,,,,,,,,,,,,30,29,0,0
```

---

# Dataset Structure 

While the request message structure described above is optimised to reduce size, the dataset structure is optimised to simplify queries for analytics. 

In particular the arrays in the request message structure are flattened and transformed into the following dataset structure, which includes additional elements.

- The added timestamps are based on `time_local` sent in the request message, which is replaced by these timestamps.  

- The timestamps are stored in the canonical format used for data storage ('YYYY-MM-DD HH:mm:ss.SSSS') as shown in the examples.

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | ---
`pms_id` | - | string | - | Id of the PMS system, as displayed on the cabinet. This attribute replaces `pms.id` in the request message.
`pack_id` | - | string | - | Id of the `pack` associated with this data record. This attribute replaces `pack.id` as a top-level attribute, so that it can be included in the data clustering specification.
`pack.volts` | volts | float | - | The cellblocks are connected in series so the pack voltage is the sum of all 14 cell voltages (`cell.volts[1-14]`).
`pack.amps` | amps | float | - | _(no change from request message)_.
`pack.watts` | watts | float | - | The product of `pack.volts` and `pack.amps`.
`pack.vcl` | volts | float | - | The lowest cell voltage in `cell.volts`.
`pack.vch` | volts | float | - | The highest cell voltage in `cell.volts`.
`pack.dock` | - | integer | 1-48 | _(no change from request message)_.
`temp_top` | degC | float | - | The 1st element in `pack.temp`.
`temp_mid` | degC | float | - | The 2nd element in `pack.temp`.
`temp_bottom` | degC | float | - | The 3rd element in `pack.temp`.
`cell_nn_volts` | volts | float | - | The element number corresponding to nn in `cell.volts`.
`cell_nn_dvcl` | millivolts | float *(array)* | - | The millivolts difference (delta) between the lowest cell voltage (`cell.vcl`) and each cell voltage in `cell.volts`.
`cell_nn_open` | open/closed | integer | 1/0 | 1 if any element in `cell.open` contains the value nn, otherwise 0.
`fet_in.open` | open/closed | integer | 1/0 | 1 if any element in `fet.open` contains the value 1 as corresponds to `fet_in`, otherwise 0.
`fet_out.open` | open/closed | integer | 1/0 | 1 if any element in `fet.open` contains the value 2 as corresponds to `fet_out`, otherwise 0.
`fet_in.temp` | degC | float | - | The 1st element in `fet.temp`.
`fet_out.temp` | degC | float | - | The 2nd element in `fet.temp`.
`sys.source` | - | string | - | The identifier of the data sender, based on the API key sent in the request header. The value is a foreign key to the `system.source` dataset table.
`time_event` | - | datetime | - | The UTC time of the event which produced this data sample.
`time_local` | - | datetime | - | The local time of the event which produced this data sample. Note that the timezone offset is discarded.
`time_processing` | - | datetime | - | The UTC time when the request was received and *processed* on the API host.


The transformed JSON structure which is sent to the message broker at the first stage of processing the `dataset/pms` POST message, and which is used to load data into the datawarehouse, is shown in the followng sample:

```
*** MESSAGE ***
Topic: pms
Key: PMS-01-001
Value:	
```

```json
{   "pms_id": "PMS-01-002",
    "pack_id": "0248",
    "pack": { "volts": 51.262, "amps": -0.625, "watts": -32.039, 
        "vcl": 3.654, "vch": 3.676, "dock": 4, 
        "temp_top": 35, "temp_mid": 33, "temp_bottom": 34 },
    "cell_01": {"volts": 3.661, "dvcl": 7, "open": 0 },
    "cell_02": {"volts": 3.666, "dvcl": 12, "open": 0 },
    "cell_03": {"volts": 3.654, "dvcl": 0, "open": 0},
    "cell_04": {"volts": 3.676, "dvcl": 22, "open": 0 },
    "cell_05": {"volts": 3.658, "dvcl": 4, "open": 0 },
    "cell_06": {"volts": 3.662, "dvcl": 8, "open": 0 },
    "cell_07": {"volts": 3.660, "dvcl": 6, "open": 0 },
    "cell_08": {"volts": 3.659, "dvcl": 5, "open": 0 },
    "cell_09": {"volts": 3.658, "dvcl": 4, "open": 0 },
    "cell_10": {"volts": 3.657, "dvcl": 3, "open": 0 },
    "cell_11": {"volts": 3.656, "dvcl": 2, "open": 0 },
    "cell_12": {"volts": 3.665, "dvcl": 11, "open": 0 },
    "cell_13": {"volts": 3.669, "dvcl": 15, "open": 0 },
    "cell_14": {"volts": 3.661, "dvcl": 7, "open": 0},
    "fet_in": {"open": 1, "temp": 34.1 },
    "fet_out": {"open": 0, "temp": 32.2 },
    "sys": {"source": "S000" },
    "time_event": "2019-02-09 08:00:17.0200",
    "time_local": "2019-02-09 15:00:17.0200",
    "time_processing": "2019-09-08 05:00:48.9830"        
},
```

### Partitions and Clustering

__pms__ dataset tables are partitioned based on `time_local` and clustered by `pms_id` and `pack_id`.