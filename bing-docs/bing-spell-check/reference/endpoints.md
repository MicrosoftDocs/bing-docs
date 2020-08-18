---
title: Bing Spell Check API v7 Reference
titleSuffix: Bing Services
description: Describes the programming elements of Bing Spell Check API.
ms.service: bing-search-services
ms.subservice: bing-spell-check
author: swhite-msft
manager: ehansen
ms.topic: reference
ms.date: 07/15/2020
ms.author: scottwhi
---

# Spell Check API v7 reference

The Spell Check API lets you check a text string for spelling and grammar errors. This section provides technical details about the query parameters and headers that you use to request spell checking, and the JSON response objects that contain the results. For examples that show how to make requests, see [Bing Spell Check overview](../overview.md). 
  
For information about permitted use and display of the results, see [Use and display requirements](../../bing-web-search/use-display-requirements.md).

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.
  
## Endpoints  

To check the spelling and grammar of a block of text, send a GET or POST request to:  
  
```
https://api.bing.microsoft.com/v7.0/spellcheck
```  
  
The request must use the HTTPS protocol.  
  
Because of URL length limits, you typically use a POST request unless you're checking only short strings.  

> [!NOTE]
> The maximum URL length is 2,048 characters. To ensure that your URL length does not exceed the limit, the maximum length of your query parameters should be less than 1,500 characters. If the URL exceeds 2,048 characters, the server returns 404 Not found.  
  
## Next steps

Check out the following programming elements you'll use when sending requests:

- [Headers](headers.md)
- [Query parameters](query-parameters.md)
- [Response objects](response-objects.md)
