---
author: swhite-msft
ms.author: scottwhi
ms.service: bing-web-services
ms.topic: include
ms.date: 07/15/2020
---

The service and your subscription type determine the number of queries per second (QPS) that you can make. Make sure your application includes the logic to stay within your quota. If the QPS limit is met or exceeded, the request fails and the service returns an HTTP 429 status code. The response includes the `Retry-After` header, which indicates how long you must wait before sending another request.

## Denial-of-service versus throttling

The service makes a differentiation between a denial-of-service (DoS) attack and a QPS violation. If the service suspects a DoS attack, the request succeeds (HTTP status code is 200 OK). However, the body of the response is empty.