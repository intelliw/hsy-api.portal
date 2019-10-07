# inverter POST
---

### Request message structure

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
          "grid": { "volts": [48.000, 48.000, 48.000], "amps": [1.2, 1.2, 1.2], "pf": [0.92, 0.92, 0.92] },
          "status": "0001"
        },
```

### Message attributes

The inverter request message attributes are specified below. 

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | ---
`inverter.id` | - | string | - | Id of the Inverter charge controller. *(__Note__: a site can have any number of Inverter controllers, and each controller can have multiple PV strings and Loads)*.
`time_local` | - | datetime | RFC 3339 | The local time of the event which produced this data sample, represented with a mandatory `+/-` offset from UTC for the device's location, in compressed `ISO 8601/RFC3339` (YYYYMMDDThhmmss±hhmm).
`pv.volts` | volts | float *(array)* | *array size 1-4* | An ordered set of Voltage readings for PV strings connected to this Inverter. Each value in the data array applies to a numbered PV string based on its position in the array. For example the 2nd value in the data array is the data for the 2nd PV string. The array size depends on the number of PV strings. Presently upto 4 PV strings per controller are supported. In future each string will have its own dedicated controller.
`pv.amps` | amps | float *(array)* | *array size 1-4* | An ordered set of Current readings for PV strings (corresponding to voltage readings in `pv.volts`).  
`battery.volts` | volts | float | - | The voltage of the connected Battery.
`battery.amps` | amps | float [+/-] | - | The current for the connected Battery. The value is positive for charge current and negative when discharging.
`load.volts` | volts | float *(array)* | *array size 1-2* | An ordered set of Voltage readings for connected Loads. Each value in the data array applies to a Load number based on its position in the array. For example the 2nd value in the data array is the data for the 2nd Load. The array size depends on the number of loads. Each load and its ordinal position must be declared in this API documentation.
`load.amps` | amps | float *(array)* | *array size 1-2* | An ordered set of Current readings for connected Loads (corresponding to values in `load.volts`).  
`grid.volts` | volts | float *(array)* | *array size 1-3* | An ordered set of up to 3 Voltage readings for each phase of the connected grid supply. The array size depends on the number of phases in the supply. If the supply is single-phase there will be only one element in the array.
`grid.amps` | amps | float *(array)* | *array size 1-2* | An ordered set of Current readings for for each phase of the connected grid supply (corresponding to values in `grid.volts` and `grid.pf`).  
`grid.pf` | - | float *(array)* | *array size 1-2, maximum 1.0* | An ordered set of Power Factor readings for each phase of the connected grid supply (corresponding to values in `grid.volts` and `grid.amps`). 
`status` | - | string | *maxLength 4, minLength 4* | A 4-character, hex-encoded string value corresponding to a bitmap of status fields; described in the __Equipment status__ section below.


### Equipment status
The `status` attribute in the request message should contain a hex-encoding of the following bitmaped status fields. 

- bit '0' corresponds to the least significant bit (the right-most bit). 

- bits 1-15 are currently not used in this scheme but must be padded with zeros to produce a 4-character hex value.

As an example, a `status` of '__0001__' is equivalent to __0000 0000 0000 0001__ which indicates :
-   that the data bus is connected (_ok_)


Bit   | Status                        | Field Name        | Value | ..
---   | ---                           | ---               | ---   | ---  
0     | Bus Connectivity              | `bus_connect`     | 1     | _ok_
..    |                               |                   | 0     | _fault_
1-15  | _unused_                      | -                 | -     | -



### POST /dataset/inverter request

[/devices/dataset/inverter](https:/api.sundaya.monitored.equipment/devices/dataset/mppt)

The request example below shows datasets collected at a BTS site with 2 Kehua SPI-B2 Inverter / Charge Controllers ('SPI-B2-01-001' and 'SPI-B2-01-002').

*Note: A site can have any number of Inverter-controllers, and each controller can have multiple PV strings and Loads. Presently upto 4 PV strings per controller are supported. In future each string will have its own dedicated controller*.

The example shows two data samples for each Inverter Controller taken 10 seconds apart (SPI-B2-01-001 at **15:00.06** and SPI-B2-01-002 at **15:00.07**).

In this example each Inverter controller has :

- 2 PV strings as shown by the data arrays in `pv.volts` and `pv.amps`.

- 2 AC Loads as shown by `load.volts` / `load.amps`.  

The size of each Inverter dataset is 367 bytes including whitespace, or 248 bytes if whitespace is stripped (using JSON.stringify).


```
*** REQUEST ***	
POST /devices/dataset/inverter HTTP/1.1	
Host: api.sundaya.monitored.equipment
Accept: application/json
Content-Type: application/json
    
```

```json
{
  "datasets": [
    { "inverter": { "id": "SPI-B2-01-001" }, 
      "data": [
        { "time_local": "20190909T150006.032+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1, "amps": 0.0 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] }, 
          "grid": { "volts": [48.000, 48.000, 48.000], "amps": [1.2, 1.2, 1.2], "pf": [0.92, 0.92, 0.92] },
          "status": "0001"
        },
        { "time_local": "20190909T150016.022+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1, "amps": 0.0 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] },
          "grid": { "volts": [48.000, 48.000, 48.000], "amps": [1.2, 1.2, 1.2], "pf": [0.92, 0.92, 0.92] },
          "status": "0001"
        }
      ]
    },
    { "inverter": { "id": "SPI-B2-01-002" }, 
      "data": [
        { "time_local": "20190909T150007.032+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1, "amps": 0.0 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] },
          "grid": { "volts": [48.000, 48.000, 48.000], "amps": [1.2, 1.2, 1.2], "pf": [0.92, 0.92, 0.92] },
          "status": "0001"
        },
        { "time_local": "20190909T150017.022+0700",
          "pv": { "volts": [48.000, 48.000], "amps": [6.0, 6.0] },
          "battery": { "volts" : 55.1, "amps": 0.0 }, 
          "load": { "volts": [48.000, 48.000], "amps": [1.2, 1.2] },
          "grid": { "volts": [48.000, 48.000, 48.000], "amps": [1.2, 1.2, 1.2], "pf": [0.92, 0.92, 0.92] },
          "status": "0001"
        }
      ]
    }
  ]
}
```

### POST /dataset/inverter response

A 200 Response will be sent if the request is accepted and queued for asynchronous processing. 

```
*** RESPONSE ***	
200 OK HTTP/1.1	
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
            "target": "dataset:inverter"
        }
    ]
}
```

If the request is processed synchronously a 201 Response will be returned as follows. 

```
*** RESPONSE ***	
201 Created HTTP/1.1	
Location: https:/api.sundaya.monitored.equipment/device/SPI-B2-01-001/dataset/inverter/period/minute/20190209T1500+0700/1
Location: https:/api.sundaya.monitored.equipment/device/SPI-B2-01-002/dataset/inverter/period/minute/20190209T1500+0700/1
Content-Type: application/json
Content-Length: 1171	

```

```json
{
    "status": "201",
    "message": "Created",
    "details": [
        {
            "message": "New data logs created",
            "target": "device:SPI-B2-01-001 | dataset:inverter | events:2"
        },
        {
            "message": "New data logs created.",
            "target": "device:SPI-B2-01-002 | dataset:inverter | events:2"
        }
    ]
}
```

