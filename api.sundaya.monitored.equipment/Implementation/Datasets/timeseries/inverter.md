# timeseries.inverter
---

### API Consumer message ('timeseries.inverter')

Each dataset item in the the `dataset/inverter` POST message body 'datasets' array, is transformed into a separate JSON message as shown below. 

The consumer process transforms messages into JSON structure shown in the following sample.

This consumer sends this message to the `timeseries.inverter` dataset table in the datawarehouse as an audit log, and to the `timeseries.inverter.dataset` message broker topic for further stream processing.

```
*** MESSAGE ***
topic: timeseries.inverter
key: SPI-B2-01-002
value:	
```

```json
{
    "inverter_id": "SPI-B2-01-002",
    "pv": [
        {"volts": 48, "amps": 6, "watts": 288 },
        {"volts": 48, "amps": 6, "watts": 288 } ],
    "battery": {"volts": 55.1, "amps": 0.0, "watts": 0 },
    "load": [
        { "volts": 48, "amps": 1.2, "watts": 57.6 },
        { "volts": 48, "amps": 1.2, "watts": 57.6 } ],
    "grid": [
        {"volts": 48, "amps": 1.2, "pf": 0.92, "watts": 91.785 },
        {"volts": 48, "amps": 1.2, "pf": 0.92, "watts": 91.785 },
        {"volts": 48, "amps": 1.2, "pf": 0.92, "watts": 91.785 } ],
    "status": { "bus_connect": true },    
    "sender": "S000",
    "time_event":"2020-02-08 08:00:17.0220",
    "time_zone":"+07:00",
    "time_processing":"2019-09-10 04:11:09.2930"
},
```

---

# Dataset structure 

While the request message structure described above is optimised to reduce size, the dataset structure is optimised to simplify queries for analytics.

In particular arrays in the request message structure are flattened and transformed into the following dataset structure, which includes additional elements.

- The added timestamps are based on `time_local` sent in the request message, which is replaced by these timestamps.  

- The timestamps are stored in the canonical format used for data storage ('YYYY-MM-DD HH:mm:ss.SSSS') as shown in the examples.

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | ---
`inverter_id` | - | string | - | Id of the Inverter charge controller. This attribute replaces `inverter.id` in the request message.
`pv[nn].volts` | volts | float | - | The value corresponding to nn in the request `pv.volts` array.
`pv[nn].amps` | amps | float | - | The value corresponding to nn in the request `pv.amps` array.
`pv[nn].watts` | watts | float | - | The product of `pv.volts` and `pv.amps`.
`battery.volts` | volts | float | - | _(no change from request message)_.
`battery.amps` | amps | float | - | _(no change from request message)_.
`battery.watts` | watts | float | - | The product of `battery.volts` and `battery.amps`.
`load[nn].volts` | volts | float | - | The value corresponding to nn in the request `load.volts` array.
`load[nn].amps` | amps | float | - | The value corresponding to nn in the request `load.amps` array.
`load[nn].watts` | watts | float | - | The product of `load.volts` and `load.amps`.
`grid[nn].volts` | volts | float | - | The value corresponding to nn in the request `grid.volts` array. This value is assumed to be line-to-line volage for a 3-phase supply.
`grid[nn].amps` | amps | float | - | The value corresponding to nn in the request `grid.amps` array.
`grid[nn].pf` | amps | float | *maximum 1.0* | The value corresponding to nn in the request `grid.pf` array.
`grid[nn].watts` | watts | float | - | Calculated watts for each supply phase based on the formula: `grid.watts` = `grid.volts` * `grid.amps` * `grid.pf` * `√3`. If the supply is single-phase there will be only one element in the array and accordingly the formula used will be formula: `grid.watts` = `grid.volts` * `grid.amps` * `grid.pf`.
`status` | - | status | - | A set of status fields in a complex type which is described in __Equipment status__ section below.
`sender` | - | string | - | The identifier of the data sender, based on the API key sent in the request header. The value is a foreign key to the `system.source` dataset table, which provides traceability, and data provenance for data received through the API endpoint.
`time_event` | - | datetime | - | The UTC time of the event which produced this data sample.
`time_zone` | - | string | _+/-HH:MM_ | The time offset in the time zone of the event which produced this data sample.
`time_processing` | - | datetime | - | The UTC time when the request was received and *processed* on the API host.


### Equipment status attributes 

The dataset includes the following status attributes which are based on the hex-encoded request `status.code`.

Attribute | Data | Constraint | Description
--- | --- | --- | ---
`bus_connect` | boolean | _true/false_ | The device's `Bus Connectivvity` status. Indicated whether the device's data bus is connected (_true_) or faulty (_false_). Corresponds to bit __0__ in the binary-decoded request `status.code`.


### Partitions and clustering

__inverter__ dataset tables are partitioned based on `time_event` and clustered by `inverter_id`.

---

# Source system

![Inverter Data](/images/InverterData.png)

--- 
