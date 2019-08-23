# mppt Dataset
---

![MPPT Data](../../images/MPPTData.png)

# Request Message

The following snippet shows the structure of a `mppt` request:

```json
{
  "datasets": [
    { "mppt": { "id": "IT6415AD-01-001" }, 
      "data": [
        { "time_local": "20190209T150006.032+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] }
        },
```

### Message Attributes 

The mppt dataset attributes are specified below. 

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

--- 

# Dataset Structure 

The data structure sent to the message broker at the first stage of processing, is shown in the followng example:

```
*** MESSAGE ***
Topic: mppt
Key: IT6415AD-01-001
Value:	
```

```json
{ 
    "time_processing_utc":"2019-02-09T09:31:05.0110+0000",
    "time_utc": "2019-02-09T09:30:00.0200+0000",
    "time_local": "2019-02-09T16:30:00.0200+0700",
    "id": "IT6415AD-01-001",
    "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0], "watts": 288.01 },
    "battery": { "volts" : 55.1 }, 
    "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2], "watts": 57.60 }
},
```