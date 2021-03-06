# Standards & Conventions
---

## Versions
The API endpoint host is [http://api.endpoints.sundaya.cloud.goog](http://api.endpoints.sundaya.cloud.goog). 

All requests to the API endpoint receive the latest version of the API.     

Client applications may request a specific version through the `Accept` header.

    Accept: application/vnd.sundaya.v1.0+yaml

## Date-time Format
Date and time parameters must be expressed in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format and must conform to [RFC3359](https://tools.ietf.org/html/rfc3339) .

    http://api.endpoints.sundaya.cloud.goog/energy/{type}/{period}/{epoch}

e.g. [http:/api.endpoints.sundaya.cloud.goog/energy/hse/period/week/20190210](http:/api.endpoints.sundaya.cloud.goog/energy/hse/period/week/20190210)

The compressed version of ISO 8601 is required, without semi colons and with `T` as the time designator, as shown in examples below.

## Timezones
Timezones must be represented in API parameters as `UTC` time, or as `Local` time with UTC offset. 

The supported timezone formats are described below with examples: 

- __UTC__ time, represented with a trailing `Z` or trailing zero offset `+0000`

    format: `YYYYMMDDTHHmmss.SSSSZ` or `YYYYMMDDTHHmmss.SSSS+0000`

    example: *0930 UTC == 1630 WIB (Indonesian Western Standard Time)*
    
    [http://api.endpoints.sundaya.cloud.goog/energy/hse/period/hour/20190209T0930Z](http://api.endpoints.sundaya.cloud.goog/energy/hse/period/hour/20190209T0930Z "Z signifies time zone as UTC") 

- __Local__ time with UTC offset, represented with a trailing `+/-` offset from UTC (e.g. `+0700`) 
    
    format: `YYYYMMDDTHHmmss.SSSS±HHmm`

    example: *1630 WIB == 0930 UTC*

    [http://api.endpoints.sundaya.cloud.goog/energy/hse/period/hour/20190209T1630+0700](http://api.endpoints.sundaya.cloud.goog/energy/hse/period/hour/20190209T1630+0700 "+0700 is JKT offset from  UTC")

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
GET /energy/hse/period/week/20190204/ HTTP/1.1	
Host: api.endpoints.sundaya.cloud.goog
Accept: application/vnd.collection+json	
api_key: X2zaSyASFGxf4PmOitVS1Dt911PcZ4IQ8PUUMqA
    
*** RESPONSE ***	
200 OK HTTP/1.1	
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

---

## Security

The API requires a secret Key to authenticate your account. 

Your client application authenticates to the API by providing a secret key in the request header.

### Requesting an API key

You can [request access](mailto:admin@api.sundaya.com) by signing up for an account. 

You can also request an additional key if you need to replace your key in future.

### API key usage

Each client application vendor has a unique key. The API gateway pre-authorises requests using this key and references it in request logs for traceability.

An API key is required in all paths except the following:

- The 'GET' operations in the `/api/versions` path does not require an API key.

- The 'GET' operations in the `/energy` path does not require an API key for the default site (`site=999`) which is reserved for aggregate querying.

In all other paths an API key is required using one of the following options.

Method | Parameter | In | Description
--- | --- | --- | ---
GET | `api_key` | query | `api_key` query parameter is accepted only in GET requests. 
POST, GET | `x-api-key` | header | POST requests must provide a `x-api-key` header. This option is also preferred for GET requests.

```
*** REQUEST ***	
POST /devices/dataset/pms HTTP/1.1	
Host: api.endpoints.sundaya.cloud.goog
Accept: application/vnd.collection+json	
x-api-key: X2zaSyASFGxf4PmOXtVS1Dt911PcZ4IQ8PUUMqX
```

```
*** REQUEST ***	
GET energy/hse/period/week/20190930T1200?api_key=X2zaSyASFGxf4PmOXt911PcZ4IQ8PUUMqX HTTP/1.1
Host: api.endpoints.sundaya.cloud.goog
Accept: text/html	
api_key: X2zaSyASFGxf4PmOXt911PcZ4IQ8PUUMqX

```

`401 Unauthorized` is returned if the API key is not valid or does not have access to the requested path.

```
*** RESPONSE ***	
401 Unauthorized HTTP/1.1	
Content-Type: application/json; charset=utf-8
Content-Length: 286
```

### Portal usage

The developer portal injects a `api_key` **query** parameter when using the 'Try this API' interface for GET requests. The parameter is not inspected by the API host and is required only for portal functionality. 

A `x-api-key` **header** must be populated and sent when using 'Try this API' with POST requests. 
