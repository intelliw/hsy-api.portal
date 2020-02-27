# monitoring.pms
---

### API Consumer message ('monitoring.pms')

Each dataset item in the the `dataset/pms` POST message body 'datasets' array, is transformed into a separate JSON message as shown below. 

The consumer process transforms messages into JSON structure shown in the following sample.

Typically there will be 14 data items in each `pms` dataset, one for each pack.

This consumer sends this message to the `monitoring.pms` dataset table in the datawarehouse as an audit log, and to the `monitoring.pms.dataset` message broker topic for further stream processing.

```
*** MESSAGE ***
topic: monitoring.pms
key: PMS-01-001
value:	
```

```json
{   "pms_id": "PMS-01-002",
    "pack_id": "0248",
    "pack": { "volts": 51.262, "amps": -0.625, "watts": -32.039,    
        "vcl": 3.654, "vch": 3.676, "dock": 4, 
        "temp_top": 35, "temp_mid": 33, "temp_bottom": 34 },
    "cell": [
        { "volts": 3.661, "dvcl": 7, "open": false },
        { "volts": 3.666, "dvcl": 12, "open": false },
        { "volts": 3.654, "dvcl": 0, "open": false },
        { "volts": 3.676, "dvcl": 22, "open": false },
        { "volts": 3.658, "dvcl": 4, "open": false },
        { "volts": 3.662, "dvcl": 8, "open": false },
        { "volts": 3.66, "dvcl": 6, "open": false },
        { "volts": 3.659, "dvcl": 5, "open": false },
        { "volts": 3.658, "dvcl": 4, "open": false },
        { "volts": 3.657, "dvcl": 3, "open": false },
        { "volts": 3.656, "dvcl": 2, "open": false },
        { "volts": 3.665, "dvcl": 11, "open": true},
        { "volts": 3.669, "dvcl": 15, "open": false },
        { "volts": 3.661, "dvcl": 7, "open": false } ], 
    "fet": [
        { "open": true, "temp": 34.1 },
        { "open": false, "temp": 32.2 } ],
    "status": { "bus_connect": true, "temp": 48.3 }, 
    "sender": "S000", 
    "time_event": "2020-02-08 08:00:17.0200", 
    "time_zone": "+07:00", 
    "time_processing": "2020-02-08 08:00:48.9830"
},
```

---

# Dataset structure 

While the request message structure described above is optimised to reduce size, the dataset structure is optimised to simplify queries for analytics. 

In particular the arrays in the request message structure are flattened and transformed into the following dataset structure, which includes additional elements.

- The added timestamps are based on `time_local` sent in the request message, which is replaced by these timestamps.  

- timestamps are stored in canonical format ('YYYY-MM-DD HH:mm:ss.SSSS') as shown in the examples.

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | ---
`pms_id` | - | string | - | Id of the PMS system, as displayed on the cabinet. This attribute replaces `pms.id` in the request message.
`pack_id` | - | string | - | Id of the `pack` associated with this data record. This attribute is used in place of `pack.id` as a top-level attribute, so that it can be included in the data clustering specification.
`pack.volts` | volts | float | - | The cellblocks are connected in series so the pack voltage is the sum of all 14 cell voltages (`cell.volts[1-14]`).
`pack.amps` | amps | float | - | _(no change from request message)_.
`pack.watts` | watts | float | - | The product of `pack.volts` and `pack.amps`.
`pack.vcl` | volts | float | - | The lowest cell voltage in `cell.volts`.
`pack.vch` | volts | float | - | The highest cell voltage in `cell.volts`.
`pack.dock` | - | integer | 1-48 | _(no change from request message)_.
`pack.temp_top` | degC | float | - | The 1st element in `pack.temp`.
`pack.temp_mid` | degC | float | - | The 2nd element in `pack.temp`.
`pack.temp_bottom` | degC | float | - | The 3rd element in `pack.temp`.
`cell[nn].volts` | volts | float | - | The value corresponding to nn in the request `cell.volts` array.
`cell[nn].dvcl` | millivolts | float *(array)* | - | The millivolts difference (delta) between the lowest cell voltage in the pack (`pack.vcl`) and the cell voltage (`cell[nn].volts`).
`cell[nn].open` | _open/closed_ | integer | _true/false_ | 1 if any element in the request `cell.open` array contained the value nn, otherwise 0.
`fet[nn].open` | _open/closed_ | integer | _true/false_ | The first element in `fet[nn]` corresponds to the input Fet (Fet In) and the second element corrsponds to the output fet (Fet Out). The value of `fet[1].open` is _true_ if any element in the request `fet.open` field contains the value 1, otherwise it will be _false_. The same applies to `fet[2].open` which is _true_ or _false_ depending on whether the request `fet.open` field contains the value 2.
`fet[nn].temp` | degC | float | - | `fet[nn].temp`will be _true_ if the request `fet.temp` field contains the value `nn`.
`status.temp` | degC | float | - | The Ehub's self-monitored temperature.
`status.bus_connect` | - | boolean | - | One of the status fields described in __Equipment status__ section below.
`sender` | - | string | - | The identifier of the data sender, based on the API key sent in the request header. The value is a foreign key to the `system.source` dataset table, which provides traceability, and data provenance for data received through the API endpoint.
`time_event` | - | datetime | - | The UTC time of the event which produced this data sample.
`time_zone` | - | string | _+/-HH:MM_ | The time offset in the time zone of the event which produced this data sample.
`time_processing` | - | datetime | - | The UTC time when the request was received and *processed* on the API host.


### Equipment status attributes 

The dataset includes the following status attributes which are based on the hex-encoded code in the request's `status.code`.

Attribute | Data | Constraint | Description
--- | --- | --- | ---
`bus_connect` | boolean | _true/false_ | The device's `Bus Connectivvity` status. Indicated whether the device's data bus is connected (_true_) or faulty (_false_). Corresponds to bit __0__ in the binary-decoded request `status.code`.


### Partitions and clustering

__pms__ dataset tables are partitioned based on `time_event` and clustered by `pms_id` and `pack_id`.


---

# Source system

A PMS system consists of 1-4 Cabinets with 1 Busbar in each Cabinet.

Each Busbar docks 4-12 **Cases**.

- Case and Pack data are related through a shared **id**.
- Pack data includes the Busbar **dock** number.

A Case contains 14 **Cell Blocks**, 1 **Fet Board**, and 1 **Acqu. Board**. 

PMS data consists of two distinct datasets, _Monitoring_ data and _Transaction/Master_ data, which are joined through a relationship as shown in the model below:

![PMS System](/images/PMSComposite.png)

### Monitoring data

PMS monitoring data streams in as a time-series unbounded dataset. The data is schedule-driven (e.g. every 5 seconds) and is append-only.

Monitoring data is collected from:
- 4-48 **Packs**
- 14 **Cells** per Pack
- 2 **Fets** per Pack

The dataset is collected through a recurring schedule and appended to a time-series log. 

The Monitoring Dataset links to Master data through a shared key in **PMS id** and **Case Id** respectively. 

![PMS Data](/images/PMSData.png)

### Transaction/Master data 

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