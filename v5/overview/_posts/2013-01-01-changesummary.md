---
parent_category: overview
parent_category_label: Overview

title: 'API Change Summary'

sortorder: 11
category-sortorder: 1
layout: null
---

The following list documents a running list of changes to version 5 of the ThousandEyes API.  Each release date is linked to the specific release notes for that release, for which Authentication is required.  For details related to other versions of the ThousandEyes API, see the links on the left sidebar.

#APIv5

[August 15, 2018][releasenotes-20180815]

* (modified) The way pagination returns the pages element has changed. The `previous` property was removed from pages element and the `next` property now uses `lastRoundId` URL parameter instead of `page` for the pagination

[July 03, 2018][releasenotes-20180703]

* (added) `/agents` endpoint now accepts a new  querystring parameter: `agentTypes=CLOUD|ENTERPRISE|ENTERPRISE_CLUSTER`

[March 28, 2018][releasenotes-20180328]

* (added) For tests which included TCP-based Network Measurements (under the Advanced Settings tab of the test configuration) you can now pick `TCP` or `ICMP` as a `protocol` when creating or editing tests.

[January 31, 2018][releasenotes-20180131]
* We are deprecating the use of TLS 1.0 in ThousandEyes infrastructure. After tonight, clients which use only TLS 1.0 will not be able to access the API. If your client program or script is returning errors such as `The underlying connection was closed: An unexpected error occurred on a send.`, `EOF occurred in violation of protocol` or `Connection reset by peer`, you need to either upgrade your SSL libraries or ensure you are not enforcing the use of TLS 1.0.
* (added) When creating or updating a test that uses BGP monitoring, use the new `usePublicBgp` parameter set to `1` to automatically add all available Public BGP Monitors. You can continue to use the `bgpMonitors` option to assign Private BGP Monitors.
* (changed) If a public monitor is included in the `bgpMonitors` list, all public monitors will be assigned to the test, regardless of the setting specified in the `usePublicBgp` parameter.

[January 10, 2018][releasenotes-20180110]

* APIv4 has been deprecated. All queries to version 4 (`https://api.thousandeyes.com/v4/`) will receive a `HTTP 301` redirect to the oldest supported API version (currently version 5).

[October 12, 2017][releasenotes-20171012]
* (added) `/dns/trace` endpoint has new parameters: `failedQueries` and `finalServerQueried`
* (added) `/net/path-vis` endpoint has new parameters: `sourceIp` and `sourcePrefix`

[September 27, 2017][releasenotes-20170927]
* (added) APIv6 is now default version. All queries without explicit version (`https://api.thousandeyes.com/` instead of `https://api.thousandeyes.com/v5/`) will receive version 6 response.
* 90-day countdown for APIv4 deprecation begins with a target date of December 26, 2017.  Please refer to the [versioning][overview-versioning] page for details.
* (added) Agent endpoints `agentState` parameter now has a third state: `Disabled`

[June 8, 2016][releasenotes-20160608]
* (fixed) added countryId to `/net/metrics` endpoint
* (added) errorDetails field is now exposed to `/net/metrics` endpoint

[May 25, 2016][releasenotes-20160525]
* updated change policy section (v4) to show that version 6 is in preview mode and accessible
* updated change policy section (v5) to show that version 6 is in preview mode and accessible

[March 30, 2016][releasenotes-20160330]
* (added) groups endpoints
* (added) groups metadata to agent and test endpoints

[December 16, 2015][releasenotes-20151216]
* (added) OrganizationName field in to /accounts endpoint output when users are subscribed to multiple organizations
* (changed) made some minor terminology updates to test metadata descriptions.

[October 14, 2015][releasenotes-20151014]
* (added) Added support for `transactionScript` output in transaction test retrieval
* (added) Added support for `transactionScript` input in transaction test creation and update
* (added) Added two new fields, `startStep` and `endStep` in transaction test retrieval, creation and update

[August 5, 2015][releasenotes-20150805]
* (added) Web transactions now support programmatic test creation.  Refer to the [test metadata][test-metadata] page for more information on using the new field.
* (changed) corrected minor markdown formatting errors

[July 22, 2015][releasenotes-20150722]
* (changed) format of `/agents` and `/agents/{agentId}` endpoint
* (changed) corrected minor markdown formatting errors

[June 10, 2015][releasenotes-20150610]
* (removed) API versions 2 and 3 have both been deprecated
* (changed) v5 is now the default version for API requests made without requesting an explicit version.

[May 27, 2015][releasenotes-20150527]
* (added) added `agents/{agentId}/update` endpoint
* (added) added `agents/{agentId}/delete` endpoint
* (fixed) `agents/{agentId}` endpoint now returns only tests assigned to the agent in the current account context.  Prior to the fix, all tests were being shown
* (fixed) date fields have been added back to the `/alerts` endpoint when using XML-based output.  Prior to the fix, these fields were missing.

[May 13, 2015][releasenotes-20150513]
* (added) added monitorId to `/net/bgp-metrics/{testId}` endpoint.
* (added) added `/agents/{agentId}` endpoint.
* (updated) documentation for agents and alerts category now shows a table of possible fields returned in the data
* (updated) removed beta flag for Voice tests

[March 4, 2015][releasenotes-20150304]
* **OFFICIAL RELEASE OF APIv5**
* (changed) savedEvent and enabled fields have been converted from attributes to elements in XML output format for the API.
* (changed) version-specific XML schema documents (.xsd files) are now being generated for each version of the API.  Refer to the Response Formats heading for more details.
* (added) the `/agents` endpoint has been updated.  Added new fields and modified existing fields related to ipAddresses.  
* (added) `bgpMonitors` field is now modifiable on tests via the API.
* (changed) renamed `includeSubPrefixes` field to `includeCoveredPrefixes`

[January 21, 2015][releasenotes-20150121]
* (fixed) documentation errors on test metadata page related to agents array.
* (added) `redirectTime` field on HTTP Server output
* (changed) test updates (fields modifiable via the API) now require the test to be enabled, in order to commit a change.  
* (added) `/bgp-monitors` endpoint (APIv5 only)

[October 29, 2014][releasenotes-20141029]
* (added) `roundsBeforeTrigger` field to alert Rules
* (fixed) issues retreiving page load data for saved events
* (added) saved events page added to tests category

[October 15, 2014][releasenotes-20141015]
* Preview for APIv5 launched
* (added) New endpoint for [Page Load Component Detail][pageload-component-detail-endpoint]
* (added) New endpoint for [Transaction Component Detail][transaction-component-detail-endpoint]
* (added) New endpoint for [BGP route information][bgp-aspath-endpoint]
