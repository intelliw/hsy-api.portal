# Energy Management
---

The Energy Management API provides data for energy accounting and allows users and vendors to monitor and control their Energy Assets and Energy Management Devices. 

Energy Type | Energy Assets | Energy Management Devices
--- | --- | ---
`harvest` | Renewables | Maximum Power Point Tracker (MPPT), PV Modules, Inverter
`store` | Storage | Busbar Controler (BBC), Pack Management System (PMS)
`enjoy` | Appliances | Multicore-Cable Current Sensors, Clamp-on Current Sensors
`grid` | Mains Electricity | Smart Meters

The API is based on REST / Hypermedia and specified in [OpenAPI v2.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md). 

The API and documentation is available through the *Sundaya Developer Portal* at [https://developer.sundaya.com](https://developer.sundaya.com). 

### Energy API
The `/energy` path provides metrics for four **Energy Types** :`harvest`, `store`, `enjoy`, `grid`. 

The API response consolidates data for all four energy types, for a requested period (week, month etc.).

Clients can use the API to manage **Energy Assets** through graphical views and by scheduling energy use at preferred times.

### Devices API

The `/devices` path is for vendor integrations to log energy flows, and for vendors to monitor and control their **Energy Management Devices**.


# Data Structure

### Element names

The following labels are used in API paths and responses to refer to energy data subsets. 

- `store.in` and `store.out` indicate *charge* and *discharge* flows for batteries.

- `grid.out` and `grid.in` indicate mains use, and feed-in flows to the public grid.

- `harvest` indicates renewable generation and always implies `.out`. 

- `enjoy` indicates energy use and always implies `.in`. 

### Double-entry format 

Energy flows are based on a "double entry" format (each flow has an opposite flow). 

These equal and opposite flows are summarised in the following table: 

Flow | From / To   
--- |---
`harvest` |`store.in` `enjoy` `grid.in`
`store.out` | `enjoy`
`enjoy`  |  `harvest` `store.out` `grid.out`
    
To query specific assets, clients can filter requests by `category`, `subcategory`, and `product-type` (in the request *Body*).


