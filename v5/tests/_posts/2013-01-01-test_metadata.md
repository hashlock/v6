---
parent_category: tests
parent_category_label: Tests

title: 'Test Metadata'

sortorder: 4
category-sortorder: 2
layout: null
---

Test fields are shown in the table below, by type.  The table indicates whether the fields can be used in test creation, updates, or simply in returning data.  As a reminder, in order to create a new test, the user attempting the creation must be an Account Admin; Regular users are blocked from using any of the POST-based methods.

Fields are listed alphabetically by test type below.

Where a field indicates n/a for both Test Creation and Test Update, these fields are system-generated and read only, and displayed as part of test metadata.

Test Type | Field | Test Creation | Test Update | Data Type | Acceptable Values | Notes
:------------|-------------|-------------|-------------|---------------|-----------|--------------|
(all) | alertsEnabled | Optional | Optional | integer | 0 or 1 | choose 1 to enable alerts, or 0 to disable alerts.  Defaults to 1
(all) | alertRules | Optional | Optional | array of alert rule objects {"ruleId": ruleId} | see notes | get ruleId from `/alert-rules` endpoint.  If alertsEnabled is set to 1 and alertRules is not included in a creation/update query, applicable defaults will be used.
(all) | apiLinks | n/a | n/a | array of apiLinks objects | array of apiLink objects, showing rel and href elements | Read only; self links to endpoint to pull test metadata, and data links to endpoint for test data
(all) | createdBy | n/a | n/a | string | Username (email@company.com) | read only
(all) | createdDate | n/a | n/a | string | YYYY-MM-DD HH:mm:ss formatted date | read only; shown in UTC
(all) | enabled | n/a | Optional | integer | 0 or 1 | choose 1 to enable the test, 0 to disable the test
(all) | groups | Optional | Optional | array of label objects (`"groups": [ { "name": "groupName", "groupId": groupId, "builtIn": 0}]`) | see notes | get groupId from `/groups`endpoint.
(all except DNS+ tests) | liveShare | n/a | n/a | integer | 0 or 1 | read only; indicates 1 for a test shared with your account, 0 for a normal test
(all) | modifiedBy | n/a | n/a | string | Username (email@company.com) | read only
(all) | modifiedDate | n/a | n/a | string | YYYY-MM-DD HH:mm:ss formatted date | read only; shown in UTC
(all) | savedEvent | n/a | n/a | integer | 0 or 1 | read only; indicates 1 for a saved event, 0 for a normal test
(all) | testId | n/a | n/a | integer | unique ID of test | read only; each test is assigned a unique ID; this is used to access test data from other endpoints.
(all) | testName | Required | Optional | string | (any) | Test name must be unique
(all) | type | n/a | n/a | string | type of test being queried | This is a read only value, as test type is implicit in the test creation url.
Network | protocol | Optional | n/a | string | `TCP` or `ICMP` | protocol and port (shown below) are mutually exclusive - if ICMP is selected and port is provided, the request will error; defaults to `TCP`
HTTP Server, DNS Server, Page Load | protocol | Optional | Optional | string | `TCP` or `ICMP` | protocol used by dependent Network tests (End-to-end, Path Trace, PMTUD); defaults to `TCP`
BGP | bgpMonitors | Optional | Optional | array of BGP Monitor objects {"monitorId": monitorId} | see notes | Get monitorId from `/bgp-monitors` endpoint. If a public monitor is included in the `bgpMonitors` list on test creation/update, all public monitors will be assigned to the test, regardless of the setting specified in the `usePublicBgp` parameter.
BGP | usePublicBgp | Optional | Optional | integer | 0 or 1 | set to `1` to automatically add all available Public BGP Monitors
BGP | includeCoveredPrefixes | Optional | Optional | integer | 0 or 1 | set to 1 to include queries for subprefixes detected under this prefix
BGP | prefix | Required | n/a | string | a.b.c.d/e | a.b.c.d is a network address, with the prefix length defined as e.  Prefixes can be any length from 8 to 24
DNS Server | agents | Required | Optional | array of agent objects {"agentId": agentId} | see notes | get agentId from `/agents` endpoint
DNS Server | bandwidthMeasurements | Optional | Optional | integer | 0 or 1 | set to 1 to measure bandwidth.  Only applies to Enterprise Agents assigned to the test, and requires that networkMeasurements is set.  **We do not recommend setting this flag on DNS server tests**
DNS Server | bgpMeasurements | Optional | Optional | integer | 0 or 1 | choose 1 to enable bgp measurements, 0 to disable; defaults to 1 when networkMeasurements is set
DNS Server | bgpMonitors | Optional | Optional | array of BGP Monitor objects {"monitorId": monitorId} | see notes | get monitorId from `/bgp-monitors` endpoint.
DNS Server | dnsServers | Required | Optional | array of DNS Server objects {"serverName": "fqdn of server"} | | |
DNS Server | domain | Required | n/a | string | see notes | target record for test, suffixed by record type (ie, `www.thousandeyes.com CNAME`).  If no record type is specified, the test will default to an ANY record.
DNS Server | interval | Required | Optional | integer | \[300, 600, 900, 1800, 3600\] | value in seconds
DNS Server | mtuMeasurements | Optional | Optional | integer | 0 or 1 | set to 1 to measure MTU sizes on network from agents to the target
DNS Server | networkMeasurements | Optional | Optional | integer | 0 or 1 | choose 1 to enable network measurements, 0 to disable;  defaults to 1
DNS Server | recursiveQueries | Optional | Optional | integer | 0 or 1 | set to 1 to run query with RD (recursion desired) flag enabled
DNS Trace | agents | Required | Optional | array of agent objects {"agentId": agentId} | see notes | get agentId from `/agents` endpoint
DNS Trace | domain | Required | n/a | string | see notes | target record for test, suffixed by record type (ie, `www.thousandeyes.com CNAME`).  If no record type is specified, the test will default to an ANY record.
DNS Trace | interval | Required | Optional | integer | \[300, 600, 900, 1800, 3600\] | value in seconds
DNS+ Domain | domain | Required | n/a | string | see notes | target record for test, followed by record type (ie, `www.thousandeyes.com A`)
DNS+ Domain | interval | Required | Optional | integer | \[900, 1800, 3600\] | value in seconds
DNS+ Server | interval | Required | Optional | integer | \[3600, 7200, 10800\] | value in seconds
DNS+ Server | server | Required | Read-only | string | see notes | fqdn of DNS resolver
DNSSEC | agents | Required | Optional | array of agent objects {"agentId": agentId} | see notes | get agentId from `/agents` endpoint.
DNSSEC | domain | Required | n/a | string | see notes | target record for test, followed by record type (ie, `www.thousandeyes.com A`)
DNSSEC | interval | Required | Optional | integer | \[300, 600, 900, 1800, 3600\] | value in seconds
HTTP Server | agents | Required | Optional | array of agent objects {"agentId": agentId} | see notes | get agentId from `/agents` endpoint.
HTTP Server | authType | Optional | Optional | string | `NONE`, `BASIC`, `NTLM`, `KERBEROS` | HTTP Authentication type; defaults to `NONE`
HTTP Server | bandwidthMeasurements | Optional | Optional | integer | 0 or 1 | set to 1 to measure bandwidth.  Only applies to Enterprise Agents assigned to the test, and requires that networkMeasurements is set.  
HTTP Server | bgpMeasurements | Optional | Optional | integer | 0 or 1 | choose 1 to enable bgp measurements, 0 to disable; defaults to 1 when networkMeasurements is set
HTTP Server | bgpMonitors | Optional | Optional | array of BGP Monitor objects {"monitorId": monitorId} | see notes | get monitorId from `/bgp-monitors` endpoint.
HTTP Server | contentRegex | Optional | Optional | string | [Regular Expressions][regexp-quickref] | This field does not require escaping
HTTP Server | downloadLimit | Optional | Optional | integer | any | specify maximum number of bytes to download from the target object
HTTP Server | headers | Optional | Optional | array of header strings \["header: value", "header2: value"\] | any | use HTTP header values in this list
HTTP Server | httpTargetTime | Optional | Optional | integer | (100..5000) | target time for HTTP server completion; specified in milliseconds
HTTP Server | httpTimeLimit | Optional | Optional | integer | (5..60) | defaults to 5 seconds
HTTP Server | interval | Required | Optional | integer | \[120, 300, 600, 900, 1800, 3600\] | value in seconds
HTTP Server | mtuMeasurements | Optional | Optional | integer | 0 or 1 | set to 1 to measure MTU sizes on network from agents to the target
HTTP Server | networkMeasurements | Optional | Optional | integer | 0 or 1 | choose 1 to enable network measurements, 0 to disable;  defaults to 1
HTTP Server | password | Optional | Optional | string | see notes | password to be used for Basic/NTLM authentication
HTTP Server | postBody | Optional | Optional | string | see notes | Enter the post body in this field.  No escaping is required.  If the post body is set to something other than empty, the requestMethod will be set to POST.
HTTP Server | sslVersion | Read only | Read Only | string | corresponds to sslVersionId | Reflects the verbose ssl protocol version used by a test
HTTP Server | sslVersionId | Optional | Optional | integer | \[0,3,4,5,6\] | 0 for auto, 3 for SSLv3, 4 for TLS v1.0, 5 for TLS v1.1, 6 for TLS v1.2
HTTP Server | url | Required | n/a | string | see notes | target for the test
HTTP Server | useNtlm | Optional | Optional | integer | 0 or 1 | choose 1 to use NTLM, 0 to use Basic Authentication.  Requires username/password to be set
HTTP Server | userAgent | Optional | Optional | string | see notes | user-agent string to be provided during the test
HTTP Server | username | Optional | Optional | string | see notes | username to be used for Basic/NTLM authentication
HTTP Server | verifyCertificate | Optional | Optional | integer | 0 or 1 | set to 0 to ignore certificate errors (defaults to 1)
Network | agents | Required | Optional | array of agent objects {"agentId": agentId} | see notes | get agentId from `/agents` endpoint.
Network | bandwidthMeasurements | Optional | Optional | integer | 0 or 1 | set to 1 to measure bandwidth.  Only applies to Enterprise Agents assigned to the test
Network | bgpMeasurements | Optional | Optional | integer | 0 or 1 | choose 1 to enable bgp measurements, 0 to disable; defaults to 1
Network | bgpMonitors | Optional | Optional | array of BGP Monitor objects {"monitorId": monitorId} | see notes | get monitorId from `/bgp-monitors` endpoint.
Network | interval | Required | Optional | integer | \[120, 300, 600, 900, 1800, 3600\] | value in seconds
Network | mtuMeasurements | Optional | Optional | integer | 0 or 1 | set to 1 to measure MTU sizes on network from agents to the target
Network | port | Optional | n/a | integer | (1..65535) | if protocol is TCP, defaults to port 80.  Protocol and port (shown below) are mutually exclusive - if ICMP is selected and port is provided, the request will error.
Network | server | Required | n/a | string | (any) | target name or IP address
Page Load | agents | Required | Optional | array of agent objects {"agentId": agentId} | see notes | get agentId from `/agents` endpoint
Page Load | authType | Optional | Optional | string | `NONE`, `BASIC`, `NTLM`, `KERBEROS` | HTTP Authentication type; defaults to `NONE`
Page Load | bandwidthMeasurements | Optional | Optional | integer | 0 or 1 | set to 1 to measure bandwidth.  Only applies to Enterprise Agents assigned to the test, and requires that networkMeasurements is set
Page Load | bgpMeasurements | Optional | Optional | integer | 0 or 1 | choose 1 to enable bgp measurements, 0 to disable; defaults to 1 when networkMeasurements is set
Page Load | bgpMonitors | Optional | Optional | array of BGP Monitor objects {"monitorId": monitorId} | see notes | get monitorId from `/bgp-monitors` endpoint.
Page Load | httpInterval | Required | Optional | integer | \[120, 300, 600, 900, 1800, 3600\] | Cannot be larger than the interval value; defaults to the same value as interval
Page Load | httpTargetTime | Optional | Optional | integer | (100..5000) | target time for HTTP server completion; specified in milliseconds
Page Load | httpTimeLimit | Optional | Optional | integer | (5..60) | defaults to 5 seconds
Page Load | includeHeaders | Optional | Optional | integer | 0 or 1 | set to 1 to capture response headers for objects loaded by the test. Default is 1.
Page Load | interval | Required | Optional | integer | \[120, 300, 600, 900, 1800, 3600\] | value in seconds
Page Load | mtuMeasurements | Optional | Optional | integer | 0 or 1 | set to 1 to measure MTU sizes on network from agents to the target
Page Load | networkMeasurements | Optional | Optional | integer | 0 or 1 | choose 1 to enable network measurements, 0 to disable;  defaults to 1
Page Load | pageLoadTargetTime | Optional | Optional | integer | (1..30) |  target time for Page Load completion; specified in seconds; cannot exceed pageLoadTimeLimit value
Page Load | pageLoadTimeLimit | Optional | Optional | integer | (5..MIN(0.25 * interval, 60) | must be larger than httpTimeLimit; defaults to 10 seconds
Page Load | password | Optional | Optional | string | see notes | password to be used for Basic/NTLM authentication
Page Load | sslVersion | Read only | Read Only | string | corresponds to sslVersionId | Reflects the verbose ssl protocol version used by a test
Page Load | sslVersionId | Optional | Optional | integer | \[0,3,4,5,6\] | 0 for auto, 3 for SSLv3, 4 for TLS v1.0, 5 for TLS v1.1, 6 for TLS v1.2
Page Load | url | Required | n/a | string | see notes | target for the test
Page Load | useNtlm | Optional | Optional | integer | 0 or 1 | choose 1 to use NTLM, 0 to use Basic Authentication.  Requires username/password to be set
Page Load | userAgent | Optional | Optional | string | see notes | user-agent string to be provided during the test
Page Load | username | Optional | Optional | string | see notes | username to be used for Basic/NTLM authentication
Page Load | verifyCertificate | Optional | Optional | integer | 0 or 1 | set to 0 to ignore certificate errors (defaults to 1)
Transaction | agents | Required | Optional | array of agent objects {"agentId": agentId} | see notes | get agentId from `/agents` endpoint.  
Transaction | authType | Optional | Optional | string | `NONE`, `BASIC`, `KERBEROS` | HTTP Authentication type; defaults to `NONE`
Transaction | includeHeaders | Optional | Optional | integer | 0 or 1 | set to 1 to capture response headers for objects loaded by the test.  Default is 1.
Transaction | interval | Required | Optional | integer | \[300, 600, 900, 1800, 3600\] | value in seconds
Transaction | password | Optional | Optional | string | see notes | password to be used for Basic/NTLM authentication
Transaction | targetTime | Optional | Optional | integer | (1..60) | target time for completion, defaults to 50% of time limit; specified in seconds
Transaction | timeLimit | Optional | Optional | integer | (5..MIN(0.25 * interval, 180)) | time limit for transaction; defaults to 30s
Transaction | userAgent | Optional | Optional | string | see notes | user-agent string to be provided during the test
Transaction | username | Optional | Optional | string | see notes | username to be used for Basic/NTLM authentication
Transaction | url | Required | Read Only | string | see notes | target for the test
Transaction | transactionScript | Optional (see notes) | Optional (see notes) | string | see notes | HTML version of a transaction script.  Quotes must be escaped (precede `"` characters with `\`).  To obtain a transaction script for upload, click the Export as file option from the ThousandEyes Recorder.  Note that this field is write-only and can only be used on test creation.
Transaction | transactionSteps | Optional (see notes) | Optional (see notes) | array of `transactionStep` objects. | see `transactionSteps.<field>` values below | Either `transactionSteps` or `transactionScript` can be specified.  If `transactionSteps` is specified, then this value will override any `transactionScript` field supplied in the call
Transaction | transactionSteps.stepNum | Required | Optional | integer | Zero-indexed step number.  | Steps must be provided sequentially, and must start at zero
Transaction | transactionSteps.stepName | Required | Optional | string | see notes | name for the step
Transaction | transactionSteps.command | Required | Optional | string | see notes | command for the step
Transaction | transactionSteps.target | Required | Optional | string | see notes | target for the step
Transaction | transactionSteps.value | Optional | Optional | string | see notes | if a value field is required as part of the transaction step (for example, a waitForCondition command), specify the value in this field.  If no value is required, omit the value field.
Transaction | startStep | Optional | Optional | integer | Transaction step number to begin the transaction timer - this is based on the zero-indexed `stepNum` field specified in `transactionSteps`.  | If omitted, the transaction will start at step 0 of the transaction script
Transaction | endStep | Optional | Optional | integer | Transaction step number to end the transaction timer - this is based on the zero-indexed `stepNum` field specified in `transactionSteps`.  | If omitted, the transaction will end at the last step in the transaction script
Voice | agents | Required | Optional | array of agent objects {"agentId": agentId} | see notes | get agentId from `/agents` endpoint.
Voice | bgpMeasurements | Optional | Optional | integer | 0 or 1 | choose 1 to enable bgp measurements, 0 to disable; defaults to 1 when networkMeasurements is set
Voice | codec | n/a | Read Only | string | see notes | codec label
Voice | codecId | Required | Optional | integer | [Voice codec list][voip-codec-list] | see list for acceptable values
Voice | dscp | n/a | Read Only | string | see notes | dscp label
Voice | dscpId | Required | Optional | integer | [DSCP list][dscp-list] | see list for acceptable values
Voice | interval | Required | Optional | integer | \[300, 600, 900, 1800, 3600\] | value in seconds
Voice | jitterBuffer | Required | Optional | integer | (0..150) | de-jitter buffer size (in seconds)
Voice | mtuMeasurements | Optional | Optional | integer | 0 or 1 | set to 1 to measure MTU sizes on network from agents to the target
Voice | networkMeasurements | Optional | Optional | integer | 0 or 1 | choose 1 to enable network measurements, 0 to disable;  defaults to 1
Voice | targetAgentId | Required | Read Only | integer | pull from /agents | Both the `"agents": []` and the targetAgentId cannot be cloud agents.  Can be Enterprise Agent -> Cloud, Cloud -> Enterprise Agent, or Enterprise Agent -> Enterprise Agent
