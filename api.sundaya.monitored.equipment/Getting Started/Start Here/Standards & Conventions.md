# Standards & Conventions
---

## API specification
The API is based on REST / Hypermedia and specified in [OpenAPI v2.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md). 

API documentation is available through the *Sundaya Developer Portal* at [https://docs.sundaya.monitored.equipment](https://docs.sundaya.monitored.equipment).



## API Endpoint
The API endpoint host is [https://api.sundaya.monitored.equipment](https://api.sundaya.monitored.equipment). 

All requests to the API endpoint receive the latest version of the API.     

Client applications may request an older API version by specifying the version number in the `Accept` header.

    Accept: application/vnd.sundaya.v1.0+yaml



## Date-time Format
Date and time parameters must be expressed in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format and must conform to [RFC3359](https://tools.ietf.org/html/rfc3339) .

    https://api.sundaya.monitored.equipment/energy/{type}/{period}/{epoch}

e.g. [https:/api.sundaya.monitored.equipment/energy/hsy/period/week/20190210](https:/api.sundaya.monitored.equipment/energy/hsy/period/week/20190210)

The compressed version of ISO 8601 is required, without semi colons and with `T` as the time designator, as shown in examples below.

## Timezones
Timezones must be represented in API parameters as `UTC` time, or as `Local` time with UTC offset. 

The supported timezone formats are described below with examples: 

- __UTC__ time, represented with a trailing `Z` or trailing zero offset `+0000`

    format: `YYYYMMDDTHHmmss.SSSSZ` or `YYYYMMDDTHHmmss.SSSS+0000`

    example: *0930 UTC == 1630 WIB (Indonesian Western Standard Time)*
    
    [https://api.sundaya.monitored.equipment/energy/hsy/period/hour/20190209T0930Z](https://api.sundaya.monitored.equipment/energy/hsy/period/hour/20190209T0930Z "Z signifies time zone as UTC") 

- __Local__ time with UTC offset, represented with a trailing `+/-` offset from UTC (e.g. `+0700`) 
    
    format: `YYYYMMDDTHHmmss.SSSS±HHmm`

    example: *1630 WIB == 0930 UTC*

    [https://api.sundaya.monitored.equipment/energy/hsy/period/hour/20190209T1630+0700](https://api.sundaya.monitored.equipment/energy/hsy/period/hour/20190209T1630+0700 "+0700 is JKT offset from  UTC")

Local time without offset (and therefore location unspecified) is not supported.

- API timestamp parameters in `GET` requests may be provided in either format: `UTC` time, or `Local` time.

- API timestamp parameters in `POST` requests must be provided as `Local` time. `UTC` time is not accepted.



## Media types
Request `Body` parameters and all response objects are sent and received in JSON. 

Clients should specify and consume `Accept` and `Content-Type` request and response headers in their applications. This ensures that an expected response is received, even as new media types or hypermedia schemes are introduced and designated as default in future.

These media types are currently supported and specified for each API path:

    application/vnd.collection+json
    application/json 
    text/html
    text/plain
    text/csv

A media type can be requested by sending an `Accept` header:

- If an `Accept` header is *not* specified (or is wildcarded with '*/*') the default `application/vnd.collection+json` media type will be returned. 

- If multiple `Accept` headers are sent the response will select the first matching media type in the list shown above, in the order shown.

- If the request `Accept` headers do not contain a supported type a `415`response (*Unsupported Media Type*) will be returned.



## Headers
This following example shows a sample HTTP request and response.
```
*** REQUEST ***	
GET /energy/hsy/period/week/20190204/ HTTPS/1.1	
Host: api.sundaya.monitored.equipment
Accept: application/vnd.collection+json	
x-api-key: X2zaSyASFGxf4PmOitVS1Dt911PcZ4IQ8PUUMqA
    
*** RESPONSE ***	
200 OK HTTPS/1.1	
Content-Type: application/vnd.collection+json	
Content-Length: 10757	
    
[{ "collection": {...}, ... }]
```
 

## Operations
The API supports basic CRUD operations (create, read, update, and delete) using standard HTTP method requests, as summarized in the following table.

Operation | Definition
--- | --- 
`GET` | Retrieve information about the resource.
`POST` | Create, copy, or restore the resource.
`PUT` | Replace or update the resource. 
`DELETE` | Delete the resource. 



## Response codes
The API supports a limited set of responses for each API path, based on the following.

Code | Status | Definition
--- | --- | ---
`200` | OK | Data created, or queued for later processing.
`201` | Created | Resource created.
`400` | Bad Request | The client specified an invalid argument. 
`401` | Unauthorized | The client does not have sufficient permission. 
`404` | Not Found | The resource was not found.
`415` | Unsupported Media Type | The requested Accept header type is not supported.
`500` | Internal Server Error | The server encountered an unexpected condition.




## Data element names
Dataset and repository elements should be named according to the following conventions.

These general conventions apply in addition to the element-specific conventions.
- no plurals except in dataset table names, API paths, and API field names.
- spaces removed
- periods and commas not allowed
- names short but meaningful
- numbers allowed

Element | Convention | Example
--- | --- | ---
`instance id` | lowercase, **hyphens** allowed | _reporting_, _sales-portal_ 
`cluster` | lowercase, **hyphens** allowed, '**c-**_n_' suffix | _kafka-c1_
`master instance` | cluster name and '**m-**_n_' suffix | _kafka-c1-m1_
`worker instance` | cluster name and '**w-**_n_' suffix | _kafka-c1-w1_
`table name` | lowercase, **underscores** allowed | _device_period_
`column family` | uppercase, **underscores** allowed | _ENERGY_PERIOD_
<code>column&nbsp;qualifier</code> |  lowercase, **underscores** allowed | _timeofdayhour_, _pack_id_
`field name` | lowercase and **underscores**, or mixedcase | _time_event_, _volts_, _productCategory_
`row id` | uppercase, **hash** allowed |_PMS#P00123#1425330757685_

Note: `field name` refers to fields in API messages and JSON documents.




## Dataset names

_Dataset_ names are based on their _Repository_ archetype and their _Content_ type. 

1. A _Content_ type mnemonic should **prefix** the _Dataset_ name.

2. A general descriptor should follow after the prefix.

The Dataset naming convention is therefore:
 ```xml
    <content-mnemonic>_<descriptive-qualifier>
 ```   
e.g.    `tel_pms` is the name of the _PMS Telemetry_ dataset.
        - `tel` is the \<content-mnemonic> for the **telemetry** _Content_ type.

The list of _Content_ mnemonics is provided in the [Storage/Content](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Storage) section.




## Storage service names

A `Storage` _Service_ is a service which writes data to a _Repository_.

1. The mnemonic `sto` should **prefix**  a storage _Service_ name.

2. A _Dataset_ name or the descriptor of the data being written, should follow after the prefix.

3. A _Repository_ qualifier should **suffix** the storage _Service_ name.

The storage _Service_ naming convention is therefore:
 ```xml
    sto.<dataset-descriptor>.<repository-qualifier>
 ```   
e.g.    `sto.tel_device.any_bq` is the name of the _Service_ for writing device _Telemetry_ to the _BigQuery Analytics_ repository.
        - `tel_device` is a general descriptor for the different device _Telemetry_ datasets.
        - `any_bq` is the the _BigQuery Analytics_ repository.

The list of _Repository_ qualifiers is provided in the [Storage/Respositories](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Storage) section. The list of _Dataset_ names is also provided in that section.




## Query service names

A `Query` _Service_ is a service which queries data from a _Repository_.

1. The mnemonic `qry` should **prefix**  a query _Service_ name.

2. A _Dataset_ name or the descriptor of the data being queried, should follow after the prefix.

3. A _Repository_ qualifier should **suffix** the query _Service_ name.

The query _Service_ naming convention is therefore:
 ```xml
    qry.<dataset-descriptor>.<repository-qualifier>
 ```   
e.g.    `qry.eng_period.rpt_bt` is the name of the _Service_ for querying _Energy Periods_ data from the _Bigtable Reporting_ repository.

The list of _Repository_ qualifiers and dataset names is provided in [Storage](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Storage) section. The list of _Dataset_ names is also provided in that section.




## Topic names

Messaging topics are named according to the _Dataset_ or data descriptor based on the following convention.

1. `pub` should **prefix** the _Topic_ name. 

2. A _Dataset_ name or the descriptor of the data being published, should follow after the prefix.

The _Topic_ naming convention is therefore:
 ```xml
    pub.<dataset-descriptor>
 ```   
 e.g.   `pub.tel_pms` is the name of the _Topic_ for _PMS Telemetry_.




## Subsription names

Messaging subscriptions are named according to the data descriptor, and the target _Repository_ or other descriptor, as follows.

1. `sub` should **prefix** the _Subscription_ name. 

2. The _Dataset_ name or the descriptor of the data in the subscribed _Topic_, should follow after the prefix.

3. The _Repository_ qualifier or the general descriptor of the _Subscriber's_ production target should **suffix** the name.

The _Topic_ naming convention is therefore:
 ```xml
    sub.<dataset-descriptor>.<target-descriptor>
 ```   
 e.g.   `sub.tel_pms.any_bq` is the name of the _Subscription_ for storing _PMS Telemetry_ data in the _BigQuery Analytics_ repository.
        `sub.tel_pms.rpt_bt` is the name of the _Subscription_ for storing _PMS Telemetry_ data in the _Bigtable Reporting_ repository.



## Producer service names

A `Producer` _Service_ is a service which publishes messages to an _API_ endpoint or to a messaging _Topic_.

1. The mnemonic `api` or `pub` should **prefix**  a producer _Service_ name depending on whether its production target is an _API_ or _Topic_.

2. A _Dataset_ name or the descriptor of the data being published, should follow after the prefix.

The producer _Service_ naming convention is therefore:
 ```xml
    api|pub.<dataset-descriptor>
 ```   
e.g.    `pub.tel_device.` is the name of the producer _Service_ for publishing device _Telemetry_ to various topics.




## Abbreviations
Field names may include canonical abbreviations for units based on [RFC8428](https://www.iana.org/assignments/senml/senml.xhtml) standard for Sensor Measurement Lists (SenML).

The abbreviations are optional and recommended as they help shorten field names (which often repeat in a dataset message) and therefore reduce transmitted bytes, especially as it this addresses bandwidth-constrained limitations in device edge to cloud links. 

The abbreviations may be used as a prefix or suffix of a field name. 

The following is a list of recommended abbreviations.

Term                 | Abbreviation    | Example       | Scale
---                  | ---             | ---           | ---
volts                | `V`             | _pv_V_        |
amps                 | `A`             | _pv_A_        |
watts                | `W`             | _pv_W_        |
joules,<br>megajoules | `J`,<br>`MJ`   | <br>_pack_MJ_ | 
watt-hour<br>kilowatt-hour  | `J`      |               | 3,600<br>3,600,000
degrees Celsius      | `Cel`           | _fet_Cel_     |
time                 | `t`             | _t_zone_      |
second,<br>millisecond | `s`,<br>`ms`  |               |
lumen                | `lm`            |               |

In the above example _t_zone_ refers to '_time zone_'.

These abbreviations are optional and may not be used in all cases such as when a field name should be descriptive to assist new developers.

---
