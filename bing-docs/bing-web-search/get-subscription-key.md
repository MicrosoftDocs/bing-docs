---
title: Sign up for a subscription key
titleSuffix: Bing Search Services
description: Use Azure Marketplace to sign up for a Bing Search Service subscription and get your subscription key.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Sign up for a subscription key

To use Bing Search Service APIs, you need a subscription key. Here are the steps to sign up for a Bing Search Service subscription and get your subscription key.

1. Open a browser and navigate to <a href="https://azuremarketplace.microsoft.com/en-us/marketplace/" target="_blank">Azure Marketplace</a>.
1. Enter *Bing search* into the search box and press enter.
1. The search results will list the following Bing Search Service subscriptions that you can sign up for:  
   1. Bing Search
   1. Bing Custom Search
   1. Bing Entity Search
   1. Bing Autosuggest
   1. Bing Spell Check  
1. To learn more about the services that interest you and what they offer, click the service.
1. To get a subscription for the service, click **GET IT NOW**.  
   1. To get a subscription, you must be signed in to Azure with your Microsoft account. If you don't have an account, click **Sign up for a free account**.  
1. After agreeing with the terms of use and privacy policy, click **Continue**.
1. You can get a free trial by clicking **Start free**.
1. On the next splash screen, you have the option of continuing with the free trial (click **Start free** again) or buying the subscription now (click **Or buy now**). You can always start with the free trial and buy your subscription later.

## Free trial option

If you clicked **Start**, simply follow the sign up process.

1. The first step is to verify your identify. Enter a phone number. Choose to verify the phone number by using a text verification code or by receiving a phone call.
1. Second, verify your credit card number. You won't be charged during the trial period. Read the **No automatic charges** section in the right pane.  
   1. Add a payment method or click **Next** if Azure found a previous payment method for you.  
1. Check the box if you agree to the subscription agreement, offer details, and privacy statement.
1. Click **Sign up**.

> [!NOTE]
> You should be redirected to Azure Portal where you can create your subscription and get your subscription key. If Azure wasn't able to redirect you, go to <a href="https://portal.azure.com" target="_blank">Azure Portal</a> and sign in with your Microsoft account.

 
### On Azure portal

1. Type *Bing* in the search box.
1. Under **Marketplace**, select the Bing service you're interested in (for example, **Bing search**).
1. To create your subscription:  
   1. Enter a resource name. Names may contain alphanumeric characters and dashes (-) only. 
   1. The **Subscription** field should be set to **Free Trial**.
   1. In the **Pricing tier** dropdown, select **Free F1** package. The other packages are for the pay model. To view package options and pricing, click **View full pricing details**.
   1. If you have an existing resource group that you want to add this subscription to, select it from the **Resource group** dropdown list. Otherwise, click **Create new** to create a resource group.
   1. Select a location from the **Resource group location** dropdown. The location is where the metadata associated with your account resides and has no impact on runtime availability.
   1. Check the box that indicates that you have read and understood the notice.
   1. Click **Create**. This starts the deployment process, which can take several minutes.
   1. When the deployment process completes, click **Go to resource**.
   1. To get your subscription key to use in API calls, click **Keys and Endpoint** in the left pane.  


## Next steps

- Learn about [calling the Bing Web Search API](search-the-web.md).
- Learn about the [quickstarts](quickstarts/quickstarts.md) and [samples](samples.md) that are available to help you get up and running fast.
- Review [Web Search API v7 reference](reference/endpoints.md) documentation.  

