# mppt POST
---

### Request message structure

The following snippet shows the structure of a `mppt` request:

```json
{
  "datasets": [
    { "mppt": { "id": "IT6415AD-01-001" }, 
      "data": [
        { "time_local": "20190209T150006.032+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1, "amps": 0.0 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] },
          "status": "1A79"
        },
```

### Message attributes 

The mppt request message attributes are specified below. 

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | --- 
`mppt.id` | - | string | - | Id of the MPPT charge controller. *(__Note__: a site can have any number of MPPT controllers, and each controller can have multiple PV strings and Loads)*.
`time_local` | - | datetime | RFC 3339 | The local time of the event which produced this data sample, represented with a mandatory `+/-` offset from UTC for the device's location, in compressed `ISO 8601/RFC3339` (YYYYMMDDThhmmss±hhmm).
`pv.volts` | volts | float *(array)* | *array size 1-4* | An ordered set of Voltage readings for PV strings connected to this MPPT. Each value in the data array applies to a numbered PV string based on its position in the array. For example the 2nd value in the data array is the data for the 2nd PV string. The array size depends on the number of PV strings. Presently upto 4 PV strings per controller are supported. In future each string will have its own dedicated controller.
`pv.amps` | amps | float *(array)* | *array size 1-4* | An ordered set of Current readings for PV strings (corresponding to voltage readings in `pv.volts`).  
`battery.volts` | volts | float | - | The voltage of the connected Battery.
`battery.amps` | amps | float [+/-] | - | The current for the connected Battery. The value is positive for charge current and negative when discharging.
`load.volts` | volts | float *(array)* | *array size 1-2* | An ordered set of Voltage readings for connected Load. Each value in the data array applies to a Load number based on its position in the array. For example the 2nd value in the data array is the data for the 2nd Load. The array size depends on the number of loads. Each load and its ordinal position must be declared in this API documentation. In a BTS installation Load 1 is the VSAT system and Load 2 is the BTS.
`load.amps` | amps | float *(array)* | *array size 1-2* | An ordered set of Current readings for connected Loads  (corresponding to values in `load.volts`).
`status` | - | string | *maxLength 4, minLength 4* | A 4-character, hex-encoded string value corresponding to a bitmap of status fields; described in the __Equipment status__ section below.


### Equipment Status

The `status` attribute in the request message should contain a hex-encoding of the following bitmaped status fields. 

- bit '0' corresponds to the least significant bit (the right-most bit). 

- bits 14-15 are currently not used in this scheme but must be padded with zeros to produce a 4-character hex value.

As an example, a `status` of '__1A79__' is equivalent to __0001 1010 0111 1001__ which indicates :
-   the data bus is connected and _ok_ (`Bit 0`=1).
-   the _mppt_ system is on _standby_ (`Bit 13`=0) with a charging status of _float_ (`Bit 10,11`=0,1).
-   pv, load, fets, and input statuses are all _ok_.


Bit # | Status                        | Field Name        | Value | ..
---   | ---                           | ---               | ---   | ---  
0     | Bus Connectivity              | `bus_connect`     | 1     | _ok_
..    |                               |                   | 0     | _fault_
1, 2  | Input Status                  | `input`           | 0, 0  | _normal_
..    |                               |                   | 0, 1  | _no-power_
..    |                               |                   | 1, 0  | _high-volt-input_
..    |                               |                   | 1, 1  | _input-volt-error_
3     | Charging Mosfet               | `chgfet`          | 0     | _ok_  
..    |                               |                   | 1     | _short_
4     | Charging Anti Reverse Mosfet  | `chgfet_antirev`  | 0     | _ok_  
..    |                               |                   | 1     | _short_
5     | Anti Reverse Mosfet           | `fet_antirev`     | 0     | _ok_  
..    |                               |                   | 1     | _short_
6     | Input Current                 | `input_current`   | 0     | _ok_
..    |                               |                   | 1     | _overcurrent_
7,8   | Load                          | `load`            | 0, 0  | _ok_
..    |                               |                   | 0, 1  | _overcurrent_
..    |                               |                   | 1, 0  | _short_
..    |                               |                   | 1, 1  | _not-applicable_
9     | PV Input                      | `pv_input`        | 0     | _ok_
..    |                               |                   | 1     | _short_
10,11 | Charging Status               | `charging`        | 0, 0  | _not-charging_
..    |                               |                   | 0, 1  | _float_
..    |                               |                   | 1, 0  | _boost_
..    |                               |                   | 1, 1  | _equalisation_
12    | System Status                 | `system`          | 0     | _ok_
..    |                               |                   | 1     | _fault_
13    | Standby Status                | `standby`         | 0     | _standby_
..    |                               |                   | 1     | _running_
14-15 | _unused_                      | -                 | 0     | -



### POST /dataset/mppt request

[/devices/dataset/mppt](https:/api.sundaya.monitored.equipment/devices/dataset/mppt)

The request example below shows datasets collected at a BTS site with 2 EPSolar MPPT Charge Controllers ('IT6415AD-01-001' and 'IT6415AD-01-002').

*Note: A site can have any number of MPPT controllers, and each controller can have multiple PV strings and Loads. Presently upto 4 PV strings per controller are supported. In future each string will have its own dedicated controller*.

The example shows two data samples for each MPPT Controller taken 10 seconds apart (IT6415AD-01-001 at **15:00.06** and IT6415AD-01-002 at **15:00.07**).

In this example each MPPT controller has :

- 2 PV strings as shown by the data arrays in `pv.volts` and `pv.amps`.

- 2 DC Loads as shown by `load.volts` / `load.amps`: Load 1 is the VSAT system and Load 2 is the BTS.  

The size of each MPPT dataset is 367 bytes including whitespace, or 248 bytes if whitespace is stripped (using JSON.stringify).


```
*** REQUEST ***	
POST /devices/dataset/mppt HTTPS/1.1	
Host: api.sundaya.monitored.equipment
Accept: application/json
Content-Type: application/json
    
```

```json
{
  "datasets": [
    { "mppt": { "id": "IT6415AD-01-001" }, 
      "data": [
        { "time_local": "20190909T150006.032+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1, "amps": 0.0 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] },
          "status": "1A79"
        },
        { "time_local": "20190909T150016.022+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1, "amps": 0.0 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] },
          "status": "1A79"
        }
      ]
    },
    { "mppt": { "id": "IT6415AD-01-002" }, 
      "data": [
        { "time_local": "20190909T150007.032+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1, "amps": 0.0 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] },
          "status": "1A79"
        },
        { "time_local": "20190909T150017.022+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1, "amps": 0.0 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] },
          "status": "1A79"
        }
      ]
    }
  ]
}
```

### POST /dataset/mppt response

A 200 Response will be sent if the request is accepted and queued for asynchronous processing. 

```
*** RESPONSE ***	
200 OK HTTPS/1.1	
Content-Type: application/json
Content-Length: 1171	

```

```json
{
    "status": "200",
    "message": "ok",
    "details": [
        {
            "message": "Data queued for processing.",
            "target": "dataset:mppt"
        }
    ]
}
```

If the request is processed synchronously a 201 Response will be returned as follows. 

```
*** RESPONSE ***	
201 Created HTTPS/1.1	
Location: https:/api.sundaya.monitored.equipment/device/IT6415AD-01-001/dataset/mppt/period/minute/20190209T1500+0700/1
Location: https:/api.sundaya.monitored.equipment/device/IT6415AD-01-002/dataset/mppt/period/minute/20190209T1500+0700/1
Content-Type: application/json
Content-Length: 1171	

```

```json
{
    "status": "201",
    "message": "Created",
    "details": [
        {
            "message": "New data logs created.",
            "target": "device:IT6415AD-01-001 | dataset:mppt | events:2"
        },
        {
            "message": "New data logs created.",
            "target": "device:IT6415AD-01-002 | dataset:mppt | events:2"
        }
    ]
}
```

