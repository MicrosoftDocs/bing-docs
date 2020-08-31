---
title: Configure your Bing Custom Search instance
titleSuffix: Bing Search Services
description: The portal lets you create a search instance that specifies the slices of the web (domains, subpages, and webpages) that you want to search.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Configure your Bing Custom Search instance

A Custom Search instance lets you tailor the search experience to include content only from websites that your users care about. Instead of performing a Web-wide search, you specify the domains and webpages that Bing searches. You can also pin, boost, block, and demote specific content to create a custom view of the web that helps your users quickly find relevant content. To create your custom view, use the [Bing Custom Search portal](https://customsearch.ai).


## Terminology

The documentation uses the following terminology for defining different slices of the web:

|Slice name|Description
|-|-
|Domain|A domain slice specifies an Internet domain such as `www.microsoft.com`. Bing searches all content found under the domain. Omitting `www.` causes Bing to search the domain’s subdomains. For example, if you specify `microsoft.com`, Bing also returns results from `support.microsoft.com` or `technet.microsoft.com`.
|Subpage|A subpage slice specifies a domain path. Bing searches all content found at and below the path. You may specify a maximum of two sub-folders in the path. For example, `www.microsoft.com/en-us/windows/`.
|Webpage|A webpage slice specifies a specific webpage. Bing searches only that webpage. You can optionally specify whether to include subpages.

> [!IMPORTANT]
> All domains, subpages, and webpages that you specify must be public and indexed by Bing. If you own a public site that you want to include in the search but Bing hasn’t indexed it, see the [Bing webmaster documentation](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) for details about getting your site indexed. The webmaster documentation also provides details about getting Bing to crawl your site if the index is out of date.


## Get started by creating a custom search instance

1. Open a browser and navigate to the [Bing Custom Search portal](https://customsearch.ai).  
   
1. If you're new to Bing Custom Search, click **Get Started**, accept the terms, and sign in with your Microsoft account. 

   1. The UX displays a carousel with Custom Search features. Thumb through the features and click the exit button ('X' in the top-right corner of the carousel).
  
   1. To create your first instance, click **Create new instance**. 
  
1. Otherwise, click **My Instances** and then click **New Instance**.  
  
1. Enter a descriptive instance name and click **Ok**. You can change the name of your instance at any time.


## Add slices of the web to your custom search instance

After creating a custom search instance, specify the slices of the web that you want Bing to search. You specify slices on the **Active** tab under **Search Experience**.

Use the following options to specify a slice.  

1. Enter the URL of one or more websites you want to include in your search. Enter the website URL where it says **Type in a URL**. Enter a:
   - Domain (for example, https:\//www.microsoft.com)
   - Domain and path (for example, https:\//www.microsoft.com/en-us/surface)
   - Webpage (for example, https:\//www.microsoft.com/en-us/p/surface-earbuds/8r9cpq146064?activetab=pivot%3aoverviewtab)
   
   > [!NOTE]
   > Bing Custom Search only returns results for domains and webpages that are public and have been indexed by Bing.
   
2. Let Bing find websites for you to include based on a search string.  
   
   1. In the right Preview pane under the search box, click **My Instance** and select Bing. 
   1. Enter a search string in the search box.
   1. Scroll through the search results. If you see a site you want to add to your instance, click **Add site**. 
   1. Specify which part of the URL you want to add.
  
3. Select a suggested website from the **You might want to add** list by clicking the '+' sign. Bing generates the list after you enter a URL in the **Active** list. Click **Refresh** to get updated suggestions after updating your custom search instance's settings. This section is only visible if suggestions are available.
  
4. Upload a list of URLs by uploading a text file using the upload icon (see the cloud icon). To upload a file, create a text file and specify a single domain, subpage, or webpage per line. Bing rejects the file if it isn't formatted correctly and ignores duplicate entries.

To edit or delete URLs, use the options under the **Controls** column. 


## Block slices of the web from your custom search instance

To explicitly block content from websites, specify the slices of the web that you don't want Bing to search. You specify slices on the **Blocked** tab under **Search Experience**.

Use the following options to specify a slice.  

1. Enter the URL of one or more websites you want to block from your search. Enter the website URL where it says **Type in a URL**. Enter a:
   - Domain (for example, https:\//www.microsoft.com)
   - Domain and path (for example, https:\//www.microsoft.com/en-us/surface)
   - Webpage (for example, https:\//www.microsoft.com/en-us/p/surface-earbuds/8r9cpq146064?activetab=pivot%3aoverviewtab)
   
2. Let Bing find websites for you to include based on a search string.  
   
   1. In the right Preview pane under the search box, click **My Instance** and select Bing. 
   1. Enter a search string in the search box.
   1. Scroll through the search results. If you see a site you want to add to the blocked list, copy the URL from the results. You can't click **Add site**; clicking **Add site** adds the URL to the **Active** list. 
  
3. Select a suggested website from the **You might want to add** list by clicking the '+' sign. Bing generates the list after you enter a URL in the **Active** list. Click **Refresh** to get updated suggestions after updating your custom search instance's settings. This section is only visible if suggestions are available.  
  
4. Block an entry in the **Active** list. If the **Active** list contains a slice that you no longer want Bing to search, click the block icon (circle with a line through it) in the **Controls** column. The slice is moved from the **Active** list to the **Blocked** list.  
  
5. Block a webpage listed in the search results. Enter a search query in the **Preview** pane on the right. For each webpage in the search result, you have the option to block the webpage from future searches. To block the webpage, click **Block**. Specify which part of the URL you want to block.
  
To edit or delete URLs, use the options under the **Controls** column. 

> [!NOTE]
> * You can only upload a file to the **Active** list. You cannot use it to add slices to the **Blocked** list.  
> * If the **Blocked** list contains a domain, subpage, or webpage that you specified in the upload file, Bing removes it from the **Blocked** list and adds it to the **Active** list.


## Boosting and demoting search results

You can super boost, boost, or demote any domain or subpage in the **Active** list. By default, all slices are added with no ranking adjustments. Slices of the web that are Super boosted or Boosted are ranked higher in the search results (with super boost ranking higher than boost). Items that are demoted are ranked lower in the search results.

You can super boost, boost, or demote items by using the **Ranking Adjust** controls in the **Active** list, or by using the Boost and Demote controls in the **Preview** pane. If you use the controls in the **Preview** pane, the service adds the slice to your **Active** list and marks it as **Boosted**.

> [!NOTE] 
> Boosting and demoting domains and subpages is one of many methods Bing Custom Search uses to determine the order of search results. Because of other factors influencing the ranking of different web content, the effects of adjusting rank may vary. Use the Preview pane to test the effects of adjusting the rank of your search results. 

Super boost, boost, and demote are not available for image and video searches.


## Pin slices to the top of search results

Another option that affects ranking is pinning URLs to the top of the search results. The **Pinned** tab under **Search Experience** contains the list of pinned webpages. You may pin a maximum of one webpage per search string.

You can specify pinned webpages in the following ways:

- In the **Pinned** tab, enter the URL of the webpage to pin to the top, and its corresponding search string.

- In the **Preview** pane, enter a search string. Find the webpage you want to pin for that search string and click **Pin to top**. The service adds the webpage and search string combination to the **Pinned** list.

Pinning results is not available for image or video searches.

### Specify the pin's match condition

By default, webpages are only pinned to the top of the search results when a user's search string exactly matches one of the pinned search strings. You can change this behavior by specifying one of the following match conditions:

> [!NOTE]
> All comparisons between the user's search query, and the pin's search query are case insensitive.

|Value|Description
|-|-
|Starts with|The pin is a match if the user's search string starts with the pin's search string
|Ends with|The pin is a match if the user's search string ends with the pin's search string.
|Contains|The pin is a match if the user's search string contains the pin's search string.

To change the pin's match condition, click the pin's edit icon (pencil). In the **Query match condition** column, click the dropdown list and select the new condition to use. Then, click the save icon to save the change.

### Change the order of your pinned sites

To change the order of your pins, you can drag-and-drop the them, or edit their order number by clicking the edit icon (pencil) in the **Controls** Column of the **Pinned** list.

If multiple pins satisfy a match condition, Bing Custom Search will use the one highest in the list.


## Publish or revert a search instance

Custom search has two environments: staging/testing (see the **Configuration** tab) and production (see the **Production** tab). When you create a new instance or make changes to an existing instance, those changes occur in the testing environment.

After you've validated your changes and are ready to publish, click **Publish**. Changes are not reflected against your production endpoints until you publish.

Before publishing, if you decide that you don't want to keep the changes you've made, click **Revert**. When you revert your changes, the **Published** version remains unchanged and the **Configuration** version is reverted to match the **Published** version.


## Search for images and videos

The search results include any images or videos that Bing found that are relevant. If you want to get only images or only videos, use [Bing Custom Image Search API](../../bing-custom-image-search/overview.md) or [Bing Custom Video Search API](../../bing-custom-video-search/overview.md), respectively. 


## Test your search instance with the Preview pane

You can test your search instance by using the preview pane on the portal's right side to submit search queries and view the results. 

1. Below the search box, select **My Instance**. You can compare the results from your search experience to Bing, by selecting **Bing**. 
2. Select a safe search filter and which market to search (see [Query Parameters](../reference/query-parameters.md)).
3. Enter a query and press enter or click the search icon to view the results from the current configuration. You can change your search type you perform by clicking **Web**, **Image**, or **Video** to get corresponding results. 


## View statistics

If you subscribed to Custom Search at the appropriate level, the UX adds a **Statistics** tab to your production instances. The statistics tab shows details about how your Custom Search endpoints are used, including call volume, top queries, geographic distribution, response codes, and safe search. You can filter details using the provided controls.


## Usage guidelines

- For each custom search instance, the maximum number of ranking adjustments that you may make to **Active** and **Blocked** slices is limited to 400.
- Adding a slice to the **Active** or **Blocked** tabs counts as one ranking adjustment.
- Boosting and demoting counts as two ranking adjustments.
- For each custom search instance, the maximum number of pins that you may make is limited to 200.


## Next steps

- Learn how to [use Custom Search API to search your instance](search-your-custom-view.md).
- Learn how to [configure a hosted UI experience](hosted-ui.md).
- Learn how to [share your search instance](share-your-custom-search.md) with teammates.
