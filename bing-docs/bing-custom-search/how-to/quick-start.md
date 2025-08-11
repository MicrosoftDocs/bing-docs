---
title: "Quickstart: Create your first Bing Custom Search instance"
titleSuffix: Bing Search Services
description: Use this quickstart to create a custom Bing instance that can search the domains and webpages that you specify. 
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---
# Quickstart: Create your first Bing Custom Search instance

To use Bing Custom Search, create a custom search instance that defines your view or slice of the web. The instance contains the public domains, websites, and webpages that you want to search, along with any ranking adjustments you may want.

<!--
![A picture of the Bing Custom Search portal](../media/blockedCustomSrch.png)
-->

## Prerequisites

To use Bing Custom Search, you need to define an Azure Bing Custom Search resource and get your key. For details, see [Create Bing Search Service resource](../../bing-web-search/create-bing-search-service-resource.md).

## Create a custom search instance

To create a Bing Custom Search instance:

1. Open a browser and navigate to the [Bing Custom Search portal](https://customsearch.ai).  

1. If you're new to Bing Custom Search, click **Get Started**, accept the terms, and sign in with your Microsoft account.

   1. The UX displays a carousel with Custom Search features. Thumb through the features and click the exit button ('X' in the top-right corner of the carousel).

   1. To create your first instance, click **Create new instance**.

1. Otherwise, click **My Instances** and then click **New Instance**.  

1. Enter a descriptive instance name and click **Ok**. You can change the name of your instance at any time.

1. On the **Active** tab under **Search Experience**, enter the URL of one or more websites you want to include in your search.

   > [!NOTE]
   > Bing Custom Search instances will only return results for domains and webpages that are public and have been indexed by Bing.  

   1. After adding a URL, the UX displays a list of websites **You might want to add**. Select sites from the list as appropriate; otherwise, enter another website URL where it says **Type in a URL**.  
  
1. To **Try out the search experience**, enter a search string in the right Preview pane. If no results are returned, try entering a different URL.  

1. If all is well, click **Publish** to publish your changes to the production environment. After your instance is published, click **Go to production environment**.

1. Copy your **Custom Configuration ID**, the query parameter to which you set the *customConfig* when calling Custom Search API.

   > [!NOTE]
   > To use the instance in production, you must have a subscription key.

## Next steps

- To get full details about creating an instance, see [Define a custom view](define-your-custom-view.md).
