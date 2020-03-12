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

An abbreviated _Content_ type mnemonic should **prefix** _Dataset_ names.

- The list of _Content_ mnemonics is provide in the [Storage/Content](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Storage) section.

The Dataset naming convention is therefore: 
\<content-mnemonic>_\<descriptive-qualifier>
    
e.g. `tel_pms` is the name of the _PMS Telemetry_ dataset.
- `tel` is the /<content-mnemonic> for the **telemetry** _Content_ type.
- the fully-qualified dataset name includes the repository name: `analytics.tel_pms` 

## Storage Service names

An abbreviated _Storage_ class mnemonic should **prefix** all `Storage` _Service_ names.

- A `Storage` _Service_ is either a _Writer_ or _Reader_ of a _Repository_.  

- The list of _Storage_ class mnemonics is provide in the [Storage](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Storage) section.

A _Repository_ qualifier should **suffix** all `Storage` _Service_ names.

- The list of _Repository_ qualifiers is provides in the [Storage\Repository](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Storage) section.

The `Storage` _Service_ naming convention is therefore: 
    \<storage-class-mnemonic>.\<dataset-name>.\<repository-qualifier>
    
    e.g. `wri.tel_pms.any_bq`       XXXXXXXXXXXXXXXXX should be wri.any_bq which takes a tel_pms as an argument

    - is the name of the _Writer_ for _PMS Telemetry_ data to the _BigQuery Analytics_ repository.

## Messaging Service names

An abbreviated _Messaging_ class mnemonic should **prefix** all `Messaging` _Service_ names.

- The list of _Messaging_ class mnemonics is provide in the [Messaging](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Messaging) section.

A _Dataset_ qualifier should **suffix** all `Storage` _Service_ names.


A `Messaging` _Service_ is either:

- a _Producer_ or _Consumer_ of a _Dataset_

- a _Publisher_ or _Consumer_ of a _Topic/ Subscription_





    \<messaging-class-mnemonic>.\<dataset-name>[.\<topic_name>]

    e.g. `prd.tel_pms` 

    - is the name of the _Producer_ service for _PMS Telemetry_ (which calls a _Storage_ or _Publisher_ service as described in [Messaging](/docs/api.sundaya.monitored.equipment/0/c/Implementation/Architecture/Messaging)).
    - the \<descriptive-qualifier> is optional


## Messaging topic names
Messaging system topics and subscriptions should be named according to these general conventions.
- topics prefixed with `pub_` 
- subscription names are prefixed with `sub_` 


## Abbreviations
Field names may contain the following canonical abbreviations to shorten the number of bytes needed to transmit or store the field.

An abbreviation may only be used as itself, or as a prefix or suffix of a field name. 

The following examples describe the allowed use of abbreviations.

Term                 | Abbreviation    | Example
---                  | ---             | ---
volts                | `v`             | _pv_v_
amps                 | `a`             | _pv_a_
watts                | `w`             | _pv_w_
joules,<br>megajoules | `j`, `mj`       | _pack_mj_
temperature<br>time  | `t`             | _fet_t_<br>_t_zone_, 

`t` may refer to _temperature_ or _time_: in the above examples _t_zone_ refers to '_time zone_' and _fet_t_ refers to '_fet temperature_'.

if the semantics of the field name do not remove ambiguity _time_ should be assumed as the default.





---
