---
title: Headers used by Custom Autosuggest API
titleSuffix: Bing Services
description: Describes the headers used by Bing Custom Autosuggest API requests and responses.
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-custom-autosuggest
ms.topic: reference
ms.date: 07/15/2020
ms.author: scottwhi
---

# Custom Autosuggest API v7 headers

The following are the headers that a request and response may include.  

> [!NOTE] 
> Remember that the Terms of Use require compliance with all applicable laws, including regarding use of these headers. For example, in certain jurisdictions, such as Europe, there are requirements to obtain user consent before placing certain tracking devices on user devices.
  

## Request headers

|Header|Required|Description|  
|-|-|-  
|<a name="subscriptionkey"></a>Ocp-Apim-Subscription-Key|Yes|The subscription key that you received when you signed up for this service in <a href="https://www.microsoft.com/cognitive-services/" target="_blank">Cognitive Services</a>.  


## Response headers
  
|Header|Description  
|-|-  
|<a name="traceid"></a>BingAPIs-TraceId|The ID of the log entry that contains the details of the request. When an error occurs, capture this ID. If you are not able to determine and resolve the issue, include this ID along with the other information that you provide the Support team.
|<a name="retryafter"></a>Retry-After|The response includes this header if you exceed the number of queries allowed per second (QPS) or per month (QPM). The header contains the number of seconds that you must wait before sending another request. 
