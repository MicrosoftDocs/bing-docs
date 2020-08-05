---
title: Bing Custom Image Search API v7 Reference
titleSuffix: Bing Services
description: Describes the programming elements of Bing Custom Image Search API.
ms.service: bing-search-services
ms.subservice: bing-custom-image-search
author: swhite-msft
manager: ehansen
ms.topic: reference
ms.date: 07/15/2020
ms.author: scottwhi
---

# Custom Image Search API v7 reference

The Custom Image Search API lets you send a search query to Bing and get back a list of relevant images from the slice of Web that your Custom Search instance defines. For information about configuring a Custom Search instance, see [Configure your custom search experience](../../bing-custom-search/how-to/define-your-custom-view.md). 
  
For information about permitted use and display of the results, see [Use and display requirements](../../bing-web-search/use-display-requirements.md).

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.
  
## Endpoints  

To request image search results, send a GET request to:  
  
```
https://api.bing.microsoft.com/bingcustomsearch/v7.0/images/search
```

The request must use the HTTPS protocol. 

> [!NOTE]
> The maximum URL length is 2,048 characters. To ensure that your URL length does not exceed the limit, the maximum length of your query parameters should be less than 1,500 characters. If the URL exceeds 2,048 characters, the server returns 404 Not found.  
  
## Next steps

Check out the following programming elements you'll use when sending search requests:

- [Headers](headers.md)
- [Query parameters](query-parameters.md)
- [Response objects](response-objects.md)
