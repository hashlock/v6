---
parent_category: overview
parent_category_label: Overview

title: 'API Change Summary'

sortorder: 1
category-sortorder: 1
layout: null
---

The following list documents a running list of changes to version 4 of the ThousandEyes API.  Each release date is linked to the specific release notes for that release, for which Authentication is required.  For details related to other versions of the ThousandEyes API, see the links on the left sidebar.

#APIv4

[January 10, 2018][releasenotes-20180110]

* APIv4 has been deprecated. All queries to version 4 (`https://api.thousandeyes.com/v4/`) will receive a `HTTP 301` redirect to the oldest supported API version (currently version 5). Please refer to [current API version documentation][current-api].

[September 27, 2017][releasenotes-20170927]
* APIv6 is now default version. All queries without explicit version (`https://api.thousandeyes.com/` instead of `https://api.thousandeyes.com/v5/`) will receive version 6 response.
* 90-day countdown for APIv4 deprecation begins with a target date of December 26, 2017.  Please refer to the [versioning][overview-versioning] page for details.

[May 25, 2016][releasenotes-20160524]
* updated change policy section (v4) to show that version 6 is in preview mode and accessible
* updated change policy section (v5) to show that version 6 is in preview mode and accessible

[June 10, 2015][releasenotes-20150610]
* (changed) version 4 is no longer the default version of the ThousandEyes API.  To make requests to version 4, specify the version in the request, using `https://api.thousandeyes.com/v4/`
* (removed) API versions 2 and 3 have been deprecated

[May 27, 2015][releasenotes-20150527]
* (note) This is the last release where v4 is the current API endpoint.
* (fixed) date fields have been added back to the `/alerts` endpoint when using XML-based output.  Prior to the fix, these fields were missing.

[January 21, 2015][releasenotes-20150121]
* (fixed) documentation errors on test metadata page related to agents array.
* (added) `redirectTime` field on HTTP Server output

[October 29, 2014][releasenotes-20141029]
* (added) `roundsBeforeTrigger` field to alert Rules
* (fixed) issues retreiving page load data for saved events
* (added) saved events page added to tests category

[October 15, 2014][releasenotes-20141015]
* (added) `recursiveQueries: 0|1` to DNS Server instant tests (defaults to 0)
* (added) `sslVersionId: 0|1|3` to HTTP Server instant tests (defaults to 0)
* (added) `verifySslCertificate: 0|1` to HTTP Server instant tests (defaults to 1)
* (added) `postBody: <string>` to HTTP Server instant tests.

[September 17, 2014][releasenotes-20140917]
* Changed layout of test update and test creation endpoints, added [Test Metadata][test-metadata] reference page
* Harmonized user interface options for test creation/edit with API options - see creation/update endpoints for more details
* Added `serverId` and `serverName` objects to DNS Server alerts, specifying specific DNS Server which is alerting.
* Corrected API-based test creation; all tests created via the API prior to this change were created with alerting disabled.

[August 20, 2014][releasenotes-20140820]
* (added) [voice test endpoints][voip-endpoints].  This feature is currently in beta.  If you are interested in joining the beta, contact the Customer Success team, or your account representative.
* (added) ability to update transaction tests
* (added) alert rules to `/tests/{testId}` endpoint
* (changed) In this release, we also split API documentation into version-specific targets.  See the left sidebar to change your version context.

[July 23, 2014][releasenotes-20140723]
* (added) `mappings` field for DNS Trace instant test response
* (changed) fixed an issue where status changed time format depending on endpoint

[July 9, 2014][releasenotes-20140709]
* API v4 released
* (changed) Terminology change: Private Agent renamed to Enterprise Agent
* (changed) Terminology change: Public Agent renamed to Cloud Agent
