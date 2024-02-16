---
title: Resize and crop image thumbnails - Bing Web Search API
titleSuffix: Bing Search Services
description: Some answers from the Bing Search APIs include URLs to thumbnail images served by Bing, which you can resize and crop, and may contain query parameters.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 02/15/2024
ms.author: v-alpunnamar
---

# Resize and crop thumbnail images

> [!NOTE]
> Be sure when cropping and resizing thumbnail images that you're doing so in accordance with a search scenario that respects third party rights, as required by your Bing Search API [use and display requirements](use-display-requirements.md).

Some answers include URLs to thumbnail images served by Bing. The following examples show several thumbnail URL formats that you might find in an answer.

```curl
https://<host>/th?id=ON.A317B1375C1ADD5C646CB8635AE4E9&pid=News
https://<host>/th?id=OVP.VC36z4V1MoxzlwETyoaQHgFo&pid=Api
https://<host>/th?id=AMMS_c1a785119b4d9fc14b6571a2a2f728&w=110&h=110&c=7&rs=1&qlt=80&cdv=1&pid=16.1
```

You may resize and crop thumbnail images. To resize a thumbnail:

1. Remove all query parameters except the *id* and *pid* parameters.
2. Add only the *w* (width) or *h* (height) query parameter, but not both.
3. Set the *w* or *h* query parameter to the desired size, in pixels. Bing maintains the aspect ratio for you.

If you specify an image size that’s larger than the thumbnail’s original size, Bing adds white padding around the image as needed. For example, if the image’s original size is 474x316 and you set *w* (width) to 500, Bing returns a 500x333 image. Bing pads the top and bottom edges with 8.5 pixels of white padding and the left and right edges with 13 pixels of white padding.

To prevent Bing from adding white padding if the requested size is greater than the thumbnail’s original size, include the *p* query parameter. For example, if you include the *p* parameter in the above example, Bing returns a 474x316 image instead of a 500x333 image. Set the *p* parameter to 0 (zero).

```curl
https://<host>/th?id=AMMS_92772df988...&w=500&p=0&pid=16.1
```

If you specify both the *w* and *h* query parameters, Bing maintains the thumbnail’s aspect ratio and adds white padding as needed. For example, if the thumbnail’s original size is 474x316 and you set the width and height parameters to 200x200 (&w=200&h=200), Bing returns an image that contains 33 pixels of white padding on the top and bottom. If you include the *p* query parameter, Bing returns an image that’s 200x134.

## Cropping a thumbnail image

To crop an image, include the *c* (crop) query parameter. The following are the possible values that you may specify.

- `4` &mdash; Blind Ratio  
- `7` &mdash; Smart Ratio  

### Requesting smart ratio cropping

If you request Smart Ratio cropping (c=7), Bing crops the image from the center of the image’s region of interest outward, while maintaining the image’s aspect ratio. The region of interest is the area of the image that Bing determines contains the most important part. The following shows an example region of interest.

![Region of interest](media/resize-crop/bing-resize-crop-regionofinterest.png)

If Bing cannot determine the image’s region of interest, Bing uses [Blind Ratio cropping](#requesting-blind-ratio-cropping).

Here's the original image used in the following examples.

![Original landscape image](media/resize-crop/bing-resize-crop-landscape.png)

Here's what it looks like if you resize the image to 200x200 using Smart Ratio cropping.
  
![Landscape image cropped to 200x200](media/resize-crop/bing-resize-crop-landscape200x200c7.png)
  
Here's what it looks like if you resize the image to 200x100 using Smart Ratio cropping.

![Landscape image cropped to 200x100](media/resize-crop/bing-resize-crop-landscape200x100c7.png)
  
Here's what it looks like if you resize the image to 100x200 using Smart Ratio cropping.
  
![Landscape image cropped to 100x200](media/resize-crop/bing-resize-crop-landscape100x200c7.png)

### Requesting blind ratio cropping

If you request Blind Ratio cropping (c=4), Bing uses the following rules to crop the image.

- If (Original Image Width / Original Image Height) < (Requested Image Width / Requested Image Height), Bing measures the image from its top left corner and crops it at the bottom.  
  
- If (Original Image Width / Original Image Height) > (Requested Image Width / Requested Image Height), Bing measures the image from the center and crops it to the left and right.

Here's the original image used in the following examples.

![Original sunflower image](media/resize-crop/bing-resize-crop-sunflower.png)
  
Here's what it looks like if you resize the image to 200x200 using Blind Ratio cropping.
  
![Sunflower image cropped to 200x200](media/resize-crop/bing-resize-crop-sunflower200x200c4.png)
  
Here's what it looks like if you resize the image to 200x100 using Blind Ratio cropping.
  
![Sunflower image cropped to 200x100](media/resize-crop/bing-resize-crop-sunflower200x100c4.png)
  
Here's what it looks like if you resize the image to 100x200 using Blind Ratio cropping.
  
![Sunflower image cropped to 100x200](media/resize-crop/bing-resize-crop-sunflower100x200c4.png)
