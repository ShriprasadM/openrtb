### Ad Pod Exclusions

Sponsers: PubMatic

## Overview
Category exclusion and advertiser exclusion are one of the crucial features when it comes to building an ad pod

In oRTB 2.6, IAB have already introduced the ad pod parameters. Using those parameters one can request for structured, dynamic and hybrid type of ad pods

This document extends the same context and introducing the new set of parameters, which can be used for specifying category as well as advertiser exclusion requirements to your exchage

## Requested Changes
Addition to **AdCOM v1.0 Restrictions** Object

## Assumptions
Mentioned attributes only applicable if item/impreesion is an ad pod or slot in adpod

### Object:<code>exclcat</code>
<table>
  <tr>
    <td><strong>Attribute&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Description</strong></td>
  </tr>
  <tr>
    <td><code>exclcat</code></td>
    <td>string array</td>
    <td>Ensures no ads, with same ad category in the given list are served toghether within a dynamic/dynamic portion of hybrid pod. default is empty list, indicating ads with same category can be served together.
    In case of structured ad pod, no ad, which belongs to mentioned list of ad catgories will be serve for the slot. 
    Ad category must follow <a href="https://github.com/InteractiveAdvertisingBureau/AdCOM/blob/main/AdCOM%20v1.0%20FINAL.md#list_categorytaxonomies">taxonomy</a>.
    e.g. ["IAB-1", "IAB-2"]</td>
  </tr>
</table>

### Object:<code>excladv</code>
<table>
  <tr>
    <td><strong>Attribute&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Description</strong></td>
  </tr>
  <tr>
    <td><code>excladv</code></td>
    <td>integer</td>
    <td>Ensures no ads, with same domain in the given list are served toghether within a dynamic/dynamic portion of hybrid pod. default is empty list, indicating ads with same domain can be served together.
    In case of structured ad pod, no ad, which belongs to mentioned list of ad domains will be serve for the slot. e.g. ["ford.com", "maruti.com"]</td>
  </tr>
</table>

#### Ad Pod Examples

##### _Dynamic Ad Pod Request_

Request to build a dynamic ad pod where, no ads with same `Business and Finance (45)` and `Careers(123)` categories should be served. Also no ads with same domain `ford.com` and `maruti.com` should be served.

```json
{
    "item": [
        {
            "id": "item_1",
            "spec": {
                "video": {
                    "podid": "dynamic_pod",
                    "poddur": 90,
                    "maxseq": 3,
                    "rqddurs": [
                        30,
                        45,
                        60
                    ]
                },
                "restrictions": {
                    "exclcat": [
                        "52"
                    ],
                    "excladv": [
                        "ford.com"
                    ]
                }
            }
        }
    ]
}
```

##### _Structured Ad Pod Request_

Request to build a structured ad pod where,
1st ad should not be from `Business and Finance (45)` and `ford.com`
2nd ad can be of `any ad category` and from `any domain`
3rd ad should not be from `Careers(123)`  category but can be from `any domain`


```json
{
    "item": [
        {
            "id": "item_1",
            "spec": {
                "video": {
                    "podid": "structured_pod",
                    "minduration": 5,
                    "maxduration": 10,
                    "restrictions": {
                        "exclcat": [
                            "52"
                        ],
                        "excladv": [
                            "ford.com"
                        ]
                    }
                }
            }
        },
        {
            "id": "item_2",
            "spec": {
                "video": {
                    "podid": "structured_pod",
                    "minduration": 10,
                    "maxduration": 15,
                    "restrictions": {
                        "exclcat": [],
                        "excladv": []
                    }
                }
            }
        },
        {
            "id": "item_1",
            "spec": {
                "video": {
                    "podid": "structured_pod",
                    "minduration": 15,
                    "maxduration": 30,
                    "restrictions": {
                        "exclcat": [
                            "123"
                        ],
                        "excladv": []
                    }
                }
            }
        }
    ]
}
```

##### _Hybrid Ad Pod Request_

Request to build a hybrid ad pod where,
1st ad should not be from `Business and Finance (45)` and `ford.com`
dynamic portion no ads with same `Business and Finance (45)` categories should be served. Also no ads with same domain `maruti.com` should be served.

```json
{
    "item": [
        {
            "id": "item_1",
            "spec": {
                "video": {
                    "podid": "hybrid_pod",
                    "minduration": 5,
                    "maxduration": 10
                },
                "restrictions": {
                    "exclcat": [
                        "52"
                    ],
                    "excladv": [
                        "ford.com"
                    ]
                }
            }
        },
        {
            "id": "item_2",
            "spec": {
                "video": {
                    "podid": "hybrid_pod",
                    "poddur": 90,
                    "maxseq": 3,
                    "rqddurs": [
                        30,
                        45,
                        60
                    ]
                },
                "restrictions": {
                    "exclcat": [
                        "123"
                    ],
                    "excladv": [
                        "maruti.com"
                    ]
                }
            }
        }
    ]
}
```

##### _Cross Ad Pod Exclusion_
Request to build 2 dynamic ad pods where multiple ads should not be from `Business and Finance (45)` category and `ford.com` domain

```json
{
    "item": [
        {
            "id": "item_1",
            "spec": {
                "video": {
                    "podid": "dynamic_pod_1",
                    "poddur": 90,
                    "maxseq": 3,
                    "rqddurs": [
                        30,
                        45,
                        60
                    ]
                }
            }
        },
        {
            "id": "item_2",
            "spec": {
                "video": {
                    "podid": "dynamic_pod_2",
                    "poddur": 60,
                    "maxseq": 2,
                    "rqddurs": [
                        15,
                        30,
                        45,
                        60
                    ]
                }
            }
        }
    ],
    "context": {
        "restrictions": {
            "exclcat": [
                "45"
            ],
            "excladv": [
                "ford.com"
            ]
        }
    }
}

```