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

e.g. [http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20190210](http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20190210)

The compressed version of ISO 8601 is required, without semi colons and with `T` as the time designator, as shown in examples below.


## Timezones
Timezones are not assumed and must be explicitly specified where API parameters allow for a timestamp to be provided. 

The Timezone can be specified in UTC or local time as shown:

- __UTC__, expressed with a trailing `Z` 

    e.g. [http://api.endpoints.sundaya.cloud.goog/energy/hse/periods/minute/20190209T0930Z](http://api.endpoints.sundaya.cloud.goog/energy/hse/periods/minute/20190209T0930Z "Z signifies time zone as UTC") == 09:30 UTC

- __Local__ time in Jakarta with +/- offset 

    http://api.endpoints.sundaya.cloud.goog/energy/hse/periods/minute/YYYYMMDDTHHmmss.SSS±HHmm

    e.g. [http://api.endpoints.sundaya.cloud.goog/energy/hse/periods/minute/20190209T1630-0700](http://api.endpoints.sundaya.cloud.goog/energy/hse/periods/minute/20190209T1630-0700 "-0700 is JKT offset from  UTC") == 09:30 UTC
## Media types
Request `Body` parameters and all response objects are sent and received in JSON. 

Clients should specify and consume `Accept` and `Content-Type` request and response headers to help maintain backward compatibility in their applications. This ensures that an expected response is received, even as new media types or hypermedia schemes are introduced and designated as default.

These media types are currently supported:

    application/json 
    application/vnd.collection+json
    text/html

## Headers
This following example shows a sample HTTP request and response.
```
*** REQUEST ***	
GET /energy/hse/periods/week/20190204/ HTTP/1.1	
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

The following registered types are returned in the `rel` attribute of links in `application/vnd.collection+json` responses. 
- **self**	- Identifies the link"s context.

    In `collection.links` it points to the collection as a whole (`name` = *week*)            

    - e.g. href=<a>[http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20190210](http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20190210)</a>

    In `collection.items.links` it points to a child item in the collection (`name`=*day*).
    - e.g. href=<a>[http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/day/20190204](http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/day/20190204)</a>

- **collection** - in `collection.links` it points to the child items which make up the collection (`name`=*week.day*).
    
    - e.g. href=<a>[http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/day/20190204](http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/day/20190204)</a>

- **item** - in `collection.items.links` it points to the subitems of the child item: i.e the grandchild items of the collection (`name`=*day.hour*).

    - e.g. href=<a>[http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/hour/201902050600](http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/hour/201902050600)</a>

- **up** - Identifies the parent of the collection or item (`name`=*month* if the  link is in a 'week' collection object).
    
- **next**, **prev** - Identifies the next or previous sibling of the item series (`name` = *week*). The `prompt` and `title` properties signify the next or previous item in the series (`prompt` = *Week 07 2019*).

    
