---
parent_category: test_data
parent_category_label: Test Data

path: '{{ site.version_url_prefix_request }}/web/http-server/{testId}'
title: '(Web) HTTP Server'
type: GET

sortorder: 6
category-sortorder: 3
layout: null
---

Returns response code, response and fetch times, as well as component-level (DNS, Connect, Wait, Receive and Fetch) timing for the load of an object over HTTP.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `window=[0-9]+[smhdw]?` specifies a window of time for the result set.  See [Time Ranges][overview-timerange] for more information.
* `from=YYYY-mm-ddTHH:MM:SS&to=YYYY-mm-ddTHH:MM:SS` specifies an explicit start (and optionally, end) for your range of data.  See [Time Ranges][overview-timerange] for more information.
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information
* `headers=1` optional, will output request and response header information in output.  Example below assumes headers=1 was specified as an optional parameter.

### Request

* `{testId}` the ID of the HTTP Server (or page load) test you wish to retrieve

### Response

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
agentId | integer | n/a | unique ID of agent, from `/agents` endpoint
agentName | string | n/a | display name of the agent responding
countryId | string | n/a | ISO-3166-1 alpha-2 country code of the agent
date | dateTime | n/a | yyyy-MM-dd hh:mm:ss, in UTC
roundId | long | seconds | epoch time (seconds) indicating the start time of the round
permalink | url | n/a | link to jump to this result in the front end
serverIp | string | n/a | IP address of destination server
responseCode | integer | n/a | code of HTTP response
numRedirects | integer | n/a | number of redirects
redirectTime | integer | milliseconds | cumulative redirect timing
dnsTime | integer | milliseconds | time required to resolve DNS 
sslTime | integer | milliseconds | time to negotiate SSL/TLS
connectTime | integer | milliseconds | time required to establish a TCP connection to the server
waitTime | integer | milliseconds | time elapsed between completion of request and first byte of response
receiveTime | integer | milliseconds | elapsed time between first and last byte of response
wireSize | float | bytes | size of content, in bytes
responseTime | integer | milliseconds | time to first byte
fetchTime | integer | milliseconds | response time + receive time
requestHeaders | string | n/a | crlf-delimited list of request headers in header: value format
reponseHeaders | string | n/a | crlf-delimited list of response headers in header: value format
errorType | string | n/a | type of error encountered; corresponds to phase of connection
errorDetails | string | n/a | error details, if an error were encountered


### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/web/http-server/817.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Header

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
            "testId": 817,
            "testName": "http://www.thousandeyes.com",
            "type": "http-server",
            "interval": 900,
            "httpTimeLimit": 5,
            "url": "http://www.thousandeyes.com",
            "modifiedDate": "2013-05-11 02:02:21",
            "networkMeasurements": 1,
            "createdBy": "API Sandbox User (noreply@thousandeyes.com)",
            "modifiedBy": "API Sandbox User (noreply@thousandeyes.com)",
            "createdDate": "2012-06-28 19:33:12",
            "apiLinks": [...]
        },
        "httpServer": [
            {
                "agentName": "Hong Kong",
                "countryId": "HK",
                "date": "2013-11-13 04:32:40",
                "serverIp": "50.18.127.223",
                "responseCode": 200,
                "numRedirects": 1,
                "errorType": "None",
                "redirectTime": 42,
                "dnsTime": 0,
                "connectTime": 167,
                "waitTime": 598,
                "receiveTime": 335,
                "wireSize": 29931,
                "permalink": "https://app.thousandeyes.com/web/basic-http?__a=75&testId=817&roundId=1384317000&agentId=12",
                "agentId": 12,
                "errorDetails": "",
                "responseTime": 766,
                "roundId": 1384317000,
                "fetchTime": 1101,
                "requestHeaders": "GET /?gfe_rd=ctrl&ei=bHMoU-ukCqO00QXGr4CQAg&gws_rd=cr HTTP/1.1\r\nUser-Agent: Mozilla/5.0 AppleWebKit/999.0 (KHTML, like Gecko) Chrome/99.0 Safari/999.0\r\nHost: www.google.co.uk\r\nAccept: */*\r\n",
                "responseHeaders": "HTTP/1.1 200 OK\r\nDate: Tue, 18 Mar 2014 16:25:16 GMT\r\nExpires: -1\r\nCache-Control: private, max-age=300\r\nContent-Type: application/xhtml+xml; charset=UTF-8\r\nP3P: CP=\"This is not a P3P policy! See http://www.google.com/support/accounts/bin/answer.py?hl=en&answer=151657 for more info.\"\r\nServer: gws\r\nX-XSS-Protection: 1; mode=block\r\nX-Frame-Options: SAMEORIGIN\r\nAlternate-Protocol: 443:quic\r\nTransfer-Encoding: chunked\r\n"
            },
            ...
        ]
    },
    "pages": {
        "current": 1
    }
}```

For more information on our HTTP response status codes, see the [response status codes documentation][overview-responsestatuscodes].
