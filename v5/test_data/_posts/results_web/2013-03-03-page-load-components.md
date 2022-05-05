---
parent_category: test_data
parent_category_label: Test Data

path: '{{ site.version_url_prefix_request }}/web/page-load/{testId}/{agentId}/{roundId}'
title: '(Web) Page Load Component Detail'
type: GET

sortorder: 8
category-sortorder: 3
layout: null
---

Returns HAR (http archive) information, including component list and timing for elements loaded in a Page Load test. This is analogous to what is shown in the waterfall view for a Page Load test, with an agent selected.  Includes response data, dns, connect, ssl, send, wait and receive times for each component loaded in a page. 

Note: this endpoint is only available in v5 or higher of the ThousandEyes API

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information

### Request

* `{testId}` the ID of the Page Load test you for wish to retrieve data
* `{agentId}` the ID of the agent for which you wish to retrieve data
* `{roundId}` the ID of the round for which you wish to retrieve data 

### Response

Response data is returned according to the [HAR specification][link-harspec]. 

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
agentId | integer | n/a | unique ID of agent, from `/agents` endpoint
agentName | string | n/a | display name of the agent responding
countryId | string | n/a | ISO-3166-1 alpha-2 country code of the agent
date | dateTime | n/a | yyyy-MM-dd hh:mm:ss, in UTC
roundId | long | seconds | epoch time (seconds) indicating the start time of the round
permalink | url | n/a | link to jump to this result in the front end
responseTime | integer | milliseconds | time to first byte
totalSize | integer | bytes | sum of wire size of all objects on page
numObjects | integer | n/a | number of objects found on the page
numErrors | integer | n/a | number of objects which encountered errors during download
domLoadTime | integer | milliseconds | time to interaction
pageLoadTime | integer | milliseconds | time to completely load page
har | har\* | n/a | see [HAR specification][link-harspec] for details.


### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/web/page-load/2017/26/1412784300.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

#### Header

```HTTP/1.1 200 OK
Date: Thu, 08 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "web": {
        "test": {
            "enabled": 1,
            "savedEvent": 0,
            "testId": 2017,
            "testName": "Capital One with Headers",
            "type": "page-load",
            "interval": 300,
            "httpInterval": 120,
            "url": "http://www.capitalone.com",
            "networkMeasurements": 0,
            "mtuMeasurements": 0,
            "bandwidthMeasurements": 0,
            "bgpMeasurements": 0,
            "alertsEnabled": 1,
            "liveShare": 0,
            "httpTimeLimit": 5,
            "httpTargetTime": 1000,
            "pageLoadTimeLimit": 10,
            "pageLoadTargetTime": 6,
            "includeHeaders": 1,
            "sslVersionId": 0,
            "verifyCertificate": 1,
            "useNtlm": 0,
            "createdDate": "2013-09-14 00:02:14",
            "modifiedDate": "2014-10-10 21:45:48",
            "createdBy": "API Sandbox User (noreply@thousandeyes.com)",
            "modifiedBy": "API Sandbox User (noreply@thousandeyes.com)",
            "sslVersion": "Default",
            "apiLinks": [...],
        "pageLoad": [
            {
                "countryId": "US",
                "date": "2014-10-08 16:05:46",
                "har": {
                    "log": {
                        "creator": {
                            "name": "ThousandEyes API",
                            "version": ""
                        },
                        "entries": [
                            {
                                "pageref": "page_0",
                                "serverIPAddress": "23.7.199.12",
                                "startedDateTime": "2014-10-13T19:47:14.146Z",
                                "time": 9,
                                "timings": {
                                    "blocked": 0,
                                    "dns": -1,
                                    "connect": -1,
                                    "ssl": -1,
                                    "send": 0,
                                    "wait": 9,
                                    "receive": 0
                                },
                                "request": {
                                    "url": "http://www.capitalone.com/",
                                    "headers": [
                                        {
                                            "name": "Accept",
                                            "value": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8"
                                        },
                                    ],
                                    "method": "GET",
                                    "queryString": []
                                },
                                "response": {
                                    "headersSize": 174,
                                    "bodySize": 0,
                                    "status": 301,
                                    "headers": [...],
                                    "content": {
                                        "size": 0,
                                        "mimeType": ""
                                    },
                                    "statusText": "MOVED_PERMANENTLY",
                                    "redirectURL": ""
                                }
                            },
                            {
                                "pageref": "page_0",
                                "serverIPAddress": "23.7.199.12",
                                "startedDateTime": "2014-10-13T19:47:14.159Z",
                                "time": 568,
                                "timings": {
                                    "blocked": 0,
                                    "dns": -1,
                                    "connect": 40,
                                    "ssl": 5,
                                    "send": 0,
                                    "wait": 518,
                                    "receive": 10
                                },
                                "request": {
                                    "url": "https://www.capitalone.com/",
                                    "headers": [...],
                                    "method": "GET",
                                    "queryString": []
                                },
                                "response": {
                                    "headersSize": 2427,
                                    "bodySize": 19372,
                                    "status": 200,
                                    "headers": [],
                                    "content": {
                                        "size": 125357,
                                        "mimeType": "text/html"
                                    },
                                    "statusText": "OK",
                                    "redirectURL": ""
                                }
                            },
                        ],
                        "pages": [
                            {
                                "id": "page_0",
                                "title": "",
                                "startedDateTime": "2014-10-13T19:47:14.145Z",
                                "pageTimings": {
                                    "onLoad": 1782,
                                    "onContentLoad": 1294
                                }
                            }
                        ],
                        "version": "1.2"
                    }
                },
                "agentName": "San Francisco, CA - CentOS",
                "agentId": 26,
                "roundId": 1412784300,
                "responseTime": 9,
                "totalSize": 546384,
                "numObjects": 45,
                "numErrors": 0,
                "domLoadTime": 1294,
                "pageLoadTime": 1782,
                "permalink": "https://app.thousandeyes.com/web/page-load?__a=11&testId=2017&roundId=1412784300&agentId=26"
            }
        ]
    }
}```

For more information on our HTTP response status codes, see the [response status codes documentation][overview-responsestatuscodes].
