# Platform Overview
---

The platform consists of the following core components:

### Edge Data Collection 

1. **Devices** - _PMS_, _MPPT_, and _Inverters_ which continuously produce monitoring data.

2. **Device Controllers** - such as _BBC_ and _EHub Gateways_, which collect and upload data through an API.

![Platform Devices](/images/platform-devices.jpg)

### Data Management

3. **Trilateral API** - synchronous _Command/Query_, and asynchronous _Publish_ and _Subscribe_ API interfaces.

4. **Data Pipeline** - _Message Broker_ cluster, and real-time ingest and query _Analytics Engine_.

5. **Data Storage** - _Data Warehouse_ for SQL analytics (technical users), deep storage, view materialisation, and data retention management.

6. **Data Visualisation** - _Analytics Engine_, OLAP data store, and _BI/OI Dashboard_ for business intelligence reports and ad-hoc queries by business users.

- `BI` (_Business Intelligence_) artefacts include reports and visualisations for analyzing trends over time.

- `OI` (_Operational Intelligence_) artefacts include real-time dashboards and alerts for exceptions to provide a holistic picture of what is currently happening across the system landscape.

![Real-Time Analytics Backend](/images/platform-backend.jpg)

---
