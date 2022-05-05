---
parent_category: agents
parent_category_label: Agents & Monitors

path: '{{ site.version_url_prefix_request }}/bgp-monitors'
title: 'BGP Monitor List'
type: GET

sortorder: 5
category-sortorder: 5
layout: null
---

Returns a list of all BGP monitors available to your account in ThousandEyes, including both public and private feeds.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user being assigned to the target account will result in an error response. See [Account group context][overview-accountcontext] for more information

### Request

* no request body

### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/bgp-monitors.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

Sends back an array of BGP monitors, including monitorId, which can be used by other areas of the API.  The example below shows both a public and private BGP monitor.

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
monitorId | integer | n/a | unique ID of BGP monitor
ipAddress | string | n/a | IP address of the BGP monitor
network | string | n/a | name of the autonomous system in which the monitor is found
monitorType | string | n/a | either Public or Private, shows the type of monitor
monitorName | string | n/a | display name of the BGP monitor

#### Header

```HTTP/1.1 200 OK
Date: Thu, 07 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "bgpMonitors": [
        {
            "monitorId": 1,
            "ipAddress": "4.69.184.193",
            "network": "Level 3 Communications, Inc. (AS 3356)",
            "monitorType": "Public",
            "monitorName": "Seattle, WA"
        },
        ...,
        {
            "monitorId": 76,
            "ipAddress": "166.78.186.62",
            "network": "AS 65314",
            "monitorType": "Private",
            "monitorName": "Private Test Peer - ThousandEyes (bgp-peering-dev, AS 65314)"
        }
    ]
}```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
