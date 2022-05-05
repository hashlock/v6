---
parent_category: tests
parent_category_label: Tests

path: '{{ site.version_url_prefix_request }}/tests/{testId}'
title: 'Test Details'
type: GET

sortorder: 3
category-sortorder: 2
layout: null
---

Returns a details for a test, including test type, name, intervals, targets, alert rules and agents.  

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information


### Request

* `{testId}` the ID of the test you wish to retrieve

### Response

Sends back all metadata for the requested test.  See [Test metadata][test-metadata] page for more information on fields returned by this endpoint.


### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/tests/817.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`
  
#### Header

```HTTP/1.1 200 OK
Date: Thu, 07 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "test": [
        {
            "enabled": 1,
            "savedEvent": 0,
            "liveShare": 0,
            "testId": 817,
            "testName": "http://www.thousandeyes.com",
            "type": "http-server",
            "interval": 900,
            "url": "http://www.thousandeyes.com",
            "alertsEnabled": 1,
            "alertRules": [
                {
                    "ruleId": 302,
                    "ruleName": "BGP alert rule",
                    "expression": "((reachability < 100%) && (changes > 0))",
                    "recipient": [
                        "API Sandbox User (noreply@thousandeyes.com)"
                    ],
                    "alertType": "BGP"
                }
            ],            
            "agents": [
                {
                    "agentId": 12,
                    "agentName": "Hong Kong",
                    "location": "Hong Kong",
                    "countryId": "HK",
                    "ipAddresses": [
                        "124.248.207.183"
                    ],
                    "prefix": "124.248.207.0/24",
                    "network": "SunnyVision Limited (AS 38478)",
                    "agentType": "Cloud
                },
                ...
            ],
            "modifiedDate": "2013-05-11 02:02:21",
            "networkMeasurements": 1,
            "createdBy": "API Sandbox User (noreply@thousandeyes.com)",
            "modifiedBy": "API Sandbox User (noreply@thousandeyes.com)",
            "createdDate": "2012-06-28 19:33:12",
            "apiLinks": [...]
        }
    ]
}```

For more information on our HTTP response status codes, see the [response status codes documentation][overview-responsestatuscodes].