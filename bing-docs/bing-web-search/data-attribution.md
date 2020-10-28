---
title: Attributing search results 
titleSuffix: Bing Search Services
description: Some Bing search results require attribution. This topic shows how to provide attribution when you display search results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Attributing search results

While Bing gathers some information by crawling the web, other information is obtained via licensors who place restrictions on the use of their data. It is important to understand that while Bing organizes and makes certain inferences or connections with the data, Bing is, in most cases, not the owner of this data. As such, partners must ensure that they adhere to fair use, copyright or other restrictions that may exist on the source data.

If any part of an answer includes the `contractualRules`, `attributions`, or `provider` fields, you must attribute the data. If the answer does not include any of these fields, no attribution is required. If the answer includes the `contractualRules` field, you must use it to attribute the data instead of `attributions` or `provider`.
 

## What data does a contractual rule apply to?

If the contractual rule includes the `targetPropertyName` field, you must apply the rule to the data in the field that `targetPropertyName` points to. Otherwise, the rule applies to the data in the object that contains the `contractualRules` field. 

Because the **LinkAttribution** and **TextAttribution** rules in the following example fragment don’t include `targetPropertyName`, the rules apply to the entity object. In this case, you must include a line immediately following the entity data that attributes the data to the providers. The line should be clearly labeled to indicate that the providers are the source of the data. For example, "Data from: Wikipedia | STATS LLC © 2016". 

For **LinkAttribution** rules, you must create a hyperlink to the provider's website. In this case, make Wikipedia a hyperlink to the Wikipedia webpage that contains information about the entity.

```json
"entities" : {  
  "id" : "https://<host>/api/v7.0/search/#Entities",  
  "queryScenario" : "DominantEntity",  
  "value" : [
    {  
      "contractualRules" : [  
        {  
          "_type" : "ContractualRules/LinkAttribution",  
          "text" : "Wikipedia",  
          "url" : "http://www.bing.com/cr?IG=B8AD7..."  
        },  
        {  
          "_type" : "ContractualRules/TextAttribution",  
          "text" : "STATS LLC © 2016"  
        }
      ]   
```

But in the following example, the **MediaAttribution** contractual rule identifies the image as the target of the rule (see `targetPropertyName`), so you must attribute the source of the image when you display it. For rules that apply to specific fields, you must include a line immediately following the targeted data that contains a hyperlink to the provider's website.

```json
"contractualRules" : [  
  {  
    "_type" : "ContractualRules/MediaAttribution",  
    "targetPropertyName" : "image",  
    "mustBeCloseToContent" : true,  
    "url" : "http://fourthcoffee.com/bphoto/rxQuLbj-UHbYXZZ2xppA/l.jpg"  
  }
],  
"name" : "Fourth Coffee",  
"url" : "http://www.fourthcoffee.net/",  
"image" : {  
  "provider" : [
    {  
      "name" : "Fabrikam",  
      "url" : "https://www.fabrikam.com/biz/fourth-coffee-bellevue?adjust_creative=bing..."  
    }
  ],  
  "contentUrl" : "https://www.bing.com/th?id=ArrJtlvND71ozg480x360&p=0&pid=Local",  
  "hostPageUrl" : "http://fourthcoffee.com/bphoto/rxQuLbj-UHbYXZZ2xpp/l.jpg",  
  "width" : 400,  
  "height" : 400  
},  
```

The above example also shows data that includes both the `contractualRules` and `provider` fields. Because `contractualRules` exists, you ignore the image's `provider` field and use the **MediaAttribution** rule to provide attribution for the image.


## Contractual rules

For information about possible contractual rules that a Bing API response may contain, see the following sections.

- [License attribution](#license-attribution)
- [Link and text attribution](#link-and-text-attribution)
- [Media attribution](#media-attribution)


### License attribution

If the list of contractual rules includes a **LicenseAttribution** rule, you must display the notice on the line immediately following the content that the license applies to. The **LicenseAttribution** rule uses the `targetPropertyName` field to identify the property that the license applies to.

The following shows an example that includes a **LicenseAttribution** rule.

![License attribution](media/attribution/licenseattribution.png)  

The license notice that you display must include a hyperlink to the website that contains information about the license. Typically, you make the name of the license a hyperlink. For example, if the notice is *Text under CC-BY-SA license* and *CC-BY-SA* is the name of the license, make *CC-BY-SA* a hyperlink.

### Link and text attribution

The **LinkAttribution** and **TextAttribution** rules are typically used to identify the data provider. If the rule doesn’t include the `targetPropertyName` field, the rule applies to the parent object that encapsulates the rule; otherwise, the rule applies to the targeted field.

To attribute the provider, include a line immediately following the content that the attribution applies to (for example, the parent object or targeted field). The line should be clearly labeled to indicate that the provider is the source of the data. For example, "Data from: Wikipedia | STATS LLC © 2016". For **LinkAttribution** rules, you must create a hyperlink to the provider's website.

The following shows an example that includes **LinkAttribution** and **TextAttribution** rules.

![Link and Text attribution](media/attribution/linktextattribution.png)  

### Media attribution

If the entity includes an image and you display it, you must provide a click-through link to the provider's website. If the entity includes a **MediaAttribution** rule, use the rule's URL to create the click-through link. Otherwise, use the URL included in the image's `provider` field to create the click-through link.

The following example includes a contractual rule and the `provider` field. Because the example includes the contractual rule, ignore the `provider` field and apply the **MediaAttribution** rule.

![Media attribution](media/attribution/mediaattribution.png)  



