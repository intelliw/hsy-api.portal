# monitoring.mppt
---

# Source system

![MPPT Data](../../images/MPPTData.png)

--- 

# Dataset structure 

While the request message structure described above is optimised to reduce size, the dataset structure is optimised to simplify queries for analytics. 

In particular the arrays in the request message structure are flattened and transformed into the following dataset structure, which includes additional elements.

- The added timestamps are based on `time_local` sent in the request message, which is replaced by these timestamps.  

- The timestamps are stored in the canonical format used for data storage ('YYYY-MM-DD HH:mm:ss.SSSS') as shown in the examples.

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | ---
`mppt_id` | - | string | - | Id of the MPPT charge controller. This attribute replaces `mppt.id` in the request message.
`pv[nn].volts` | volts | float | - | The value corresponding to nn in the request `pv.volts` array.
`pv[nn].amps` | amps | float | - | The value corresponding to nn in the request `pv.amps` array.
`pv[nn].watts` | watts | float | - | The product of `pv.volts` and `pv.amps`.
`battery.volts` | volts | float | - | _(no change from request message)_.
`battery.amps` | amps | float | - | _(no change from request message)_.
`battery.watts` | watts | float | - | The product of `battery.volts` and `battery.amps`.
`load[nn].volts` | volts | float | - | The value corresponding to nn in the request `load.volts` array.
`load[nn].amps` | amps | float | - | The value corresponding to nn in the request `load.amps` array.
`load[nn].watts` | watts | float | - | The product of `load.volts` and `load.amps`.
`status` | - | status | - | A set of status fields in a complex type which is described in __Equipment status__ section below.
`sys.source` | - | string | - | The identifier of the data sender, based on the API key sent in the request header. The value is a foreign key to the `system.source` dataset table, which provides traceability, and data provenance for data received through the API endpoint.
`time_event` | - | datetime | - | The UTC time of the event which produced this data sample.
`time_zone` | - | string | - | The time offset in the time zone of the event which produced this data sample. The offset value is in '+/-HH:MM" format.
`time_processing` | - | datetime | - | The UTC time when the request was received and *processed* on the API host.


### Equipment status attributes 

The dataset includes the following status attributes which are based on the hex-encoded request `status`.

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | ---
`bus_connected` | _ok/fault_ | integer | 1/0 | The devices's `Bus Connectivvity` status. Indicated whether the device's data bus is connected or faulty. Corresponds to bit __0__ in the binary-decoded request `status`.
`bus_connect` | _ok/fault_ | integer | 1/0 | A boolean status indicating whether the device's data bus is connected or faulty. Corresponds to bit __0__ in the binary-decoded request `status`.
`input` | status | string | _normal_, _no-power_, _high-volt-input_, _input-volt-error_ | The device's `Input Status`. Corresponds to bits __1__ and __2__ in the binary-decoded request `status`.
`chgfet` | _ok/short_ | integer | 1/0 | The devices's `Charging Mosfet` status. Corresponds to bit __3__ in the binary-decoded request `status`.
`chgfet_antirev` | _ok/short_ | integer | 1/0 | The devices's `Charging Anti Reverse Mosfet` status. Corresponds to bit __4__ in the binary-decoded request `status`.
`fet_antirev` | _ok/short_ | integer | 1/0 | The devices's `Anti Reverse Mosfet` status. Corresponds to bit __5__ in the binary-decoded request `status`.
`input_current` | _ok/ overcurrent_ | integer | 1/0 | The devices's `Input Current` status. Corresponds to bit __6__ in the binary-decoded request `status`.
`load` | status | string | _ok_, _overcurrent_, _short_, _not-applicable_ | The device's `Load`. Corresponds to bits __7__ and __8__ in the binary-decoded request `status`.
`pv_input` | _ok/short_ | integer | 1/0 | The devices's `PV Input` status. Corresponds to bit __9__ in the binary-decoded request `status`.
`charging` | status | string | _not-charging_, _float_, _boost_, _equalisation_ | The device's `Charging Status`. Corresponds to bits __10__ and __11__ in the binary-decoded request `status`.
`system` | _ok/fault_ | integer | 1/0 | The devices's `System Status`. Corresponds to bit __12__ in the binary-decoded request `status`.
`standby` | _standby/ running_ | integer | 1/0 | The devices's `Standby Status`. Corresponds to bit __13__ in the binary-decoded request `status`.


### Transformed JSON message

The transformed JSON structure is shown in the followng sample. This structure is sent to the message broker at the first stage of processing the `dataset/mppt` POST message, and later used to load data into the datawarehouse:

```
*** MESSAGE ***
Topic: mppt
Key: IT6415AD-01-001
Value:	
```

```json
{
    "mppt_id": "IT6415AD-01-002",
    "pv": [
        {"volts": 48, "amps": 6, "watts": 288 },
        {"volts": 48, "amps": 6, "watts": 288 } ],
    "battery": {"volts": 55.1, "amps": 0.0, "watts": 0 },
    "load": [ 
        { "volts": 48, "amps": 1.2, "watts": 57.6 },
        { "volts": 48, "amps": 1.2, "watts": 57.6 } ],
    "status": { "bus_connect": 1, "input": "normal", 
      "chgfet": 1, "chgfet_antirev": 1, "fet_antirev": 1, 
      "input_current": 1, "load": "ok", "pv_input": 1, "charging": "not-charging", 
      "system": 1, "standby": 1 },
    "sys": {"source": "S000" },
    "time_event": "2019-02-09 08:00:07.0320",
    "time_zone": "+07:00",
    "time_processing": "2019-09-10 04:13:08.8780"
},
```

### Partitions and clustering

__mppt__ dataset tables are partitioned based on `time_event` and clustered by `mppt_id`.