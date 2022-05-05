---
parent_category: tests
parent_category_label: Tests

path: ''
title: 'Saved Events'
sortorder: 8
category-sortorder: 2
layout: nil
---

{.inline-code}While there is no specific endpoint for Saved Events, ThousandEyes allows users to retrieve data for saved events through the normal test data endpoints.  See the [Test Data][test-data] section for a list of endpoints that can be used to retrieve data for saved events. Unlike standard tests, saved events MUST be queried with the date range you wish to retrieve.  See the `window=` and `from=` parameters in the list below.

* `window=[0-9]+[smhdw]?` specifies a window of time for the result set.  See [Time Ranges][overview-timerange] for more information.
* `from=YYYY-mm-ddTHH:MM:SS&to=YYYY-mm-ddTHH:MM:SS` specifies an explicit start (and optionally, end) for your range of data.  See [Time Ranges][overview-timerange] for more information.

Without specifying a time range parameter, your query will return only the last round of data available.  In the two examples below, the first request returns the test's first round of data, whereas the second request (where a date range is specified) returns the data for the requested range.

### Example 1 (no time range specified)

Note in this circumstance, the last round of data is returned (since a request without a time range was specified)

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/web/page-load/35199.json \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

```{
    "web": {
        "test": {
            "enabled": 1,
            "savedEvent": 1,
            "testId": 35199,
            "testName": "Google Event",
            "type": "page-load",
            "interval": 900,
            "httpInterval": 900,
            "url": "http://www.google.com",
            "networkMeasurements": 1,
            "mtuMeasurements": 1,
            "bandwidthMeasurements": 0,
            "bgpMeasurements": 1,
            "alertsEnabled": 0,
            "liveShare": 0,
            "httpTimeLimit": 5,
            "httpTargetTime": 1000,
            "pageLoadTimeLimit": 30,
            "pageLoadTargetTime": 2,
            "includeHeaders": 0,
            "sslVersionId": 0,
            "verifyCertificate": 1,
            "useNtlm": 0,
            "createdDate": "2014-10-28 16:15:10",
            "createdBy": "API Sandbox Admin (api.sandbox+admin@thousandeyes.com)",
            "sslVersion": "Default",
            "apiLinks": [...]
        },
        "pageLoad": [
            {
                "countryId": "HK",
                "date": "2014-10-22 10:03:04",
                "agentName": "Hong Kong",
                "agentId": 12,
                "roundId": 1413972000,
                "responseTime": 35,
                "totalSize": 7278,
                "numObjects": 4,
                "numErrors": 0,
                "domLoadTime": 171,
                "pageLoadTime": 205,
                "permalink": "https://app.thousandeyes.com/web/page-load?__a=75&testId=35199&roundId=1413972000&agentId=12"
            },
            ...
        ]
    },
    "pages": {
        "current": 1
    }
}```

### Example 2 (time range specified)

Note in this circumstance, the full set of data is returned, and may involve paging - since a specific range was specified.  If the time range used in the query did not overlap with the available data, no data would be returned.

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/web/page-load/35199.json?from=2014-10-22+06:00:00&to=2014-10-22+10:00:00\
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

```{
    "from": "2014-10-22 04:00:00",
    "to": "2014-10-22 08:00:00",
    "web": {
        "test": {
            "enabled": 1,
            "savedEvent": 1,
            "testId": 35199,
            "testName": "Google Event",
            "type": "page-load",
            "interval": 900,
            "httpInterval": 900,
            "url": "http://www.google.com",
            "networkMeasurements": 1,
            "mtuMeasurements": 1,
            "bandwidthMeasurements": 0,
            "bgpMeasurements": 1,
            "alertsEnabled": 0,
            "liveShare": 0,
            "httpTimeLimit": 5,
            "httpTargetTime": 1000,
            "pageLoadTimeLimit": 30,
            "pageLoadTargetTime": 2,
            "includeHeaders": 0,
            "sslVersionId": 0,
            "verifyCertificate": 1,
            "useNtlm": 0,
            "createdDate": "2014-10-28 16:15:10",
            "createdBy": "API Sandbox Admin (api.sandbox+admin@thousandeyes.com)",
            "sslVersion": "Default",
            "apiLinks": [...]
        },
        "pageLoad": [
            {
                "countryId": "BR",
                "date": "2014-10-22 06:01:17",
                "agentName": "São Paulo, Brazil",
                "agentId": 16,
                "roundId": 1413957600,
                "responseTime": 2,
                "totalSize": 6405,
                "numObjects": 3,
                "numErrors": 0,
                "domLoadTime": 176,
                "pageLoadTime": 233,
                "permalink": "https://app.thousandeyes.com/web/page-load?__a=75&testId=35199&roundId=1413957600&agentId=16"
            },
            ...,
            {
                "countryId": "HK",
                "date": "2014-10-22 10:03:04",
                "agentName": "Hong Kong",
                "agentId": 12,
                "roundId": 1413972000,
                "responseTime": 35,
                "totalSize": 7278,
                "numObjects": 4,
                "numErrors": 0,
                "domLoadTime": 171,
                "pageLoadTime": 205,
                "permalink": "https://app.thousandeyes.com/web/page-load?__a=75&testId=35199&roundId=1413972000&agentId=12"
            }
        ]
    },
    "pages": {
        "current": 1
    }
}
```