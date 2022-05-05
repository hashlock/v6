---
parent_category: overview
parent_category_label: Overview

title: 'Rate Limits'

sortorder: 7
category-sortorder: 1
layout: null
---

The ThousandEyes API throttles inbound API requests using a 240 request per minute, per organization limit.  The limit rolls over one minute from the first request in a batch, and starts again.  These values are subject to change, as we work through identifying appropriate use patterns for our API.

If you are receiving an HTTP/429 (Too many requests) response code, your request was refused on the basis of a rate limit. 

For error responses, see the [response status codes documentation][overview-responsestatuscodes].