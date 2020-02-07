# energy GET
---

### GET /energy request

[/energy/hsy/period/week/20190304/1](https://api.sundaya.monitored.equipment/energy/hsy/period/week/20190304/1?site=999)

The example below retrieves energy data for a `week` period from devices at site # 999. 

*__Note__: The optional body parameter filters the returned dataset by product type. If a body is not provided, energy data will be returned for all devices.*

```
*** REQUEST ***	
GET /energy/hsy/period/week/20190204/1?site=999 HTTPS/1.1	
Host: api.sundaya.monitored.equipment
Accept: application/vnd.collection+json, application/vnd.sundaya.v1.0+yaml
Content-Type: application/json
```

```json
Body: {
    "productCatalogItems": [
        {
            "productCategory": "Solutions",
            "productSubcategory": "Solar Home Kits",
            "productType": "JouleBox"
        },
        {
            "productCategory": "Solutions & Components"
        }
    ]
}
```


### GET /energy response

```    
*** RESPONSE ***	
200 OK HTTPS/1.1	
Content-Type: application/vnd.collection+json	
Content-Length: 3495	
```

```json
[
    {
        "collection": {
            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/week/20190204/undefined?site=999",
            "version": "0.3.14.24",
            "links": [
                {
                    "rel": "self", "name": "week", "prompt": "Week 04-02-2019", "title": "04/02/19 - 10/02/19",
                    "description": "hsy week 20190204 1 999",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/week/20190204/1?site=999",
                    "render": "link"
                },
                {
                    "rel": "collection", "name": "week.day", "prompt": "Mon Feb 4th - Sun Feb 10th",
                    "title": "04/02/19 - 10/02/19",
                    "description": "Mon Tue Wed Thu Fri Sat Sun",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190204/7?site=999"
                },
                {
                    "rel": "collection", "name": "day.timeofday", "prompt": "Feb 4 Night - Feb 10 Evening", 
                    "title": "04/02/19 00:00 - 10/02/19 23:59",
                    "description": "Night Morning Afternoon Evening",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/timeofday/20190204T0000/28?site=999"
                },
                {
                    "rel": "up", "name": "month", "prompt": "Feb 2019", 
                    "title": "01/02/19 - 28/02/19",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/month/20190201/1?site=999",
                    "render": "link"
                },
                {
                    "rel": "next", "name": "week", "prompt": "Week 11-02-2019", 
                    "title": "11/02/19 - 17/02/19",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/week/20190211/1?site=999",
                    "render": "link"
                },
                {
                    "rel": "prev", "name": "week", "prompt": "Week 28-01-2019", 
                    "title": "28/01/19 - 03/02/19",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/week/20190128/1?site=999",
                    "render": "link"
                },
                {
                    "rel": "service-meta", "name": "period", "key": "period", 
                    "value": {
                        "period": "week", "context": "week", "duration": 1, "epochInstant": "20190204T000000.0000",                        "endInstant": "20190210T235959.9990", "prompt": "Week 04-02-2019"
                    }
                },
                {
                    "rel": "service-meta", "name": "period", "key": "child", 
                    "value": {
                        "period": "day", "context": "week.day", "duration": 7, "epochInstant": "20190204T000000.0000",
                        "endInstant": "20190210T235959.9990", "prompt": "Mon Feb 4th - Sun Feb 10th"
                    }
                },
                {
                    "rel": "service-meta", "name": "period", "key": "grandchild", 
                    "value": { 
                        "period": "timeofday", "context": "day.timeofday", "duration": 28, "epochInstant": "20190204T000000.0000", 
                        "endInstant": "20190210T235959.9990", "prompt": "Feb 4 Night - Feb 10 Evening"
                    }
                }
            ],
            "items": [
                {
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190204/undefined?site=999",
                    "links": [
                        {
                            "rel": "self", 
                            "name": "week.day",
                            "prompt": "Mon Feb 4th",
                            "title": "04/02/19",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190204/1?site=999",
                            "render": "link"
                        },
                        {
                            "rel": "collection",
                            "name": "day.timeofday",
                            "prompt": "Feb 4 Night - Feb 4 Evening",
                            "title": "04/02/19 00:00 - 04/02/19 23:59",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/timeofday/20190204T0000/4?site=999"
                        }
                    ],
                    "data": [
                        {
                            "name": "week.day",
                            "value": "20190204/1",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "56.570"
                                },
                                {
                                    "name": "store.in",
                                    "value": "33.927"
                                },
                                {
                                    "name": "store.out",
                                    "value": "61.614"
                                },
                                {
                                    "name": "yield",
                                    "value": "56.572"
                                },
                                {
                                    "name": "sell",
                                    "value": "38.995"
                                },
                                {
                                    "name": "buy",
                                    "value": "28.702"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190204T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "10.184 18.051 18.664 9.671"
                                },
                                {
                                    "name": "store.in",
                                    "value": "13.804 4.325 13.052 2.746"
                                },
                                {
                                    "name": "store.out",
                                    "value": "16.564 10.821 14.423 19.806"
                                },
                                {
                                    "name": "yield",
                                    "value": "16.258 13.689 9.304 17.321"
                                },
                                {
                                    "name": "sell",
                                    "value": "3.022 16.463 7.674 11.836"
                                },
                                {
                                    "name": "buy",
                                    "value": "4.400 8.581 4.730 10.991"
                                }
                            ]
                        }
                    ]
                },
                {
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190205/undefined?site=999",
                    "links": [
                        {
                            "rel": "self",
                            "name": "week.day",
                            "prompt": "Tue Feb 5th",
                            "title": "05/02/19",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190205/1?site=999",
                            "render": "link"
                        },
                        {
                            "rel": "collection",
                            "name": "day.timeofday",
                            "prompt": "Feb 5 Night - Feb 5 Evening",
                            "title": "05/02/19 00:00 - 05/02/19 23:59",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/timeofday/20190205T0000/4?site=999"
                        }
                    ],
                    "data": [
                        {
                            "name": "week.day",
                            "value": "20190205/1",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "26.196"
                                },
                                {
                                    "name": "store.in",
                                    "value": "56.628"
                                },
                                {
                                    "name": "store.out",
                                    "value": "36.043"
                                },
                                {
                                    "name": "yield",
                                    "value": "48.202"
                                },
                                {
                                    "name": "sell",
                                    "value": "55.822"
                                },
                                {
                                    "name": "buy",
                                    "value": "54.568"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190205T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "5.798 9.387 7.153 3.858"
                                },
                                {
                                    "name": "store.in",
                                    "value": "13.859 15.008 19.250 8.511"
                                },
                                {
                                    "name": "store.out",
                                    "value": "12.960 4.432 9.256 9.395"
                                },
                                {
                                    "name": "yield",
                                    "value": "12.742 5.670 18.395 11.395"
                                },
                                {
                                    "name": "sell",
                                    "value": "18.583 11.587 11.442 14.210"
                                },
                                {
                                    "name": "buy",
                                    "value": "3.124 15.476 16.943 19.025"
                                }
                            ]
                        }
                    ]
                },
                {
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190206/undefined?site=999",
                    "links": [
                        {
                            "rel": "self",
                            "name": "week.day",
                            "prompt": "Wed Feb 6th",
                            "title": "06/02/19",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190206/1?site=999",
                            "render": "link"
                        },
                        {
                            "rel": "collection",
                            "name": "day.timeofday",
                            "prompt": "Feb 6 Night - Feb 6 Evening",
                            "title": "06/02/19 00:00 - 06/02/19 23:59",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/timeofday/20190206T0000/4?site=999"
                        }
                    ],
                    "data": [
                        {
                            "name": "week.day",
                            "value": "20190206/1",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "46.611"
                                },
                                {
                                    "name": "store.in",
                                    "value": "44.131"
                                },
                                {
                                    "name": "store.out",
                                    "value": "39.703"
                                },
                                {
                                    "name": "yield",
                                    "value": "39.117"
                                },
                                {
                                    "name": "sell",
                                    "value": "60.981"
                                },
                                {
                                    "name": "buy",
                                    "value": "76.797"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190206T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "12.417 11.265 7.452 15.477"
                                },
                                {
                                    "name": "store.in",
                                    "value": "18.048 6.352 16.345 3.386"
                                },
                                {
                                    "name": "store.out",
                                    "value": "2.957 9.657 18.212 8.877"
                                },
                                {
                                    "name": "yield",
                                    "value": "13.022 2.984 11.715 11.396"
                                },
                                {
                                    "name": "sell",
                                    "value": "10.654 18.597 12.645 19.085"
                                },
                                {
                                    "name": "buy",
                                    "value": "19.441 17.596 20.526 19.234"
                                }
                            ]
                        }
                    ]
                },
                {
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190207/undefined?site=999",
                    "links": [
                        {
                            "rel": "self",
                            "name": "week.day",
                            "prompt": "Thu Feb 7th",
                            "title": "07/02/19",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190207/1?site=999",
                            "render": "link"
                        },
                        {
                            "rel": "collection",
                            "name": "day.timeofday",
                            "prompt": "Feb 7 Night - Feb 7 Evening",
                            "title": "07/02/19 00:00 - 07/02/19 23:59",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/timeofday/20190207T0000/4?site=999"
                        }
                    ],
                    "data": [
                        {
                            "name": "week.day",
                            "value": "20190207/1",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "56.842"
                                },
                                {
                                    "name": "store.in",
                                    "value": "64.177"
                                },
                                {
                                    "name": "store.out",
                                    "value": "39.310"
                                },
                                {
                                    "name": "yield",
                                    "value": "68.785"
                                },
                                {
                                    "name": "sell",
                                    "value": "49.786"
                                },
                                {
                                    "name": "buy",
                                    "value": "23.785"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190207T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "20.552 9.054 20.075 7.161"
                                },
                                {
                                    "name": "store.in",
                                    "value": "19.879 18.029 17.228 9.041"
                                },
                                {
                                    "name": "store.out",
                                    "value": "4.165 6.034 12.873 16.238"
                                },
                                {
                                    "name": "yield",
                                    "value": "19.699 15.295 13.705 20.086"
                                },
                                {
                                    "name": "sell",
                                    "value": "9.460 18.805 10.248 11.273"
                                },
                                {
                                    "name": "buy",
                                    "value": "8.390 2.863 8.801 3.731"
                                }
                            ]
                        }
                    ]
                },
                {
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190208/undefined?site=999",
                    "links": [
                        {
                            "rel": "self",
                            "name": "week.day",
                            "prompt": "Fri Feb 8th",
                            "title": "08/02/19",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190208/1?site=999",
                            "render": "link"
                        },
                        {
                            "rel": "collection",
                            "name": "day.timeofday",
                            "prompt": "Feb 8 Night - Feb 8 Evening",
                            "title": "08/02/19 00:00 - 08/02/19 23:59",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/timeofday/20190208T0000/4?site=999"
                        }
                    ],
                    "data": [
                        {
                            "name": "week.day",
                            "value": "20190208/1",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "48.078"
                                },
                                {
                                    "name": "store.in",
                                    "value": "48.018"
                                },
                                {
                                    "name": "store.out",
                                    "value": "50.699"
                                },
                                {
                                    "name": "yield",
                                    "value": "49.278"
                                },
                                {
                                    "name": "sell",
                                    "value": "51.296"
                                },
                                {
                                    "name": "buy",
                                    "value": "43.716"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190208T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "5.146 18.784 16.488 7.660"
                                },
                                {
                                    "name": "store.in",
                                    "value": "5.458 11.376 12.744 18.440"
                                },
                                {
                                    "name": "store.out",
                                    "value": "20.361 5.182 12.005 13.151"
                                },
                                {
                                    "name": "yield",
                                    "value": "6.446 17.492 17.149 8.191"
                                },
                                {
                                    "name": "sell",
                                    "value": "19.457 11.394 9.071 11.374"
                                },
                                {
                                    "name": "buy",
                                    "value": "9.368 11.416 18.173 4.759"
                                }
                            ]
                        }
                    ]
                },
                {
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190209/undefined?site=999",
                    "links": [
                        {
                            "rel": "self",
                            "name": "week.day",
                            "prompt": "Sat Feb 9th",
                            "title": "09/02/19",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190209/1?site=999",
                            "render": "link"
                        },
                        {
                            "rel": "collection",
                            "name": "day.timeofday",
                            "prompt": "Feb 9 Night - Feb 9 Evening",
                            "title": "09/02/19 00:00 - 09/02/19 23:59",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/timeofday/20190209T0000/4?site=999"
                        }
                    ],
                    "data": [
                        {
                            "name": "week.day",
                            "value": "20190209/1",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "57.824"
                                },
                                {
                                    "name": "store.in",
                                    "value": "47.738"
                                },
                                {
                                    "name": "store.out",
                                    "value": "33.678"
                                },
                                {
                                    "name": "yield",
                                    "value": "57.888"
                                },
                                {
                                    "name": "sell",
                                    "value": "54.465"
                                },
                                {
                                    "name": "buy",
                                    "value": "60.211"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190209T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "17.147 14.755 16.075 9.847"
                                },
                                {
                                    "name": "store.in",
                                    "value": "13.478 15.928 5.792 12.540"
                                },
                                {
                                    "name": "store.out",
                                    "value": "9.534 8.306 9.863 5.975"
                                },
                                {
                                    "name": "yield",
                                    "value": "17.747 18.682 14.194 7.265"
                                },
                                {
                                    "name": "sell",
                                    "value": "15.311 7.422 20.293 11.439"
                                },
                                {
                                    "name": "buy",
                                    "value": "13.771 18.717 14.601 13.122"
                                }
                            ]
                        }
                    ]
                },
                {
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190210/undefined?site=999",
                    "links": [
                        {
                            "rel": "self",
                            "name": "week.day",
                            "prompt": "Sun Feb 10th",
                            "title": "10/02/19",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190210/1?site=999",
                            "render": "link"
                        },
                        {
                            "rel": "collection",
                            "name": "day.timeofday",
                            "prompt": "Feb 10 Night - Feb 10 Evening",
                            "title": "10/02/19 00:00 - 10/02/19 23:59",
                            "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/timeofday/20190210T0000/4?site=999"
                        }
                    ],
                    "data": [
                        {
                            "name": "week.day",
                            "value": "20190210/1",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "33.669"
                                },
                                {
                                    "name": "store.in",
                                    "value": "53.197"
                                },
                                {
                                    "name": "store.out",
                                    "value": "50.641"
                                },
                                {
                                    "name": "yield",
                                    "value": "53.124"
                                },
                                {
                                    "name": "sell",
                                    "value": "61.102"
                                },
                                {
                                    "name": "buy",
                                    "value": "53.471"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190210T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "7.512 9.892 3.522 12.743"
                                },
                                {
                                    "name": "store.in",
                                    "value": "15.399 14.689 18.929 4.180"
                                },
                                {
                                    "name": "store.out",
                                    "value": "11.374 15.894 9.721 13.652"
                                },
                                {
                                    "name": "yield",
                                    "value": "14.726 14.333 18.406 5.659"
                                },
                                {
                                    "name": "sell",
                                    "value": "19.582 19.311 19.055 3.154"
                                },
                                {
                                    "name": "buy",
                                    "value": "7.627 9.419 20.305 16.120"
                                }
                            ]
                        }
                    ]
                }
            ]
        }
    }
]
```
