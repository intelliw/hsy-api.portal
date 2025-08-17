# MPPT Telemetry (tel_mppt)
---

### API Consumer message ('monitoring.mppt')

Each dataset item in the the `dataset/mppt` POST message body 'datasets' array, is transformed into a separate JSON message as shown below. 

The consumer process transforms messsages into JSON structure shown in the following sample.

This consumer sends this message to the `analytics.tel_mppt` dataset table in the datawarehouse as an audit log, and to the `monitoring.mppt.dataset` message broker topic for further stream processing.

```
*** MESSAGE ***
topic: monitoring.mppt
key: IT6415AD-01-001
value:	
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
    "status": { "bus_connect": true, "input": "normal", 
      "chgfet": "ok", "chgfet_antirev": "ok", "fet_antirev": "ok", 
      "input_current": "ok", "load": "ok", "pv_input": "ok", "charging": "float", 
      "system": "ok", "standby": "standby" },
    "sender": "S000",
    "time_event": "2020-02-08 08:00:07.0320",
    "time_zone": "+07:00",
    "time_processing": "2019-09-10 04:13:08.8780"
},
```

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
`sender` | - | string | - | The identifier of the data sender, based on the API key sent in the request header.<br>The value is a foreign key to the `system.source` dataset table.
`time_event` | - | datetime | - | The UTC time of the event which produced this data sample.
`time_zone` | - | string | _+/-HH:MM_ | The time offset in the time zone of the event which produced this data sample.
`time_processing` | - | datetime | - | The UTC time when the request was received and *processed* on the API host.


### Equipment status attributes 

The dataset includes the following status attributes which are based on the hex-encoded request `status`.

Attribute | Data | Constraint | Description
--- | --- | --- | ---
`bus_connect` | boolean | _true/false_ | A boolean status indicating whether the device's data bus is connected (_true_) or faulty (_false_). Corresponds to bit __0__ in the binary-decoded request `status`.
`input` | string | _normal_, _no-power_, _high-volt-input_, _input-volt-error_ | The device's `Input Status`. Corresponds to bits __1__ and __2__ in the binary-decoded request `status`.
`chgfet` | string | _ok/short_ | The device's `Charging Mosfet` status. Corresponds to bit __3__ in the binary-decoded request `status`.
`chgfet_antirev` | string | _ok/short_ | The device's `Charging Anti Reverse Mosfet` status. Corresponds to bit __4__ in the binary-decoded request `status`.
`fet_antirev` | string | _ok/short_ | The device's `Anti Reverse Mosfet` status. Corresponds to bit __5__ in the binary-decoded request `status`.
`input_current` | string | _ok/ overcurrent_ | The device's `Input Current` status. Corresponds to bit __6__ in the binary-decoded request `status`.
`load` | string | _ok_, _overcurrent_, _short_, _not-applicable_ | The device's `Load`. Corresponds to bits __7__ and __8__ in the binary-decoded request `status`.
`pv_input` | string | _ok/short_ | The device's `PV Input` status. Corresponds to bit __9__ in the binary-decoded request `status`.
`charging` | string | _not-charging_, _float_, _boost_, _equalisation_ | The device's `Charging Status`. Corresponds to bits __10__ and __11__ in the binary-decoded request `status`.
`system` | string | _ok/fault_ | The device's `System Status`. Corresponds to bit __12__ in the binary-decoded request `status`.
`standby` | string | _standby/ running_ | The device's `Standby Status`. Corresponds to bit __13__ in the binary-decoded request `status`.


### Partitions and clustering

__mppt__ dataset tables are partitioned based on `time_event` and clustered by `mppt_id`.

---

# Source system

![MPPT Data](../../images/MPPTData.png)

--- 

