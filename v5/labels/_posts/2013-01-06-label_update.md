---
parent_category: labels
parent_category_label: Labels

path: '{{ site.version_url_prefix_request }}/groups/{groupId}/update'
title: 'Updating a label'
type: POST

sortorder: 6
category-sortorder: 7
layout: null
---

Updates a label (formerly called group) in ThousandEyes, based on properties provided in the POST data.  In order to edit a label, the user must have access to the target label, and have access to modify the objects that the label contains.  For example, to update an agent label, the user needs the Edit Agents permission assigned to their role.

Regular users are blocked from using any of the POST-based methods.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information
* `aid={aid}` optional and requires the user to be assigned to the target account group, specifies the account group context of the request, obtained from the `/account-groups` endpoint.  Specifying this parameter without the user being assigned to the target account will result in an error response. See [Account group context][overview-accountcontext] for more information

### Request

* `groupId` the label that you wish to update, found in either the `/groups` or the `/groups/{type}` endpoint.

### Response

Returns the label details, following update.  Metadata of the response is returned according to the following table:

Field | Data Type | Units | Notes
:------------|-------------|-------------|-------------|
name | string | n/a | Name of the label
groupId | integer | n/a | Unique ID of the label; this number is negative for built-in labels.  Query the `/groups/{id}` endpoint to see a list of agents/tests with this label.
type | string | n/a | either `tests` or `agents`, indicates the type of label.
builtin | integer | n/a | 1 for built-in labels, and 0 for user-created labels.  Note that built-in labels are read-only.
agents | array of agent objects | n/a | Agent objects are shown for an `agents` type label only.  See [Agent Metadata][agent-metadata] for more details on agent objects.
tests | array of test objects | n/a | Test objects are shown for a `tests` type label only.  See [Test Metadata][test-metadata] for more detail on test objects.

### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/groups/1356/update \
  -d '{ "name": "Dave's new group", "tests": [{"testId": 5048 },{"testId": 6600 }]}' \
  -H "Content-Type: application/json" \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

#### Header

```HTTP/1.1 200 OK
Server: nginx
Date: Tue, 23 Mar 2016 00:20:26 GMT
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Strict-Transport-Security: max-age=31536000
X-Server-Name: 1-5```

#### Body

```{
    "groups": [
        {
            "name": "Dave's new group",
            "groupId": 1356,
            "type": "tests",
            "builtin": 0,
            "tests": [
                {
                    "createdDate": "2014-06-13 04:01:07",
                    "modifiedDate": "2015-01-06 19:27:03",
                    "createdBy": "Dave Fraleigh (dave@thousandeyes.com)",
                    "modifiedBy": "Dave Fraleigh (dave@thousandeyes.com)",
                    "enabled": 1,
                    "savedEvent": 0,
                    "testId": 6600,
                    "testName": "Bozo BGP test 1",
                    "type": "bgp",
                    "prefix": "38.122.0.0/16",
                    "alertsEnabled": 0,
                    "liveShare": 0,
                    "includeCoveredPrefixes": 0,
                    "apiLinks": [...]
                },
                {
                    "createdDate": "2014-03-18 20:05:03",
                    "modifiedDate": "2015-07-31 17:20:54",
                    "createdBy": "Dave Fraleigh (dave@thousandeyes.com)",
                    "modifiedBy": "Dave Fraleigh (dave@thousandeyes.com)",
                    "enabled": 0,
                    "savedEvent": 0,
                    "testId": 5048,
                    "testName": "www.yahoo.com A",
                    "type": "dns-server",
                    "interval": 300,
                    "domain": "www.yahoo.com A",
                    "networkMeasurements": 1,
                    "mtuMeasurements": 1,
                    "bandwidthMeasurements": 0,
                    "bgpMeasurements": 1,
                    "alertsEnabled": 0,
                    "liveShare": 0,
                    "recursiveQueries": 1,
                    "dnsServers": [...],
                    "apiLinks": [...]
                }
            ]
        }
    ]
}```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
