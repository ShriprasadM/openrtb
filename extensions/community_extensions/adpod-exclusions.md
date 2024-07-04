### Ad Pod Exclusions

Sponsers: PubMatic

## Overview
Category exclusion and advertiser exclusion are one of the crucial features when it comes to building an ad pod

In oRTB 2.6, IAB have already introduced the ad pod parameters. Using those parameters one can request for structured, dynamic and hybrid type of ad pods

This document extends the same context and introducing the new set of parameters, which can be used for specifying category as well as advertiser exclusion requirements to your exchage

## Requested Changes
Addition to **Impression.Video** Object

## Assumptions
Mentioned attributes only applicable if impreesions is an ad pod or slot in adpod

### Object:<code>exclcat</code>
<table>
  <tr>
    <td><strong>Attribute&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Description</strong></td>
  </tr>
  <tr>
    <td><code>exclcat</code></td>
    <td>integer</td>
    <td>Ensures no ads, which belongs to same ad category are served within a pod, if value is <code>1</code>. Default value is <code>0</code>, indicating ads with same category can be served together. ad category must follow <a href="https://github.com/InteractiveAdvertisingBureau/AdCOM/blob/main/AdCOM%20v1.0%20FINAL.md#list_categorytaxonomies">taxonomy</a></td>
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
    <td>Ensures no ads, which belongs to same advertiser domain (e.g., “ford.com”). are served within a pod, if value is <code>1</code>. Default value is <code>0</code>, indicating ads with same domain can be served together.</td>
  </tr>
</table>

#### Ad Pod Examples

##### _Dynamic Ad Pod Request_

Request to build a dynamic ad pod where, no same category ads nor ads from same advertiser domains are allowed

```json
{
    "imp" : [{
        "video" : {
            "podid": "dynamic_pod",
            "poddur" : 90,
            "maxseq" : 3,
            "rqddurs" : [30,45,60],
            "exclcat" : 1,
            "excladv" : 1
        }
    }]
}
```

##### _Structured Ad Pod Request_
```json
{
    "imp" : [{
        "video" : {
            "podid" : "structured_pod",
            "minduration" : 5,
            "maxduration": 10,
            "exclcat" : 1,
            "excladv" : 1
        }
    },{
        "video" : {
            "podid" : "structured_pod",
            "minduration" : 10,
            "maxduration": 15,
            "exclcat" : 1,
            "excladv" : 1
        }
    },{
        "video" : {
            "podid" : "structured_pod",
            "minduration" : 15,
            "maxduration": 30,
            "exclcat" : 1,
            "excladv" : 1
        }
    }]
}
```