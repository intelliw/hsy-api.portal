# Introduction
---

The Energy Management API provides energy accounting data and event notifications, for users and vendors to monitor and control their Energy Assets and Energy Management Devices.

Energy | Assets | Devices
--- | --- | ---
`harvest` | Renewables | PV Modules, Maximum Power Point Trackers (MPPT)
`store` | Storage | Busbar Controllers (BBC), Pack Management Systems (PMS)
`enjoy` | Appliances | Multicore-Cable Current Sensors, Switchboard Clamp Sensors
`grid` | Mains Electricity | Smart Meters, PV Grid-interactive Inverters

The API is based on REST / Hypermedia and specified in [OpenAPI v2.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md). 

The API and documentation is available through the *Sundaya Developer Portal* at [https://endpointsportal.sundaya.cloud.goog](https://endpointsportal.sundaya.cloud.goog). 

### Energy API
The `/energy` path provides data for four **Energy Types** :`harvest`, `store`, `enjoy`, `grid`. 

The API response consolidates data for all four energy types, in the requested period (week, month etc.).

Clients can use the API to manage **Energy Assets** through graphical views and by scheduling energy use at preferred times (through the `/devices` path).

### Devices API

The `/devices` path is for vendor integrations to log energy flows, and for vendors to monitor and control their **Energy Management Devices**.

