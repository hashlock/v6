---
parent_category: agents
parent_category_label: Agents & Monitors

path: '{{ site.version_url_prefix_request }}/agents/{agentId}/delete'
title: 'Deleting an Agent'
type: POST

sortorder: 4
category-sortorder: 5
layout: null
---

Deletes an Enterprise Agent from ThousandEyes.  Note: this feature can only be used on Enterprise Agents.

Important notes related to agent removal:
* if an agent is deleted, the modification date for tests using that agent at the time it was deleted will be changed.
* If a deleted agent is the final remaining agent on a test, then the test will be disabled when the agent is removed.

Important note: if an agent is removed, it must be re-initialized to use the same machine again in different context.  Virtual Appliances can be updated using the Reset State button in the Advanced tab of the agent management interface.  Users running packaged versions of Linux will need to remove `/var/lib/te-agent/\*.sqlite` in order to reinitialize an agent.

### Optional Parameters

* `format=json|xml` optional, specifies the format of output requested.  See [Output Formats][overview-outputformats] for more information

### Request

* `{agentId}` corresponds the unique ID of an enterprise agent, obtained from the `/agents` endpoint

### Post Data

{.inline-code}When POSTing data to the `/agents/{agentId}/delete` endpoint, users should specify an empty POST body.

### Example

`$curl https://api.thousandeyes.com{{ site.version_url_prefix_request }}/agents/966/delete \
  -d '' \
  -H "Content-Type: application/json" \
  -u noreply@thousandeyes.com:g351mw5xqhvkmh1vq6zfm51c62wyzib2`

### Response

If an agent is successfully deleted, an HTTP/204 No Content response will be returned, and an empty JSON response will be in the body of the response.

#### Header

```HTTP/1.1 206 No Content
Date: Thu, 14 May 2015 23:18:04 GMT
Server: Apache
Vary: Accept-Encoding
Strict-Transport-Security: max-age=31536000
X-Frame-Options: sameorigin
Transfer-Encoding: chunked
Content-Type: text/xml```

#### Body

```
```

For error responses, see the [response status codes documentation][overview-responsestatuscodes].
