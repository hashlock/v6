---
parent_category: test_data
parent_category_label: Test Data

path: '{{ site.version_url_prefix_request }}/net/path-vis/{testId}/{agentId}/{roundId}'
title: '(Network) Detailed Path Trace'
type: GET

sortorder: 3
category-sortorder: 3
layout: null
---

Returns a hop-by-hop summary of the path trace data collected during path visualization.   In each path visualization attempt, three attempts are made to reach the destination, and the entire path will be shown in sequence.  A **time frame** must be specified, or the current round of data will be returned.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information

### Request

* `{testId}` the ID of the test you wish to retrieve
* `{agentId}` the ID of the agent from which you wish to obtain data
* `{roundId}` the round ID for which you wish to obtain data.  Equals the beginning of the testing round, in epoch time format.

### Response

* Each route should start with a hop of 1
* Where a hop number is missing from response data, this is an indication that a star (\*) response was returned in the path trace attempt for that hop.

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
agentId | integer | n/a | unique ID of agent, from `/agents` endpoint
agentName | string | n/a | display name of the agent responding
countryId | string | n/a | ISO-3166-1 alpha-2 country code of the agent
date | dateTime | n/a | yyyy-MM-dd hh:mm:ss, in UTC
roundId | long | seconds | epoch time (seconds) indicating the start time of the round
permalink | url | n/a | link to jump to this result in the front end
server | url | n/a | target server, including port (if method used is TCP)
serverIp | string | n/a | ip address of target server
sourceIp | string | n/a | ip address of source agent
sourcePrefix | string | n/a | ip prefix of source agent
routes | array | n/a | shows 3 iterations of path trace, with each iteration specified by a pathId
routes.pathId | string | n/a | unique ID of path trace
routes.hops | array | n/a | array of hop objects indicating each step in the traceroute
routes.hops.hop | integer | n/a | index of hop
routes.hops.ipAddress | string | n/a | IP address of the hop
routes.hop.prefix | string | n/a | Prefix of IP address shown in CIDR
routes.hop.rdns | string | n/a | reverse DNS entry of IP, if available
routes.hop.network | string | n/a | Autonomous System originating the prefix
routes.hop.responseTime | integer | milliseconds | RTT to the hop's IP
routes.hop.location | string | n/a | location information for the hop
routes.hop.mpls | string | n/a | Multiprotocol Label Switching information, if available


### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/net/path-vis/817/146/1383887700.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

#### Header

```HTTP/1.1 200 OK
Date: Thu, 07 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "net": {
        "test": {
            "enabled": 1,
            "testId": 817,
            "testName": "http://www.thousandeyes.com",
            "type": "http-server",
            "interval": 900,
            "url": "http://www.thousandeyes.com",
            "modifiedDate": "2013-05-11 02:02:21",
            "networkMeasurements": 1,
            "createdBy": "API Sandbox User (noreply@thousandeyes.com)",
            "modifiedBy": "API Sandbox User (noreply@thousandeyes.com)",
            "createdDate": "2012-06-28 19:33:12",
            "apiLinks": [...]
        },
        "pathVis": [
            {
                "agentName": "San Jose, CA",
                "countryId": "US",
                "date": "2013-11-08 05:15:12",
                "server": "www.thousandeyes.com:80",
                "serverIp": "50.18.127.223",
                "sourceIp": "198.89.98.67",
                "sourcePrefix": "198.89.96.0/19",
                "routes": [
                    {
                        "pathId": "6535224890535563750337719341569",
                        "hops": [
                            {
                                "hop": 4,
                                "ipAddress": "154.54.84.53",
                                "prefix": "154.48.0.0/12",
                                "rdns": "te0-2-0-4.ccr21.sjc01.atlas.cogentco.com",
                                "network": "Cogent Communications (AS 174)",
                                "responseTime": 2,
                                "location": "San Francisco Area"
                            },
                            {
                                "hop": 5,
                                "ipAddress": "154.54.6.106",
                                "prefix": "154.48.0.0/12",
                                "rdns": "be2000.ccr21.sjc03.atlas.cogentco.com",
                                "network": "Cogent Communications (AS 174)",
                                "responseTime": 1,
                                "location": "San Francisco Area"
                            },
                            {
                                "hop": 6,
                                "ipAddress": "154.54.13.126",
                                "prefix": "154.48.0.0/12",
                                "rdns": "verio.sjc03.atlas.cogentco.com",
                                "network": "Cogent Communications (AS 174)",
                                "responseTime": 1,
                                "location": "San Francisco Area"
                            },
                            {
                                "hop": 7,
                                "ipAddress": "129.250.4.118",
                                "prefix": "129.250.0.0/16",
                                "rdns": "ae-4.r06.plalca01.us.bb.gin.ntt.net",
                                "network": "NTT America, Inc. (AS 2914)",
                                "responseTime": 2,
                                "location": "San Francisco Area",
                                "mpls": "L=399088,E=0,S=1,T=1"
                            },
                            {
                                "hop": 8,
                                "ipAddress": "140.174.21.182",
                                "prefix": "140.174.0.0/16",
                                "rdns": "ae-1.amazon.plalca01.us.bb.gin.ntt.net",
                                "network": "NTT America, Inc. (AS 2914)",
                                "responseTime": 2,
                                "location": "San Francisco Area"
                            },
                            {
                                "hop": 9,
                                "ipAddress": "205.251.229.46",
                                "prefix": "205.251.228.0/22",
                                "network": "Amazon.com, Inc. (AS 16509)",
                                "responseTime": 4,
                                "location": "Seattle Area"
                            },
                            {
                                "hop": 10,
                                "ipAddress": "72.21.222.19",
                                "prefix": "72.21.220.0/22",
                                "network": "Amazon.com, Inc. (AS 16509)",
                                "responseTime": 3,
                                "location": "Seattle Area"
                            },
                            {
                                "hop": 11,
                                "ipAddress": "216.182.236.109",
                                "prefix": "216.182.232.0/21",
                                "network": "Amazon.com, Inc. (AS 14618)",
                                "responseTime": 4,
                                "location": "Seattle Area"
                            },
                            {
                                "hop": 16,
                                "ipAddress": "50.18.127.223",
                                "prefix": "50.18.64.0/18",
                                "rdns": "ec2-50-18-127-223.us-west-1.compute.amazonaws.com",
                                "network": "Amazon.com, Inc. (AS 16509)",
                                "responseTime": 4,
                                "location": "San Francisco Area"
                            }
                        ]
                    },
                    {
                        "pathId": "6535224890535563750337719341568",
                        "hops": [...]
                    },
                    {
                        "pathId": "6535224890535563750337719341570",
                        "hops": [...]
                    }
                ],
                "permalink": "https://app.thousandeyes.com/net/path-vis?__a=75&testId=817&roundId=1383887700&serverId=71&agentId=146",
                "agentId": 146,
                "roundId": 1383887700
            }
        ]
    }
}```

For more information on our HTTP response status codes, see the [response status codes documentation][overview-responsestatuscodes].
