---
title: Create Bing Search Services Resource
titleSuffix: Bing Search Services
description: Use Azure Marketplace to sign up for a Bing Search Service and get your key.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 02/15/2024
ms.author: v-alpunnamar
---

# Create Bing Search resource through Azure Marketplace

Here are the steps to create a Bing Search Service resource through Azure Marketplace and get your key.

1. Go to <a href="https://portal.azure.com" target="_blank">Azure Portal</a> and sign in with your Microsoft account. If you don't have a Microsoft account, click **Create one!**.
1. From the portal, type *Bing* in the search box.
1. Under **Marketplace** in the search results, select the Bing service you're interested in (for example, **Bing Search** or **Bing Custom Search**).
1. If you have a free trial or pay account, skip to [Create your Bing resource](#create-your-bing-resource).
1. On the **Create a free account** splash screen, click **Start free**.
1. Next, you have the option of continuing with the free trial (click **Start free** again) or paying for an Azure subscription (click **Or buy now**). You can always start with the free trial and pay for a subscription later.

## Free trial option

If you clicked **Start free**, simply follow the sign up process.

1. Provide your name and phone number. The process includes this step only if your Microsoft account profile doesn't include your name and phone number.
1. Next, verify your identify and phone number. Enter a phone number, if it's not already set. Choose to verify the phone number by using a text verification code or by receiving a phone call.
1. Add your credit card information and click **Next**. Don't worry, you won't be charged during the trial period. Read the **No automatic charges** section in the right pane.  
1. Click the check box if you agree to the subscription agreement, offer details, and privacy statement.
1. Finally, click **Sign up**.

You should be redirected back to Azure Portal where you can create a resource and get your key. If Azure wasn't able to redirect you, go to <a href="https://portal.azure.com" target="_blank">Azure Portal</a> and sign in with your Microsoft account. Back in the portal, type *Bing* in the search box. Under **Marketplace** in the search results, select the Bing service you're interested in (for example, **Bing Search** or **Bing Custom Search**).

## Create your Bing resource

The following steps walk you through creating a Bing Search resource:  

  >[!NOTE]
  > While creating Bing Custom Search resource ensure **Bing Custom Search** is selected as top level service and not "Bing Search".
  >
  > :::image type="content" source="media/bing-web-api/bing-custom-search-create.png" alt-text="Bing Custom Search Create breadcrumb":::

1. Enter a resource name. Names may contain alphanumeric characters and dashes (-) only.
1. The **Subscription** field could be set to **Free Trial** or select your appropriate subscription.
1. In the **Pricing tier** dropdown, select **Free F1** package. The other packages are for the pay model. To view package options and pricing for the pay model, click **View full pricing details**.
1. If you have an existing resource group that you want to add this resource to, select it from the **Resource group** dropdown list. Otherwise, click **Create new** to create a resource group.
1. Select a location from the **Resource group location** dropdown. The location is where the metadata associated with your account resides and has no impact on runtime availability.
1. Click the check the box to indicate that you have read and understood the notice.
1. Click **Create**. This starts the deployment process, which can take several minutes.
1. When the deployment process completes, click **Go to resource**.
1. To get your subscription key to use in API calls, click **Keys and Endpoint** in the left pane.  

:::image type="content" source="media/bing-web-api/bing-create-blade-image.png" alt-text="Bing Search Create Blade experience":::

## Next steps

- Learn about [calling the Bing Web Search API](search-the-web.md).
- Learn about the [quickstarts](quickstarts/quickstarts.md) and [samples](samples.md) that are available to help you get up and running fast.
- Review [Web Search API v7 reference](reference/endpoints.md) documentation.  
