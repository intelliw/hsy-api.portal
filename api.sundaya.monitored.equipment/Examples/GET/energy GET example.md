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
                                    "value": "52.307"
                                },
                                {
                                    "name": "store.in",
                                    "value": "35.852"
                                },
                                {
                                    "name": "store.out",
                                    "value": "41.385"
                                },
                                {
                                    "name": "yield",
                                    "value": "50.372"
                                },
                                {
                                    "name": "sell",
                                    "value": "24.624"
                                },
                                {
                                    "name": "buy",
                                    "value": "53.573"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190204T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "6.385 20.495 5.739 19.688"
                                },
                                {
                                    "name": "store.in",
                                    "value": "8.217 12.994 9.136 5.505"
                                },
                                {
                                    "name": "store.out",
                                    "value": "13.581 4.271 11.502 12.031"
                                },
                                {
                                    "name": "yield",
                                    "value": "12.031 12.857 9.009 16.475"
                                },
                                {
                                    "name": "sell",
                                    "value": "7.759 4.025 5.307 7.533"
                                },
                                {
                                    "name": "buy",
                                    "value": "13.482 17.662 11.971 10.458"
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
                                    "value": "51.419"
                                },
                                {
                                    "name": "store.in",
                                    "value": "44.969"
                                },
                                {
                                    "name": "store.out",
                                    "value": "29.322"
                                },
                                {
                                    "name": "yield",
                                    "value": "50.232"
                                },
                                {
                                    "name": "sell",
                                    "value": "51.066"
                                },
                                {
                                    "name": "buy",
                                    "value": "47.447"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190205T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "20.526 13.767 5.865 11.261"
                                },
                                {
                                    "name": "store.in",
                                    "value": "16.533 10.643 12.882 4.911"
                                },
                                {
                                    "name": "store.out",
                                    "value": "12.385 4.345 7.909 4.683"
                                },
                                {
                                    "name": "yield",
                                    "value": "4.928 10.524 19.518 15.262"
                                },
                                {
                                    "name": "sell",
                                    "value": "15.651 3.688 14.282 17.445"
                                },
                                {
                                    "name": "buy",
                                    "value": "18.707 5.450 16.563 6.727"
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
                                    "value": "37.000"
                                },
                                {
                                    "name": "store.in",
                                    "value": "68.117"
                                },
                                {
                                    "name": "store.out",
                                    "value": "78.498"
                                },
                                {
                                    "name": "yield",
                                    "value": "50.505"
                                },
                                {
                                    "name": "sell",
                                    "value": "52.912"
                                },
                                {
                                    "name": "buy",
                                    "value": "17.083"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190206T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "12.769 9.593 8.721 5.917"
                                },
                                {
                                    "name": "store.in",
                                    "value": "17.256 18.469 15.958 16.434"
                                },
                                {
                                    "name": "store.out",
                                    "value": "20.375 18.755 19.838 19.530"
                                },
                                {
                                    "name": "yield",
                                    "value": "17.502 19.163 8.666 5.174"
                                },
                                {
                                    "name": "sell",
                                    "value": "16.237 12.672 4.581 19.422"
                                },
                                {
                                    "name": "buy",
                                    "value": "5.416 3.380 3.721 4.566"
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
                                    "value": "45.865"
                                },
                                {
                                    "name": "store.in",
                                    "value": "47.014"
                                },
                                {
                                    "name": "store.out",
                                    "value": "37.836"
                                },
                                {
                                    "name": "yield",
                                    "value": "42.859"
                                },
                                {
                                    "name": "sell",
                                    "value": "37.967"
                                },
                                {
                                    "name": "buy",
                                    "value": "28.587"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190207T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "5.228 12.303 20.375 7.959"
                                },
                                {
                                    "name": "store.in",
                                    "value": "13.955 13.255 5.175 14.629"
                                },
                                {
                                    "name": "store.out",
                                    "value": "17.626 4.747 5.158 10.305"
                                },
                                {
                                    "name": "yield",
                                    "value": "8.499 11.118 17.962 5.280"
                                },
                                {
                                    "name": "sell",
                                    "value": "16.565 7.751 8.907 4.744"
                                },
                                {
                                    "name": "buy",
                                    "value": "3.407 4.872 13.239 7.069"
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
                                    "value": "47.994"
                                },
                                {
                                    "name": "store.in",
                                    "value": "43.345"
                                },
                                {
                                    "name": "store.out",
                                    "value": "60.493"
                                },
                                {
                                    "name": "yield",
                                    "value": "46.153"
                                },
                                {
                                    "name": "sell",
                                    "value": "55.542"
                                },
                                {
                                    "name": "buy",
                                    "value": "44.309"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190208T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "15.010 7.254 16.897 8.833"
                                },
                                {
                                    "name": "store.in",
                                    "value": "6.048 19.906 10.837 6.554"
                                },
                                {
                                    "name": "store.out",
                                    "value": "19.648 4.608 18.727 17.510"
                                },
                                {
                                    "name": "yield",
                                    "value": "13.056 10.297 17.600 5.200"
                                },
                                {
                                    "name": "sell",
                                    "value": "20.304 14.961 4.987 15.290"
                                },
                                {
                                    "name": "buy",
                                    "value": "14.345 14.768 12.117 3.079"
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
                                    "value": "74.694"
                                },
                                {
                                    "name": "store.in",
                                    "value": "44.831"
                                },
                                {
                                    "name": "store.out",
                                    "value": "36.197"
                                },
                                {
                                    "name": "yield",
                                    "value": "35.908"
                                },
                                {
                                    "name": "sell",
                                    "value": "35.712"
                                },
                                {
                                    "name": "buy",
                                    "value": "44.980"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190209T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "18.546 19.182 18.249 18.717"
                                },
                                {
                                    "name": "store.in",
                                    "value": "13.000 12.251 15.579 4.001"
                                },
                                {
                                    "name": "store.out",
                                    "value": "11.471 6.318 3.935 14.473"
                                },
                                {
                                    "name": "yield",
                                    "value": "7.248 6.481 6.523 15.656"
                                },
                                {
                                    "name": "sell",
                                    "value": "15.051 14.603 3.090 2.968"
                                },
                                {
                                    "name": "buy",
                                    "value": "3.988 16.662 8.244 16.086"
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
                                    "value": "53.881"
                                },
                                {
                                    "name": "store.in",
                                    "value": "48.717"
                                },
                                {
                                    "name": "store.out",
                                    "value": "55.418"
                                },
                                {
                                    "name": "yield",
                                    "value": "62.997"
                                },
                                {
                                    "name": "sell",
                                    "value": "48.372"
                                },
                                {
                                    "name": "buy",
                                    "value": "35.700"
                                }
                            ]
                        },
                        {
                            "name": "day.timeofday",
                            "value": "20190210T0000/4",
                            "data": [
                                {
                                    "name": "harvest",
                                    "value": "12.351 18.994 16.019 6.517"
                                },
                                {
                                    "name": "store.in",
                                    "value": "18.852 6.854 2.774 20.237"
                                },
                                {
                                    "name": "store.out",
                                    "value": "18.725 17.005 12.991 6.697"
                                },
                                {
                                    "name": "yield",
                                    "value": "16.763 14.488 15.289 16.457"
                                },
                                {
                                    "name": "sell",
                                    "value": "18.492 14.514 11.864 3.502"
                                },
                                {
                                    "name": "buy",
                                    "value": "13.061 9.151 6.185 7.303"
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
