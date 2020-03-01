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
                    "rel": "self",
                    "name": "week",
                    "prompt": "Week 04-02-2019",
                    "title": "04/02/19 - 10/02/19",
                    "description": "hsy week 20190204 1 999",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/week/20190204/1?site=999",
                    "render": "link"
                },
                {
                    "rel": "collection",
                    "name": "week.day",
                    "prompt": "Mon Feb 4th - Sun Feb 10th",
                    "title": "04/02/19 - 10/02/19",
                    "description": "Mon Tue Wed Thu Fri Sat Sun",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/day/20190204/7?site=999"
                },
                {
                    "rel": "collection",
                    "name": "day.timeofday",
                    "prompt": "Feb 4 Night - Feb 10 Evening",
                    "title": "04/02/19 00:00 - 10/02/19 23:59",
                    "description": "Night Morning Afternoon Evening",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/timeofday/20190204T0000/28?site=999"
                },
                {
                    "rel": "up",
                    "name": "month",
                    "prompt": "Feb 2019",
                    "title": "01/02/19 - 28/02/19",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/month/20190201/1?site=999",
                    "render": "link"
                },
                {
                    "rel": "next",
                    "name": "week",
                    "prompt": "Week 11-02-2019",
                    "title": "11/02/19 - 17/02/19",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/week/20190211/1?site=999",
                    "render": "link"
                },
                {
                    "rel": "prev",
                    "name": "week",
                    "prompt": "Week 28-01-2019",
                    "title": "28/01/19 - 03/02/19",
                    "href": "http://api.sundaya.monitored.equipment/energy/hsy/period/week/20190128/1?site=999",
                    "render": "link"
                },
                {
                    "rel": "service-meta",
                    "name": "period",
                    "description": {
                        "name": "week",
                        "epochInstant": "20190204T000000.0000",
                        "endInstant": "20190210T235959.9990",
                        "duration": 1,
                        "child": {
                            "name": "day",
                            "epochInstant": "20190204T000000.0000",
                            "endInstant": "20190210T235959.9990",
                            "duration": 7
                        },
                        "grandchild": {
                            "name": "timeofday",
                            "epochInstant": "20190204T000000.0000",
                            "endInstant": "20190210T235959.9990",
                            "duration": 28
                        }
                    },
                    "href": "https://docs.sundaya.monitored.equipment/docs/api.sundaya.monitored.equipment/0/c/Getting%20Started/API%20Overview/Energy%20API"
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
                                    "value": "44.300"
                                },
                                {
                                    "name": "store.in",
                                    "value": "47.609"
                                },
                                {
                                    "name": "store.out",
                                    "value": "42.587"
                                },
                                {
                                    "name": "yield",
                                    "value": "33.090"
                                },
                                {
                                    "name": "sell",
                                    "value": "62.252"
                                },
                                {
                                    "name": "buy",
                                    "value": "45.682"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190204T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "16.064 16.189 5.156 6.891"
                                },
                                {
                                    "name": "store.in",
                                    "value": "20.354 5.529 11.205 10.521"
                                },
                                {
                                    "name": "store.out",
                                    "value": "8.711 15.160 10.884 7.832"
                                },
                                {
                                    "name": "yield",
                                    "value": "7.066 15.581 4.810 5.633"
                                },
                                {
                                    "name": "sell",
                                    "value": "16.555 17.294 19.887 8.516"
                                },
                                {
                                    "name": "buy",
                                    "value": "11.082 10.238 4.517 19.845"
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
                                    "value": "34.754"
                                },
                                {
                                    "name": "store.in",
                                    "value": "43.215"
                                },
                                {
                                    "name": "store.out",
                                    "value": "51.386"
                                },
                                {
                                    "name": "yield",
                                    "value": "28.528"
                                },
                                {
                                    "name": "sell",
                                    "value": "36.942"
                                },
                                {
                                    "name": "buy",
                                    "value": "38.301"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190205T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "11.301 7.108 10.016 6.329"
                                },
                                {
                                    "name": "store.in",
                                    "value": "8.354 17.750 3.763 13.348"
                                },
                                {
                                    "name": "store.out",
                                    "value": "11.846 16.398 10.002 13.140"
                                },
                                {
                                    "name": "yield",
                                    "value": "8.024 6.193 3.168 11.143"
                                },
                                {
                                    "name": "sell",
                                    "value": "5.545 6.883 16.258 8.256"
                                },
                                {
                                    "name": "buy",
                                    "value": "14.538 5.344 10.638 7.781"
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
                                    "value": "68.224"
                                },
                                {
                                    "name": "store.in",
                                    "value": "40.517"
                                },
                                {
                                    "name": "store.out",
                                    "value": "60.715"
                                },
                                {
                                    "name": "yield",
                                    "value": "45.496"
                                },
                                {
                                    "name": "sell",
                                    "value": "45.411"
                                },
                                {
                                    "name": "buy",
                                    "value": "42.247"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190206T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "13.443 14.751 20.117 19.913"
                                },
                                {
                                    "name": "store.in",
                                    "value": "13.725 3.109 5.940 17.743"
                                },
                                {
                                    "name": "store.out",
                                    "value": "19.321 17.266 4.194 19.934"
                                },
                                {
                                    "name": "yield",
                                    "value": "8.407 20.087 6.748 10.254"
                                },
                                {
                                    "name": "sell",
                                    "value": "15.631 16.540 3.696 9.544"
                                },
                                {
                                    "name": "buy",
                                    "value": "14.911 6.117 15.900 5.319"
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
                                    "value": "46.725"
                                },
                                {
                                    "name": "store.in",
                                    "value": "64.594"
                                },
                                {
                                    "name": "store.out",
                                    "value": "46.555"
                                },
                                {
                                    "name": "yield",
                                    "value": "31.307"
                                },
                                {
                                    "name": "sell",
                                    "value": "54.262"
                                },
                                {
                                    "name": "buy",
                                    "value": "39.756"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190207T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "13.943 8.933 4.026 19.823"
                                },
                                {
                                    "name": "store.in",
                                    "value": "18.554 12.843 18.119 15.078"
                                },
                                {
                                    "name": "store.out",
                                    "value": "10.626 16.948 4.286 14.695"
                                },
                                {
                                    "name": "yield",
                                    "value": "11.566 2.952 3.238 13.551"
                                },
                                {
                                    "name": "sell",
                                    "value": "14.806 12.556 15.713 11.187"
                                },
                                {
                                    "name": "buy",
                                    "value": "11.834 8.605 3.494 15.823"
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
                                    "value": "69.384"
                                },
                                {
                                    "name": "store.in",
                                    "value": "67.540"
                                },
                                {
                                    "name": "store.out",
                                    "value": "49.704"
                                },
                                {
                                    "name": "yield",
                                    "value": "40.481"
                                },
                                {
                                    "name": "sell",
                                    "value": "36.202"
                                },
                                {
                                    "name": "buy",
                                    "value": "41.567"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190208T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "19.612 18.735 19.570 11.467"
                                },
                                {
                                    "name": "store.in",
                                    "value": "20.475 11.447 17.365 18.253"
                                },
                                {
                                    "name": "store.out",
                                    "value": "8.169 13.296 15.100 13.139"
                                },
                                {
                                    "name": "yield",
                                    "value": "15.815 2.797 6.438 15.431"
                                },
                                {
                                    "name": "sell",
                                    "value": "4.301 8.525 9.546 13.830"
                                },
                                {
                                    "name": "buy",
                                    "value": "7.240 6.939 16.829 10.559"
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
                                    "value": "31.102"
                                },
                                {
                                    "name": "store.in",
                                    "value": "38.079"
                                },
                                {
                                    "name": "store.out",
                                    "value": "56.589"
                                },
                                {
                                    "name": "yield",
                                    "value": "47.826"
                                },
                                {
                                    "name": "sell",
                                    "value": "42.890"
                                },
                                {
                                    "name": "buy",
                                    "value": "69.613"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190209T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "6.116 7.041 9.184 8.761"
                                },
                                {
                                    "name": "store.in",
                                    "value": "18.163 10.797 5.702 3.417"
                                },
                                {
                                    "name": "store.out",
                                    "value": "12.269 15.486 11.627 17.207"
                                },
                                {
                                    "name": "yield",
                                    "value": "10.970 20.060 3.102 13.694"
                                },
                                {
                                    "name": "sell",
                                    "value": "17.510 12.181 8.116 5.083"
                                },
                                {
                                    "name": "buy",
                                    "value": "19.157 12.966 18.085 19.405"
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
                                    "value": "47.429"
                                },
                                {
                                    "name": "store.in",
                                    "value": "63.304"
                                },
                                {
                                    "name": "store.out",
                                    "value": "40.010"
                                },
                                {
                                    "name": "yield",
                                    "value": "56.401"
                                },
                                {
                                    "name": "sell",
                                    "value": "46.043"
                                },
                                {
                                    "name": "buy",
                                    "value": "51.385"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190210T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "16.807 15.457 10.631 4.534"
                                },
                                {
                                    "name": "store.in",
                                    "value": "11.014 20.261 13.295 18.734"
                                },
                                {
                                    "name": "store.out",
                                    "value": "2.935 9.587 17.509 9.979"
                                },
                                {
                                    "name": "yield",
                                    "value": "10.746 20.649 6.218 18.788"
                                },
                                {
                                    "name": "sell",
                                    "value": "3.412 12.805 14.581 15.245"
                                },
                                {
                                    "name": "buy",
                                    "value": "14.761 16.316 5.993 14.315"
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
