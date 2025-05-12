---
title: Get images from your custom view
titleSuffix: Bing Search Services
description: High-level overview about using Bing Custom Search to get images from your custom view of the Web.
services: bing-search-services
author:  swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---
> [!WARNING] 
> **Product to be retired** Bing Search and Bing Custom Search APIs will be retired on 11th August 2025. 
> New deployments are not available and existing resources will be disabled. [Learn more](https://aka.ms/BingAPIsRetirement)
<br/>

# Get images from your custom view

Bing Custom Images Search lets you enrich your custom search experience with images. Similar to web results, custom search supports searching for images in your view of the Web. To get images, use Custom Image Search API or enable images in your Hosted UI. We recommend using the Hosted UI feature for getting your search experience up and running in short order. For information about configuring your Hosted UI to include images, see [Configure your hosted UI experience](hosted-ui.md).

If you want more control over displaying the search results, use Custom Image Search API. If you're familiar with [Bing Image Search API](../../bing-image-search/overview.md), then you will be able to easily call Custom Image Search API. The main differences are 1) the endpoint you send requests to, and 2) you must include the *customConfig* query parameter.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/custom/images/search?q=mt+rainier&customConfig=<your configuration ID> 
```
