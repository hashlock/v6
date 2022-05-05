---
parent_category: labels
parent_category_label: Labels

path: '{{ site.version_url_prefix_request }}/groups/{groupId}'
title: 'Label details'
type: GET

sortorder: 3
category-sortorder: 7
layout: null
---

Returns details for a label (formerly called group) configured in ThousandEyes.  

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={aid}` optional and requires the user to be assigned to the target account group, specifies the account group context of the request, obtained from the `/account-groups` endpoint.  Specifying this parameter without the user being assigned to the target account will result in an error response. See [Account group context][overview-accountcontext] for more information

### Request

* `{groupId}` the ID of the label to retrieve
* no request body

### Response

Returns the label details.  Metadata of the response is returned according to the following table:

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
name | string | n/a | Name of the label
groupId | integer | n/a | Unique ID of the label; this number is negative for built-in labels.  Query the `/groups/{id}` endpoint to see a list of agents/tests with this label.
type | string | n/a | either `tests` or `agents`, indicates the type of label.
builtin | integer | n/a | 1 for built-in labels, and 0 for user-created labels.  Note that built-in labels are read-only.
agents | array of agent objects | n/a | Agent objects are shown for an agents type label  only.  See [Agent Metadata][agent-metadata] for more details on agent objects.
tests | array of test objects | n/a | Test objects are shown for a `tests` type label only.  See [Test Metadata][test-metadata] for more detail on test objects.

### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/groups/-2.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

#### Header

```HTTP/1.1 200 OK
Server: nginx
Date: Tue, 15 Mar 2016 00:20:26 GMT
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Strict-Transport-Security: max-age=31536000
X-Server-Name: 1-5```

#### Body

```{
    "groups": [
        {
            "name": "Cloud",
            "groupId": -2,
            "type": "agents",
            "builtin": 1,
            "agents": [
                {
                    "agentId": 6,
                    "agentName": "Fremont, CA (v6)",
                    "location": "San Francisco Bay Area",
                    "countryId": "US",
                    "ipAddresses": [
                        "2600:3c01::f03c:91ff:feae:4f96"
                    ],
                    "agentType": "Cloud"
                },
                {
                    "agentId": 11,
                    "agentName": "London, UK",
                    "location": "City of London, United Kingdom",
                    "countryId": "GB",
                    "ipAddresses": [
                        "176.58.99.46",
                        "178.79.138.106"
                    ],
                    "agentType": "Cloud"
                },
                {
                    "agentId": 19,
                    "agentName": "Amsterdam, Netherlands",
                    "location": "Netherlands",
                    "countryId": "NL",
                    "ipAddresses": [
                        "95.85.55.177"
                    ],
                    "agentType": "Cloud"
                }
            ]
        }
    ]
}```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
