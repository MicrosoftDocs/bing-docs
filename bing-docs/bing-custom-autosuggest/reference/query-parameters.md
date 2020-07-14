---
title: Query parameters used by Custom Autosuggest API
titleSuffix: Bing Services
description: Describes the query parameters that you can use in Bing Custom Autosuggest API requests to affect the results.
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-custom-autosuggest
ms.topic: reference
ms.date: 07/15/2020
ms.author: scottwhi
---

# Custom Autosuggest API v7 query parameters

The following are the query parameters that the request may include. The Required column indicates whether you must specify the parameter. You must URL encode the query parameter values.  
  
|Name|Value|Type|Required 
|-|-|-|-
|<a name="customconfig"></a>customConfig|Unique identifier that identifies your custom search instance.|String|Yes
|<a name="query"></a>q|The user's search query string.<br/><br/>The query string must not be empty. If empty or not specified, the list of suggestions in the response is empty.<br/><br/>The API does not support the [Bing Advanced Operators](https://help.bing.microsoft.com/#apex/18/en-US/10001/-1). If the query string includes Bing operators, the operators are treated as part of the query string, not as an operator.|String|Yes 
  
