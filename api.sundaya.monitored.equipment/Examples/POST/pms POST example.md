# pms POST
---

### Request message structure

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
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
```

### Message attributes 

The pms request message attributes are specified below. 

Attribute | Metric | Data | Constraint | Description
--- | --- | --- | --- | --- 
`pms.id` | - | string | - | The `pms.id` is configured for each site during pre-shipment. It is stored on each Ehub (or BBC); upto 4 EHubs (i.e. Cabinets) can share the same `pms.id`. The id is the foreign key for relationally joining the pms dataset to reference data in the cloud. As a `pack` self identifies, it can be hot-swapped on site without any need for re-configuring metadata to associate the pack with the pms system.
`time_local` | - | datetime | RFC 3339 | The local time of the event which produced this data sample, represented with a mandatory `+/-` offset from UTC for the device's location, in compressed `ISO 8601/RFC3339` (YYYYMMDDThhmmss±hhmm).
`pack.id` | - | string | - | The pack identifier. This is the Case id which is laser engraved in human-readable form on the outside of the case. Initially (temporarily) the `pack.id` will be populated with the pack’s acquisition board hardware id, and converted to the Case id through a lookup on the server. In future the Case id will be directly stored in the acquisition board during pre-shipment configuration and no further data transformation will be required for determining the Pack id during data collection.
`pack.dock` | - | integer | 1-48 | The dock number (1 - 48) into which this pack is installed. The dock number covers 4 cabinets with 12 docks per cabinet in sequence. For example dock number 13 indicates that the pack is installed in dock 1 of cabinet B (the second cabinet).
`pack.amps` | amps | float [+/-] | - | The current draw for this pack. The value is positive for charge current and negative when discharging. 
`pack.temp` | degC | float *(array)* | *array size 3* | An ordered set of 3 temperature readings in degC, for the temperature of the cell-pack at its top, middle, and bottom. The array position of  each value is significant: the 1st value applies to the top of the pack, 2nd to the middle, and 3rd to the bottom. 
`cell.volts` | volts | float *(array)* | *array size 14* | An ordered set of 14 Voltage readings for 14 cellblocks in this pack. Each value in the data array applies to a cell number based on its position in the array. For example the 2nd value in the data array is the data for the 2nd cell in the pack. The pack voltage is not included in the monitoring dataset: as cellblocks are connected in series the pack voltage is calculated later as the sum of all 14 cell voltages.
`cell.open` | *open/closed* | integer *(array)* | 1-14, *uniqueitems, array size 0-14* | An unordered set of unique ordinal numbers for cells  which are ‘open’ due to cell balancing. Must contain numbers from 1-14 as there are only 14 cells in a pack. For example a data value of [1,6] signifies that cell 1 and 6 are open (bypassed) to allow the other cells in the pack to charge and reach parity with this cell block. Typically the array will be empty to indicate that there is no cell balancing in effect.
`fet.temp` | degC | float | *array size 2* | An ordered set of temperature readings for the two MOSFETS in this pack. The first is the input (CMOS) and the 2nd is the output (DMOS) MOSFET.
`fet.open` | *open/closed* | integer *(array)* | 1-2, *uniqueitems, array size 2* | An unordered set of unique ordinal numbers for FETs which are ‘open’ due to pack balancing. The data values must be a number from 1-2 as there are only 2 FETS in each pack. For example a data value of [1,2] signifies that FETS 1 and 2 are both open.
`status` | - | string | *maxLength 4, minLength 4* | A 4-character, hex-encoded string value corresponding to a bitmap of status fields; described in the __Equipment status__ section below.
 
- Attributes marked as '*required on change*' (if any) may be omitted if the value has not changed since the last successful post (a POST is successful if the API server responds with a 200 level reply).
All other attributes are mandatory and must be present.


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
1-15  | _unused_                      | -                 | 0     | -


### text/csv Data

Requests with _content-type_ of `text/csv` must provide UTF-8 `csv` data in the request body and include a header row.

The data must include a column for each attribute listed in the __Message Attributes__ table above, including a separate column for each array item: 

- Column names for array items should be the the attribute name followed by the array item number.

- For example `cell.volts.1`, `cell.volts.2` etc. represent 14 elements in the `cell.volts` array attribute.

A valid __csv__ data sample including a header row and a data row is shown below. 

```csv
pms.id,time_local,pack.dock,pack.id,pack.dock,pack.amps,pack.temp.1,pack.temp.2,pack.temp.3,cell.volts.1,cell.volts.2,cell.volts.3,cell.volts.4,cell.volts.5,cell.volts.6,cell.volts.7,cell.volts.8,cell.volts.9,cell.volts.10,cell.volts.11,cell.volts.12,cell.volts.13,cell.volts.14,cell.open.1,cell.open.2,cell.open.3,cell.open.4,cell.open.5,cell.open.6,cell.open.7,cell.open.8,cell.open.9,cell.open.10,cell.open.11,cell.open.12,cell.open.13,cell.open.14,fet.temp.1,fet.temp.2,fet.open.1,fet.open.2
025,20190820T115900.000+0700,6,269,6,-1.3,33,32,31,3.793,3.796,3.78,3.788,3.797,3.795,3.792,3.796,3.788,3.779,3.795,3.795,3.788,3.793,,,,,,,,,,,,,,,30,29,0,0
```


### POST /dataset/pms request

[/device/dataset/pms](https:/api.sundaya.monitored.equipment/devices/dataset/pms)

The request example below shows data collected for 2 PMS systems at a site ('PMS-01-001' and 'PMS-01-002') with 1 cabinet each, and 4 packs in the cabinet (the smallest PMS system configuration). 

*__Note__: PMS controllers (BBC or EHub) can handle expansion up to 4 cabinets x 12 packs in each cabinet (48 packs in total). Each pack contains 14 cells*.

The example shows two data samples for each PMS taken 10 seconds apart (PMS-00-001 at **15:00.06** and, PMS-00-002 at **15:00.07**).

Each object in the `data` array contains data for a single pack, identified by `pack.id`.

- `pack.dock` provides the dock number (1 - 48) into which this pack is installed.

- `cell.volts` contains an array of 14 voltage datapoints corresponding to each of the 14 cell blocks in a pack.

The size of a dataset for a single cabinet (4 packs) is 2030 bytes including whitespace, or 1420 bytes if whitespace is stripped (using JSON.stringify).


```
*** REQUEST ***	
POST /devices/dataset/pms HTTPS/1.1	
Host: api.sundaya.monitored.equipment
Accept: application/json
Content-Type: application/json
    
```

```json
{
  "datasets": [
    { "pms": { "id": "PMS-01-001" }, 
      "data": [
        { "time_local": "20190909T150006.032+0700",
          "pack": { "id": "0241", "dock": 1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.91] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150006.032+0700",
          "pack": { "id": "0242", "dock": 2, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150006.032+0700",
          "pack": { "id": "0243", "dock": 3, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150006.032+0700",
          "pack": { "id": "0244", "dock": 4, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        }
      ]
    },
    { "pms": { "id": "PMS-01-002" }, 
      "data": [
        { "time_local": "20190909T150007.020+0700",
          "pack": { "id": "0245", "dock": 1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150007.020+0700",
          "pack": { "id": "0246", "dock": 2, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150007.020+0700",
          "pack": { "id": "0247", "dock": 3, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150007.020+0700",
          "pack": { "id": "0248", "dock": 4, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        }
      ]
    },
    { "pms": { "id": "PMS-01-001" }, 
      "data": [
        { "time_local": "20190909T150016.032+0700",
          "pack": { "id": "0241", "dock": 1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150016.032+0700",
          "pack": { "id": "0242", "dock": 2, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150016.032+0700",
          "pack": { "id": "0243", "dock": 3, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150016.032+0700",
          "pack": { "id": "0244", "dock": 4, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        }
      ]
    },
    { "pms": { "id": "PMS-01-002" }, 
      "data": [
        { "time_local": "20190909T150017.020+0700",
          "pack": { "id": "0245", "dock": 1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150017.020+0700",
          "pack": { "id": "0246", "dock": 2, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150017.020+0700",
          "pack": { "id": "0247", "dock": 3, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        },
        { "time_local": "20190909T150017.020+0700",
          "pack": { "id": "0248", "dock": 4, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2] },
            "status": "0001" }
        }
      ]
    }
  ]
}
```

### POST /dataset/pms response

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
      "target": "dataset:pms"
    }
  ]
}
```

If the request is processed synchronously a 201 Response will be returned as follows. 


```
*** RESPONSE ***	
201 Created HTTPS/1.1	
Location: https:/api.sundaya.monitored.equipment/device/CAB-01-001/dataset/pms/period/minute/20190209T1500+0700/1
Location: https:/api.sundaya.monitored.equipment/device/CAB-01-002/dataset/pms/period/minute/20190209T1500+0700/1
Content-Type: application/json
Content-Length: 8063	

```

```json
{
  "status": "201",
  "message": "Created",
  "details": [
    {
      "message": "New data events created.",
      "target": "device:PMS-01-001 | dataset:pms | events:8"
    },
    {
      "message": "New data events created.",
      "target": "device:PMS-01-002 | dataset:pms | events:8"
    }
  ]
}
```

