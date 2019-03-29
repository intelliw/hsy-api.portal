# Responses
---

## Link format

Response data is returned in a JSON Collection and includes links for the user to navigate to adjacent datasets, for example to the next or previous *week*, or to the previous *month*.

Links have the following attributes:

- **rel**  - The rel property contains the link-to-collection relationship descriptor, which can be one of the following: `self`, `collection`, `up`, `next`, `prev`. These are described below in **Link-relation types** 

- **prompt** - A brief description of the link.

- **title** - The title to display alongside the data for this item, and can be used as a caption or tooltip in the presentation..

- **href** - The URI or URL of the resource.

- **render** - 'image' or 'text' if the link should be retrieved and embedded; or 'link' to display as-is. If the property is missing the href link does not need to be presented.

The following table shows a set of links provided with energy data for a *week* period. 

![Links in energy data](../../images/collection-links-table.png)



## Link-relation types
Link-relations in Response objects are based on [RFC8288](https://tools.ietf.org/html/rfc8288#page-6). 

The following registered types are returned in the `rel` attribute of links in `application/vnd.collection+json` responses. 
- **self**	- Identifies the link's context.

    In `collection.links` it points to the collection as a whole (`name`=*'week'*)            

    e.g. href=[http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20190210](http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/week/20190210)

    In `collection.items.links` it points to a child item in the collection (`name`=*'day'*).

    e.g. href=[http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/day/20190204](http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/day/20190204)

- **collection** - in `collection.links` it points to the child items which make up the collection (`name`=*'week.day'*).
    
    e.g. href=[http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/day/20190204](http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/day/20190204)

- **item** - in `collection.items.links` it points to the subitems of the child item: i.e the grandchild items of the collection (`name`=*'day.hour'*).

    e.g. href=[http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/hour/20190205T0600](http:/api.endpoints.sundaya.cloud.goog/energy/hse/periods/hour/20190205T0600)

- **up** - Identifies the parent of the collection or item (`name`=*'month'* if a link is in collection object for a *'week'*).
    
- **next**, **prev** - Identifies the next or previous sibling of the item series (`name` = *'week'*). The `prompt` and `title` properties signify the next or previous item in the series (`prompt` = *'Week 07 2019'*).



## Graph format

The graph format including display labels and hyperlinks to navigate to related datasets is shown below. 

![Data element mappings for graph rendering](../../images/graph.data-mappings.png)