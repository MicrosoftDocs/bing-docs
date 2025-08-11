---
title: Get videos from your Custom Search instance
titleSuffix: Bing Search Services
description: High-level overview about using Bing Custom Search to get videos from your custom view of the Web.
services: bing-search-services
author:  swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---
# Get videos from your custom view

Bing Custom Video Search lets you enrich your custom search experience with videos. Similar to web results, Custom Search supports searching for videos in your view of the Web. To get videos, use Custom Video Search API or enable videos in your Hosted UI. Using the Hosted UI feature is simple and recommended for getting your search experience up and running in short order. For information about configuring your Hosted UI to include videos, see [Configure your hosted UI experience](hosted-ui.md).

If you want more control over displaying the search results, use Custom Video Search API. If you're familiar with [Bing Video Search API](../../bing-video-search/overview.md), then you will be able to easily call Custom Video Search API. The main differences are 1) the endpoint you send requests to, and 2) you must include the *customConfig* query parameter.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/custom/videos/search?q=sailing+dinghies&customConfig=<your configuration ID>
```
