# pms POST example
---

### POST /dataset/pms request

[/device/dataset/pms](http:/api.endpoints.sundaya.cloud.goog/devices/dataset/pms)

The dataset example below shows data collected for 2 PMS systems at a site ('PMS-01-001' and 'PMS-01-002') with 1 cabinet each, and 4 packs in the cabinet (the smallest PMS system configuration). 

*__Note__: PMS controllers (BBC or EHub) can handle expansion up to 4 cabinets x 12 packs in each cabinet (48 packs in total). Each pack contains 14 cells*.

The example shows two data samples for each PMS taken 10 seconds apart (PMS-00-001 at **15:00.06** and, PMS-00-002 at **15:00.07**).

Each object in the `data` array contains data for a single pack, identified by `pack.id`.

- `pack.dock` provides the dock number (1 - 48) into which this pack is installed.

- `cell.volts` contains an array of 14 voltage datapoints corresponding to each of the 14 cell blocks in a pack.

The size of a dataset for a single cabinet (4 packs) is 2030 bytes including whitespace, or 1420 bytes if whitespace is stripped (using JSON.stringify).


```
*** REQUEST ***	
POST /devices/dataset/pms HTTP/1.1	
Host: api.endpoints.sundaya.cloud.goog
Accept: application/json
Content-Type: application/json
    
```

```json
{
  "datasets": [
    { "pms": { "id": "PMS-01-001" }, 
      "data": [
        { "time_local": "20190209T150006.032+0700",
          "pack": { "id": "0241", "dock": 1, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.91] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150006.032+0700",
          "pack": { "id": "0242", "dock": 2, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150006.032+0700",
          "pack": { "id": "0243", "dock": 3, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150006.032+0700",
          "pack": { "id": "0244", "dock": 4, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        }
      ]
    },
    { "pms": { "id": "PMS-01-002" }, 
      "data": [
        { "time_local": "20190209T150007.020+0700",
          "pack": { "id": "0245", "dock": 1, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150007.020+0700",
          "pack": { "id": "0246", "dock": 2, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150007.020+0700",
          "pack": { "id": "0247", "dock": 3, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150007.020+0700",
          "pack": { "id": "0248", "dock": 4, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        }
      ]
    },
    { "pms": { "id": "PMS-01-001" }, 
      "data": [
        { "time_local": "20190209T150016.032+0700",
          "pack": { "id": "0241", "dock": 1, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150016.032+0700",
          "pack": { "id": "0242", "dock": 2, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150016.032+0700",
          "pack": { "id": "0243", "dock": 3, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150016.032+0700",
          "pack": { "id": "0244", "dock": 4, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        }
      ]
    },
    { "pms": { "id": "PMS-01-002" }, 
      "data": [
        { "time_local": "20190209T150017.020+0700",
          "pack": { "id": "0245", "dock": 1, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150017.020+0700",
          "pack": { "id": "0246", "dock": 2, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150017.020+0700",
          "pack": { "id": "0247", "dock": 3, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
        },
        { "time_local": "20190209T150017.020+0700",
          "pack": { "id": "0248", "dock": 4, "volts": 55.1, "amps": -1.601, "temp": [35.0, 33.0, 34.0],
            "cell": { "open": [1, 6],
              "volts": [3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92, 3.92] },
            "fet": { "open": [1, 2], "temp": [34.1, 32.2, 33.5] } }
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
      "target": "dataset:pms"
    }
  ]
}
```

If the request is processed synchronously a 201 Response will be returned as follows. 


```
*** RESPONSE ***	
201 Created HTTP/1.1	
Location: http:/api.endpoints.sundaya.cloud.goog/device/CAB-01-001/dataset/pms/period/minute/20190209T1500+0700/1
Location: http:/api.endpoints.sundaya.cloud.goog/device/CAB-01-002/dataset/pms/period/minute/20190209T1500+0700/1
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

The following time attributes are prepended to each data item, based on the *event* time (`time_local`) sent in the request. The `time_local` attribute is overwritten and augmented by these attributes.

 - `time_processing_utc` - the time when the request was received and *processed* on the API host.
 - `time_utc` - the *event* time converted to `UTC` time.
 - `time_local` - the *event* time converted to the local time at the device site.

Initially `time_local` is left empty if the request `time` contained a `UTC` value. (see *Timezones* in [Standards & Conventions](https://endpointsportal.sundaya.cloud.goog/docs/api.endpoints.sundaya.cloud.goog/0/c/Standards%20&%20Conventions)). 

In this case `time_local` is populated later in the pipeline after looking up device location from the `site` reference dataset.

If the request `time` contained a `Local` value, both `time_local` and `time_utc` will be populated in the initial data item. 

The initial data item format sent to the message broker is shown in the followng example:

```
*** MESSAGE ***
Topic: pms
Key: PMS-01-001
Value:	
```

```json
{
   "time_processing_utc": "2019-08-12T18:28:08.8760+0000",
   "time_utc": "2019-02-09T16:00:17.0200+0000",
   "time_local": "2019-02-09T15:00:17.0200+0700",    
   "id": "PMS-01-002",
   "pack": { "id":"0248","dock":4,"volts":55.1,"amps":-1.601,"temp":[35.0,33.0,34.0],
      "cell": { "open":[1,6],"volts":[3.92,3.92,3.92,3.92,3.92,3.92,3.92,3.92,3.92,3.92,3.92,3.92,3.92,3.92] },
      "fet": { "open":[1,2],"temp":[34.1,32.2,33.5] }
},

```
