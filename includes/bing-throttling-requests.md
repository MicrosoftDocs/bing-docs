---
author: swhite-msft
ms.service: bing-search-services
ms.topic: include
ms.date: 07/15/2020
ms.author: scottwhi
---

The service and your subscription type determine the number of queries per second (QPS) that you can make. Make sure your application includes the logic necessary to stay within your quota. If the QPS limit is met or exceeded, the request fails and returns an HTTP 429 status code. The response includes the `Retry-After` header, which indicates how long you must wait before sending another request.

## Denial-of-service versus throttling

Bing distinguishes between a denial-of-service (DoS) attack and a QPS violation. If Bing suspects a DoS attack, the request succeeds (HTTP status code is 200 OK) but the body of the response is empty.
