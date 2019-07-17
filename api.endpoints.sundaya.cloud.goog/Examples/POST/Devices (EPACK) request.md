# Devices
---

### POST /devices/dataset/epack request and response

[/device/dataset/epack](http:/api.endpoints.sundaya.cloud.goog/devices/dataset/epack)

The dataset example below shows data collected for a site with 2 JouleStore Cabinets ('CAB-01-001' and 'CAB-01-002') and 4 packs in each cabinet. 

*Note: a PMS controller (BBC) is able to handle 4 cabinets, with upto 12 packs in each cabinet, and 14 cells per pack*.

The example shows two data samples for each cabinet taken 10 seconds apart (CAB-00-001 at **15:00.06** and, CAB-00-002 at **15:00.07**).

Each object in the `data` array contains data for a single pack, identified by `pack.id`. 

- `pack.slot` indicates the cabinet slot in which the pack is installed.

- `cell.volts` contains an array of 14 voltage datapoints corresponding to each cell block in the pack.

The size of a dataset for a single cabinet (4 packs) is 2030 bytes including whitespace, or 1420 bytes if whitespace is stripped (by using JSON.stringify).


```
*** REQUEST ***	
POST /devices/dataset/epack HTTP/1.1	
Host: api.endpoints.sundaya.cloud.goog
Accept: application/json
Content-Type: application/json
    
*** RESPONSE ***	
201 OK HTTP/1.1	
Location: http:/api.endpoints.sundaya.cloud.goog/device/CAB-01-001/dataset/epack/period/minute/20190209T1500-0700/1
Location: http:/api.endpoints.sundaya.cloud.goog/device/CAB-01-002/dataset/epack/period/minute/20190209T1500-0700/1
Content-Type: application/json
Content-Length: 8063	

```


```json
{
  "datasets": [
    { "cabinet": { "id": "CAB-01-001" }, 
      "data": [
        { "time": "20190209T150006.032-0700",
          "pack": { "id": "EPACK-01-001", "slot": "01", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150006.032-0700",
          "pack": { "id": "EPACK-01-002", "slot": "02", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150006.032-0700",
          "pack": { "id": "EPACK-01-003", "slot": "03", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150006.032-0700",
          "pack": { "id": "EPACK-01-004", "slot": "04", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        }
      ]
    },
    { "cabinet": { "id": "CAB-01-002" }, 
      "data": [
        { "time": "20190209T150007.020-0700",
          "pack": { "id": "EPACK-01-005", "slot": "01", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150007.020-0700",
          "pack": { "id": "EPACK-01-006", "slot": "02", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150007.020-0700",
          "pack": { "id": "EPACK-01-007", "slot": "03", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150007.020-0700",
          "pack": { "id": "EPACK-01-008", "slot": "04", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        }
      ]
    },
    { "cabinet": { "id": "CAB-01-001" }, 
      "data": [
        { "time": "20190209T150016.032-0700",
          "pack": { "id": "EPACK-01-001", "slot": "01", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150016.032-0700",
          "pack": { "id": "EPACK-01-002", "slot": "02", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150016.032-0700",
          "pack": { "id": "EPACK-01-003", "slot": "03", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150016.032-0700",
          "pack": { "id": "EPACK-01-004", "slot": "04", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        }
      ]
    },
    { "cabinet": { "id": "CAB-01-002" }, 
      "data": [
        { "time": "20190209T150017.020-0700",
          "pack": { "id": "EPACK-01-005", "slot": "01", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150017.020-0700",
          "pack": { "id": "EPACK-01-006", "slot": "02", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150017.020-0700",
          "pack": { "id": "EPACK-01-007", "slot": "03", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        },
        { "time": "20190209T150017.020-0700",
          "pack": { "id": "EPACK-01-008", "slot": "04", "volts": "55.1", "amps": "0.0", "soc": "94.3", 
            "temp": { "top": "35.0", "mid": "33.0", "bottom": "34.0" } },
          "cell": { 
            "volts": ["3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92", "3.92"] },
          "fet": { 
            "temp": { "in": "35.0", "out": "33.0" }, "status": { "in": "0", "out": "1" } }
        }
      ]
    }
  ]
}

```
