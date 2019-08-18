# Getting Started
---

The API is based on REST / Hypermedia and specified in [OpenAPI v2.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md). 

API documentation is available through the *Sundaya Developer Portal* at [https://endpointsportal.sundaya.cloud.goog](https://endpointsportal.sundaya.cloud.goog).


### Devices API

The `/devices` path is for receiving monitoring data from trackable devices such as _[pms](https://endpointsportal.sundaya.cloud.goog/docs/api.endpoints.sundaya.cloud.goog/0/c/Implementation/Device%20Datasets/pms%20Dataset)_, _[mppt](https://endpointsportal.sundaya.cloud.goog/docs/api.endpoints.sundaya.cloud.goog/0/c/Implementation/Device%20Datasets/mppt%20Dataset)_, and _[inverters](https://endpointsportal.sundaya.cloud.goog/docs/api.endpoints.sundaya.cloud.goog/0/c/Implementation/Device%20Datasets/inverter%20Dataset)_.

These devices continuously produce monitoring data which is then collected and uploaded by device controllers (e.g. Bus Bar Controller) and gateways (e.g. EHub Gateway) through the `/devices` API path.

The collected data is then aggregated into time-windows to provide energy accounting data and event notifications, for users and vendors to monitor and control their energy and devices.

### Energy API

The `/energy` path provides time-windowed data for four **Energy Types** :`harvest`, `store`, `enjoy`, `grid`. 

The API consolidates data for all four energy types in the requested period (week, month etc.).

API consumers use this data to manage and optimise their energy assets, through graphical views and by scheduling energy use at preferred times (through the `/devices` path).

The following table summarises the four **Energy Types** and examples of their data sources (devices). 

Energy | Assets | Devices
--- | --- | ---
`harvest` | Renewables | PV Modules, Maximum Power Point Trackers (MPPT)
`store` | Storage | Busbar Controllers (BBC), Pack Management Systems (PMS)
`enjoy` | Appliances | Multicore-Cable Current Sensors, Switchboard Clamp Sensors
`grid` | Mains Electricity | Smart Meters, PV Grid-interactive Inverters


### Energy Flows

To restate the Law of Conservation of Energy in API terms, Energy flows are based on a "double entry" format where each flow has an equal and opposite flow. 

The following 6 data element names are used to refer to these energy flows in API paths and responses. 

Each data element refers to a subset of the overall energy dataset. For any given time window the net flow from these elements will be zero.

- `store.in` and `store.out` indicate *charge* and *discharge* flows for batteries.

- `grid.in` and `grid.out` indicate mains use, and feed-in/out flows from the public grid.

- `harvest` refers to renewable energy generation. 

- `enjoy` indicates energy use by end users and appliances. 

### Visualisation

The API helps to further visualise data flows in graphical views, as a flow of energy from 'sources' at the top, to 'sinks' at the bottom.  

Sources | Sinks    
--- |---
`harvest`, `store.out`, `grid.out` |`enjoy`, `store.in`, `grid.in`

Graphs are typically shown in a ‘double entry’ format (the up and down bars are the same size), as shown in the following sample. 

![Stacked bar graph format](/images/graph.stacked-bar-example.png)

The example shows the following behaviour:
- In the 1st hour all `enjoy` energy came from the battery (`store.out`). 
- In the 2nd hour half came from battery and the other half from grid (`grid.out`). 
- In the 3rd hour all came from the grid.
- In the 4th hour the sun starts delivering (`harvest`)
- In the 10th hour harvest data is more than enjoy and the energy flows into store (`store.in`)

A graph with lot of _Black_ in the top tier indicates that you need to do something about it. In general it indicates a need for more battery capacity and/or `harvest` generation capacity. 

 
    
Note: To query specific assets, API consumers can also filter requests by `category`, `subcategory`, and `productType` (in the request *Body*).

### Colour codes

The API’s response data can be visualised through an application as a stacked bar graph, based on these standard colours for each dataset in the response 'data' element.

![Colour codes & energy sources](/images/energy.colour-codes.png)
 



### Double-entry format 

The following monthly `period` graph shows a representation of all data elements returned by the energy API.

![Monthly usage example](/images/graph.monthly-usage.png)

**Top tier**

- _Green_ represents renewable energy generation (`harvest`) and is always shown in the top tier.
- _Black_ in the top tier shows energy drawn from the grid (`grid.out`).
- _Blue_ in the top tier shows energy *discharge* from batteries (`store.out`).

**Bottom tier**

- _Red_ represents energy consumption (`enjoy`) and is always shown in the bottom tier.
- _Black_ in the bottom tier shows a net excess (of `harvest` energy, compared to `store.in` and `enjoy` energy volumes), resulting in feed-in flows to the grid (`grid.in`).
- _Blue_ in the bottom tier shows battery *charge* (`store.in`) again due to a net excess of `harvest` energy.
