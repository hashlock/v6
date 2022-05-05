---
parent_category: test_data
parent_category_label: Test Data

path: '{{ site.version_url_prefix_request }}/web/transactions/{testId}/{agentId}/{roundId}/{pageNum}'
title: '(Web) Transaction Component Detail'
type: GET

sortorder: 11
category-sortorder: 3
layout: null
---

Returns HAR (http archive) information, including component list and timing for elements loaded in a transaction test.  This is analogous to what is shown in the waterfall view for a page load or transaction test, with an agent selected.  Includes response data, dns, connect, ssl, send, wait and receive times for each component loaded in a page. 

Note: this endpoint is only available in v5 or higher of the ThousandEyes API.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information

### Request

* `{testId}` the ID of the Transaction test you for wish to retrieve data
* `{agentId}` the ID of the agent for which you wish to retrieve data
* `{roundId}` the ID of the round for which you wish to retrieve data 
* `{pageNum}` the page number for the page reached in a transaction.  Can be obtained from `/web/transactions/{testId}/{agentId}/{roundId}` endpoint.  Page numbers are zero-indexed.

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
stepsCompleted | integer | n/a | number of steps successfully completed
totalSteps | integer | n/a | total number of steps in the transaction script
transactionTime | integer | milliseconds | elapsed execution time between beginning of start step and end of end step in the transaction
har | har\* | n/a | see [HAR specification][link-harspec] for details.


### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/web/transactions/2017/26/1412784300/0.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

#### Header

```HTTP/1.1 200 OK
Date: Thu, 08 Oct 2014 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "web": {
        "test": {
            "enabled": 1,
            "savedEvent": 0,
            "testId": 7600,
            "testName": "App Login - Edited using the API and renamed from API",
            "type": "transactions",
            "interval": 1800,
            "url": "https://app.thousandeyes.com",
            "totalSteps": 12,
            "alertsEnabled": 1,
            "liveShare": 0,
            "timeLimit": 30,
            "targetTime": 20000,
            "includeHeaders": 1,
            "createdDate": "2014-07-22 18:44:00",
            "modifiedDate": "2014-10-13 18:17:52",
            "createdBy": "API Sandbox User (noreply@thousandeyes.com)",
            "modifiedBy": "API Sandbox User (noreply@thousandeyes.com)",
            "apiLinks": [...],
        },
        "transaction": [
            {
                "countryId": "CA",
                "date": "2014-10-13 20:00:06",
                "stepsCompleted": 12,
                "totalSteps": 12,
                "componentErrors": 0,
                "har": {
                    "log": {
                        "creator": {
                            "name": "ThousandEyes DB Exporter",
                            "version": ""
                        },
                        "entries": [
                            {
                                "pageref": "page_1",
                                "serverIPAddress": "208.185.7.120",
                                "startedDateTime": "2014-10-13T20:22:23.727Z",
                                "time": 62,
                                "timings": {
                                    "blocked": 0,
                                    "dns": -1,
                                    "connect": -1,
                                    "ssl": -1,
                                    "send": 0,
                                    "wait": 38,
                                    "receive": 24
                                },
                                "request": {
                                    "url": "https://****:****@app.thousandeyes.com/login?fwd=%2Fdashboard",
                                    "headers": [
                                        {
                                            "name": "Accept",
                                            "value": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8"
                                        },
                                        ...
                                    ],
                                    "method": "POST",
                                    "queryString": [
                                        {
                                            "name": "fwd",
                                            "value": "/dashboard"
                                        }
                                    ]
                                },
                                "response": {
                                    "headersSize": 347,
                                    "bodySize": 3438,
                                    "status": 200,
                                    "headers": [...],
                                    "content": {
                                        "size": 9306,
                                        "mimeType": "text/html"
                                    },
                                    "statusText": "OK",
                                    "redirectURL": ""
                                }
                            },
                            ...
                        ],
                        "pages": [
                            {
                                "id": "page_1",
                                "title": "Log In - ThousandEyes",
                                "startedDateTime": "2014-10-13T20:22:23.727Z",
                                "pageTimings": {}
                            }
                        ],
                        "version": "1.2"
                    }
                },
                "steps": [
                    {
                        "stepNum": 0,
                        "pageNum": 0,
                        "duration": 826,
                        "offset": 0
                    },
                    ...
                ],
                "pages": [
                    {
                        "pageNum": 1,
                        "pageName": "Log In - ThousandEyes",
                        "componentCount": 8,
                        "errorCount": 0,
                        "duration": 205
                    }
                ],
                "agentName": "Vancouver, Canada",
                "agentId": 107,
                "roundId": 1413230400,
                "transactionTime": 5316,
                "permalink": "https://app.thousandeyes.com/web/transactions?__a=11&testId=7600&roundId=1413230400&agentId=107"
            }
        ]
    }
}
```

For more information on our HTTP response status codes, see the [response status codes documentation][overview-responsestatuscodes].
