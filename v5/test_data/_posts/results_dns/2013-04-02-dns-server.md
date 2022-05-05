---
parent_category: test_data
parent_category_label: Test Data

path: '{{ site.version_url_prefix_request }}/dns/server/{testId}/{serverId}'
title: '(DNS) Server Metrics'
type: GET

sortorder: 14
category-sortorder: 3
layout: null
---

{.inline-code} Returns the mappings for a DNS record, along with resolution time to each authoritative server, as measured from the vantage point of the agent.  Similar to `dig @server`.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `window=[0-9]+[smhdw]?` specifies a window of time for the result set.  See [Time Ranges][overview-timerange] for more information.
* `from=YYYY-mm-ddTHH:MM:SS&to=YYYY-mm-ddTHH:MM:SS` specifies an explicit start (and optionally, end) for your range of data.  See [Time Ranges][overview-timerange] for more information.
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information

### Request

* `{testId}` the ID of the DNS server test you wish to retrieve

### Response

The DNS Server tests are somewhat unique, in that you are testing multiple targets with a single test configuration.  For example - if you're testing com. NS, you'll actually end up with 13 tests, since the test should be targeting 13 different nameservers \[a-m\].gtld-servers.net.  This test tests resolution time to each of the authoritative nameservers from each agent, ending up with something of a matrix of test results for each iteration.

{.inline-code} For tests with Network Measurements enabled (`"networkMeasurements": 1`), you'll need the serverId identified in the test to pull network metrics for your DNS server test.  Syntax for DNS Server network metrics [see End-to-End Metrics][results-net-metrics] is as follows:

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/dns/server/{testId}/{serverId}.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
agentId | integer | n/a | unique ID of agent, from `/agents` endpoint
agentName | string | n/a | display name of the agent responding
countryId | string | n/a | ISO-3166-1 alpha-2 country code of the agent
date | dateTime | n/a | yyyy-MM-dd hh:mm:ss, in UTC
roundId | long | seconds | epoch time (seconds) indicating the start time of the round
permalink | url | n/a | link to jump to this result in the front end
serverId | integer | n/a | internal ID of DNS server being tested
server | string | n/a | canonical name of server being tested
resolutionTime | integer | milliseconds | how long it took to run the query against the server
errorDetails | string | n/a | if an error was encountered, error text
queries | integer | n/a | how many queries were reuqired to get to the requested result
mappings | string | n/a | final mappings returned from the request


### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/dns/server/821.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

#### Header

```HTTP/1.1 200 OK
Date: Thu, 08 Nov 2013 07:32:48 GMT
Server: Apache/2.2.22 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json```

#### Body

```{
    "dns": {
        "test": {
            "enabled": 1,
            "testId": 821,
            "testName": "thousandeyes.com ANY",
            "type": "dns-server",
            "interval": 900,
            "domain": "thousandeyes.com ANY",
            "dnsServers": [
                {
                    "serverId": 465,
                    "serverName": "a1.verisigndns.com."
                }
            ],
            "modifiedDate": "2013-05-11 02:02:16",
            "networkMeasurements": 1,
            "createdBy": "API Sandbox User (noreply@thousandeyes.com)",
            "modifiedBy": "API Sandbox User (noreply@thousandeyes.com)",
            "createdDate": "2012-06-28 20:47:29",
            "apiLinks": [...]
        },
        "server": [
            {
                "agentName": "Hong Kong",
                "countryId": "HK",
                "date": "2013-11-13 04:30:20",
                "permalink": "https://app.thousandeyes.com/dns/server?__a=75&testId=821&roundId=1384317000&serverId=465&agentId=12",
                "agentId": 12,
                "serverId": 465,
                "server": "a1.verisigndns.com.",
                "resolutionTime": 3,
                "roundId": 1384317000,
                "errorDetails": "",
                "mappings": "a1.verisigndns.com. dnssupport.verisign-grs.com. 2284831191 28800 7200 1209600 3600 (SOA)\na1.verisigndns.com. (NS)\nu1.verisigndns.com. (NS)\na2.verisigndns.com. (NS)\na3.verisigndns.com. (NS)\n54.215.17.122 (A)\n0 aspmx.l.google.com. (MX)\n5 alt1.aspmx.l.google.com. (MX)\n10 aspmx3.googlemail.com. (MX)\n5 alt2.aspmx.l.google.com. (MX)\n10 aspmx2.googlemail.com. (MX)\n\"7LO6JGX\" (TXT)\n\"v=spf1 ip4:65.122.4.140 ip4:208.185.7.125 include:_spf.google.com include:daredevil.thousandeyes.com include:sendgrid.net include:spf.braintreegateway.com include:emailer.hubspot.com include:eventbrite.com include:support.zendesk.com include:smtp.zendesk.\" \"com include:mktoma\\\" \\\"il.com ~all\" (TXT)\n\"google-site-verification=J9DoQXN_dDvzrDb53j_fETYTJjIRKOn-42sPbrJvpHA\" (TXT)"
            },
			...
        ]
    },
    "pages": {
        "current": 1
    }
}```

For more information on our HTTP response status codes, see the [response status codes documentation][overview-responsestatuscodes].