# Standards & Conventions
---

## Versions
The API endpoint host is http://api.endpoints.sundaya.cloud.goog. 

All requests to the API endpoint receive the latest version of the API.     

Client applications may request a specific version through the `Accept` header.

    Accept: application/vnd.sundaya.v1+yaml

## Date-time Format
Date and time parameters must be expressed in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format and must conform to [RFC3359](https://tools.ietf.org/html/rfc3339) .

    http://api.endpoints.sundaya.cloud.goog/energy/{energyTpe}/period/{periodType}/{endDateTime}

e.g. http:/api.endpoints.sundaya.cloud.goog/energy/hse/period/week/20190210

The compressed version of ISO 8601 is required, without semi colons and with `T` as the time designator, as shown in the examples with timezones below.


## Timezones
Timezones are not assumed and must be explicitly specified where API parameters allow for a timestamp to be provided. 

The Timezone can be specified in UTC or local time as shown:

- __UTC__, expressed with a trailing `Z` 

    e.g. http://api.endpoints.sundaya.cloud.goog/energy/hse/period/minute/20190209T0930Z == 09:30 UTC

- __Local__ time in Jakarta with +/- offset 

    http://api.endpoints.sundaya.cloud.goog/energy/hse/period/week/YYYYMMDDTHHmmss.SSS±HHmm

    e.g. http://api.endpoints.sundaya.cloud.goog/energy/hse/period/minute/20190209T1630-0700 == 09:30 UTC
## Media types
Request `Body` parameters and all response objects are sent and received in JSON. 

Clients should use `Accept` and `Content-Type` headers to preserve backward compatibility (in case a new media type or hypermedia scheme is introduced and designated as default).

These media types are supported:

    application/json 
    application/vnd.collection+json

## Headers
This following example shows a sample HTTP request and response.
```
*** REQUEST ***	
GET /energy/hse/period/week/20190210/ HTTP/1.1	
Host: api.endpoints.sundaya.cloud.goog	
Accept: application/vnd.collection+json	
    
*** RESPONSE ***	
200 OK HTTP/1.1	
Content-Type: application/vnd.collection+json	
Content-Length: xxx	
    
{ "collection" : {...}, ... }
```

## Link-relation types
Link-relations in Response objects are based on [RFC8288](https://tools.ietf.org/html/rfc8288#page-6). 

The following registered types are referenced in the `rel` attribute of the links in an `application/vnd.collection+json` response. 
- **self**	- Identifies the link"s context.

    In `collection.links` it identifies the collection (name = *week*)            

    - e.g. href=<a>http:/api.endpoints.sundaya.cloud.goog/energy/hse/period/week/20190210</a>

    In `collection.items.links` it identifies an item in the collection (name = *day*).
    - e.g. href=<a>http:/api.endpoints.sundaya.cloud.goog/energy/hse/period/day/20190204</a>

- **collection** - in `collection.links` it targets the item series whiich make up the collection (name = *week.days*).
    
    - e.g. href=<a>http:/api.endpoints.sundaya.cloud.goog/energy/hse/interval/day/20190204/20190210</a>

- **item** - in `collection.items.links` it targets subitems of the item in that context (name = *day.hours*).

    - e.g. href=<a>http:/api.endpoints.sundaya.cloud.goog/energy/hse/interval/hour/201902050600/201902050500</a>

- **up** - Identifies the parent the collection or item represented by the link"s context (name = *week.month*).
    
- **next** - Identifies the next sibling of the collection or item series represented by the link"s context (name = *week.next*).

- **prev** - Identifies the previous siblings of the collection or item series represented by the link"s context (name = *week.previous*).
    
