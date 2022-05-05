---
parent_category: agents
parent_category_label: Agents & Monitors

path: '{{ site.version_url_prefix_request }}/agents/{agentId}/update'
title: 'Updating an Agent'
type: POST

sortorder: 3
category-sortorder: 5
layout: null
---

Updates Enterprise Agent details.  Users can update the agent display name, as well as change test and account assignments.

This endpoint can only be used for Enterprise Agents, and only for users in a role that permits modification of Enterprise Agents.

Important notes related to agent modification on tests:
* if an agent is removed from a test, the modification date for tests using that agent at the time it was removed will be changed.  
* If an agent is removed from an entire account, then all tests using this agent in the removed account will be updated to reflect the removed agent.  
* If a removed agent is the final remaining agent on a test, then the test will be disabled when the agent is removed.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={accountId}` optional and requires the user to be assigned to the target account, specifies the account context of the request, obtained from the `/accounts` endpoint.  Specifying this parameter without the user to be assigned to the target account will result in an error response. See [Account Context][overview-accountcontext] for more information

### Request

* `{agentId}` corresponds the unique ID of an enterprise agent, obtained from the `/agents` endpoint

### Post Data

When POSTing data to the `/agents/{agentId}/update` endpoint, users can update the following fields:

* `agentName` string representation of an agent.  No two agents can have the same display name.
* `accounts` an array of account objects containing only an aid value, in the format { aid: integer }.  See `/accounts` to pull a list of account IDs
* `tests` an array of test objects containing only a testId value in the format { testId: integer }.  See `/tests` to pull a list of tests available in the current account context.

### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/agents/966/update \
  -d '{ "agentName": "my updated agent name", \
    "accounts": [\
      {"aid": 315},
      {"aid": 362}
    ],
    "tests": [\
      {"testId": 12065}
      {"testId": 817},
  }' \
  -H "Content-Type: application/json" \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

If an agent is successfully edited, an HTTP/200 OK response will be returned, and the agent's assigned accounts / tests will change; the newly updated agent data will be returned.  See the example below:

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
agentId | integer | n/a | unique ID of agent
agentName | string | n/a | display name of the agent
location | string | n/a | location of the agent
countryId | string | n/a | ISO-3166-1 alpha-2 country code of the agent
prefix | string | n/a | Network prefix, expressed in CIDR format
utilization | float | percentage | utilization of the agent, expressed in decimal format, where 0 = 0% and 1 = 100% utilization
ipAddresses | array | n/a | array of ipAddress entries
groups | array | n/a | array of label objects - see Labels for more information.
enabled | boolean | n/a | 1 for enabled, 0 for disabled
accounts | array | n/a | list of accounts to which the agent is assigned, expressed in the same format as `/accounts` endpoint
tests | array | n/a | list of tests assigned to the agent, expressed in the same format as `/tests` endpoint
network | string | n/a | name of the autonomous system in which the Agent is found
agentType | string | n/a | either Cloud or Enterprise, shows the type of agent
lastSeen | dateTime | n/a | yyyy-MM-dd hh:mm:ss, expressed in UTC
agentState | string | n/a | either Online or Offline

#### Header

```HTTP/1.1 200 OK
Date: Thu, 14 May 2015 23:18:04 GMT
Server: Apache
Vary: Accept-Encoding
Strict-Transport-Security: max-age=31536000
X-Frame-Options: sameorigin
Transfer-Encoding: chunked
Content-Type: text/xml```

#### Body

```{
    "agents": [
        {
            "agentId": 966,
            "agentName": "ubuntu1404-x64",
            "location": "San Francisco Bay Area",
            "countryId": "US",
            "prefix": "50.128.0.0/9",
            "utilization": 1,
            "ipAddresses": [
                "192.168.1.223"
            ],
            "publicIpAddresses": [
                "50.184.189.59"
            ],
            "enabled": 1,
            "accounts": [
                {
                    "aid": 315,
                    "accountName": "Documentation"
                },
                {
                    "aid": 362,
                    "accountName": "Enterprise Agents Dashboard"
                }
            ],
            "tests": [
                {
                    "createdDate": "2015-02-03 21:55:13",
                    "modifiedDate": "2015-05-14 23:18:03",
                    "createdBy": "API Sandbox User (noreply@thousandeyes.com)",
                    "modifiedBy": "API Sandbox User (noreply@thousandeyes.com)",
                    "enabled": 1,
                    "savedEvent": 0,
                    "testId": 12065,
                    "testName": "My Google DNS test",
                    "type": "dns-server",
                    "interval": 300,
                    "domain": "google.com A",
                    "networkMeasurements": 1,
                    "mtuMeasurements": 1,
                    "bandwidthMeasurements": 0,
                    "bgpMeasurements": 1,
                    "alertsEnabled": 0,
                    "liveShare": 0,
                    "recursiveQueries": 0,
                    "dnsServers": [
                        {
                            "serverId": 130,
                            "serverName": "ns2.google.com."
                        }
                    ],
                    "apiLinks": [...]
                },                
                {
                    "enabled": 1,
                    "testId": 817,
                    "savedEvent": 0,
                    "liveShare": 0,
                    "testName": "http://www.thousandeyes.com",
                    "type": "http-server",
                    "interval": 900,
                    "url": "http://www.thousandeyes.com",
                    "networkMeasurements": 1,
                    "createdBy": "API Sandbox User (noreply@thousandeyes.com)",
                    "modifiedBy": "API Sandbox User (noreply@thousandeyes.com)",
                    "createdDate": "2012-06-28 19:33:12",
                    "modifiedDate": "2015-05-14 23:18:03",
                    "apiLinks": [...]
                }
            ],
            "network": "Comcast Cable Communications, Inc. (AS 7922)",
            "agentType": "Enterprise",
            "lastSeen": "2015-05-14 23:18:00",
            "agentState": "Online"
        }
    ]
}```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
