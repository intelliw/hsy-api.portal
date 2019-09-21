# monitoring.inverter
---

# System Overview

![Inverter Data](../../images/InverterData.png)

# Request Message

The following snippet shows the structure of an `inverter` request:

```json
{
  "datasets": [
    { "inverter": { "id": "SPI-B2-01-001" }, 
      "data": [
        { "time_local": "20190209T150006.032+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1, "amps": 0.0 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] }, 
          "grid": { "volts": [48.000, 48.000, 48.000], "amps": [1.2, 1.2, 1.2], "pf": [0.92, 0.92, 0.92] }
        },
```

### Message Attributes

The inverter request message attributes are specified below. 

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | ---
`inverter.id` | - | string | - | Id of the Inverter charge controller. *(__Note__: a site can have any number of Inverter controllers, and each controller can have multiple PV strings and Loads)*.
`time_local` | - | datetime | RFC 3339 | The local time of the event which produced this data sample, represented with a mandatory `+/-` offset from UTC for the device's location, in compressed `ISO 8601/RFC3339` (YYYYMMDDThhmmss±hhmm).
`pv.volts` | volts | float *(array)* | *array size 1-4* | An ordered set of Voltage readings for PV strings connected to this Inverter. Each value in the data array applies to a numbered PV string based on its position in the array. For example the 2nd value in the data array is the data for the 2nd PV string. The array size depends on the number of PV strings. Presently upto 4 PV strings per controller are supported. In future each string will have its own dedicated controller.
`pv.amps` | amps | float *(array)* | *array size 1-4* | An ordered set of Current readings for PV strings (corresponding to voltage readings in `pv.volts`).  
`batt.volts` | volts | float | - | The voltage of the connected Battery.
`batt.amps` | amps | float [+/-] | - | The current for the connected Battery. The value is positive for charge current and negative when discharging.
`load.volts` | volts | float *(array)* | *array size 1-2* | An ordered set of Voltage readings for connected Loads. Each value in the data array applies to a Load number based on its position in the array. For example the 2nd value in the data array is the data for the 2nd Load. The array size depends on the number of loads. Each load and its ordinal position must be declared in this API documentation.
`load.amps` | amps | float *(array)* | *array size 1-2* | An ordered set of Current readings for connected Loads (corresponding to values in `load.volts`).  
`grid.volts` | volts | float *(array)* | *array size 1-3* | An ordered set of up to 3 Voltage readings for each phase of the connected grid supply. The array size depends on the number of phases in the supply. If the supply is single-phase there will be only one element in the array.
`grid.amps` | amps | float *(array)* | *array size 1-2* | An ordered set of Current readings for for each phase of the connected grid supply (corresponding to values in `grid.volts` and `grid.pf`).  
`grid.pf` | - | float *(array)* | *array size 1-2, maximum 1.0* | An ordered set of Power Factor readings for each phase of the connected grid supply (corresponding to values in `grid.volts` and `grid.amps`). 

--- 

# Dataset Structure 

The following fields are prepended to request message attributes at the first stage of processing the `dataset/inverter` POST message. 

The added timestamps are based on `time_local` sent in the request message, which is replaced by these timestamps.  

The dataset timestamps are stored in the canonical timestamp format used for data storage ('YYYY-MM-DD HH:mm:ss.SSSS') as shown in the examples.

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | ---
`inverter_id` | - | string | - | Id of the Inverter charge controller. This attribute replaces `inverter.id` in the request message.
`pv.watts` | - | float | - | The product of `pv.volts` and `pv.amps`.
`load.watts` | - | float | - | The product of `load.volts` and `load.amps`.
`grid.watts` | - | float | - | Calculated for each supply phase, based on the formula: `grid.watts` = `grid.volts` * `grid.amps` * `grid.pf` * `√3`.
`sys.source` | - | string | - | The identifier of the data sender, based on the API key sent in the request header. The value is a foreign key to the `system.source` dataset table, which provides traceability, and data provenance for data received through the API endpoint.
`time_utc` | - | datetime | - | The UTC time of the event which produced this data sample.
`time_local` | - | datetime | - | The local time of the event which produced this data sample. Note that the timezone offset is discarded.
`time_processing` | - | datetime | - | The UTC time when the request was received and *processed* on the API host.


The dataset structure sent to the message broker at the first stage of processing is shown in the followng example:

```
*** MESSAGE ***
Topic: inverter
Key: SPI-B2-01-001
Value:	
```

```json
{
    "inverter_id":"SPI-B2-01-002",
    "pv": {
        "volts": [48,48], "amps": [6,6],
        "watts": [288,288] },
    "battery": {
        "volts": 55.1, "amps": 0.0 },
    "load": {
        "volts": [48,48], "amps": [1.2,1.2],
        "watts": [57.6,57.6] },
    "grid": {
        "volts": [48,48,48], "amps": [1.2,1.2,1.2], "pf": [0.92,0.92,0.92],
        "watts": [91.785,91.785,91.785] },
    "sys": {
        "source": "S000" },
    "time_utc":"2019-02-09 08:00:17.0220",
    "time_local":"2019-02-09 15:00:17.0220",
    "time_processing":"2019-09-10 04:11:09.2930"
},
```

### Partitions and Clustering

__inverter__ dataset tables are partitioned based on `time_local` and clustered by `inverter_id`.
