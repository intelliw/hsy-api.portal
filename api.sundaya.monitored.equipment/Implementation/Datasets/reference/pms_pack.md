# reference.pms
---

This dataset provides reference data for pack management systems.

---

# Dataset Structure 

The following `reference.pms_pack` attributes are joined with `timeseries.pms` data to provide dashboard views and KPI metrics. 

Attribute       | Data      | Description
---             | ---       | ---
`pack.id`       | string    | The pack identifier. This is the Case id which is laser engraved in human-readable form on the outside of the case.
`board.id`      | string    | The pack’s acquisition board hardware id. Initially (temporarily) this `board.id` will be sent as the `pack.id`'in monitoring data, and the server will convert this to the actual pack.id through a lookup of this data table. In future the Case id will be directly stored in the acquisition board during pre-shipment configuration and no further data transformation will be required.
`fet.board.id`  | string    | -
`cell.id.1`     | string    | -
`cell.id.2`     | string    | -
`cell.id.3`     | string    | -
`cell.id.4`     | string    | -
`cell.id.5`     | string    | -
`cell.id.6`     | string    | -
`cell.id.7`     | string    | -
`cell.id.8`     | string    | -
`cell.id.9`     | string    | -
`cell.id.10`    | string    | -
`cell.id.11`    | string    | -
`cell.id.12`    | string    | -
`cell.id.13`    | string    | -
`cell.id.14`    | string    | -


```

