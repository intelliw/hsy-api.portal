# mppt POST
---

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
POST /devices/dataset/mppt HTTP/1.1	
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
            "target": "dataset:mppt"
        }
    ]
}
```

If the request is processed synchronously a 201 Response will be returned as follows. 

```
*** RESPONSE ***	
201 Created HTTP/1.1	
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

