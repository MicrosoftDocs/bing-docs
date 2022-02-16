---
title: How to use decoration markers to highlight text
titleSuffix: Bing Search Services
description: Learn how to use text decorations and hit highlighting in your search results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Using decoration markers to highlight text

Hit highlighting is when Bing highlights words or phrases from the user’s search string that were found in search result strings. Bing uses either Unicode characters or HTML tags to mark the words or phrases in the webpage’s name, display URL, and snippet text. Bing may mark other terms that Bing finds relevant.

By default, Bing doesn't highlight words or phrases in display strings. To enable hit highlighting, set the `textDecorations` query parameter in your request to **true**.

To specify whether you want Bing to use Unicode characters or HTML tags to mark the words or phrases, set the `textFormat` query parameter one off the following possible values.

- Raw &mdash; Uses Unicode characters to mark content that needs special formatting. The Unicode characters are in the range E000 through E019. For example, Bing uses E000 and E001 to mark the beginning and end of words or phrases for hit highlighting.

- HTML &mdash; Uses HTML tags to mark content that needs special formatting. For example, Bing uses \<b> tags to mark the beginning and end of words or phrases for hit highlight.

The default is Raw.


## Hit highlighting example

The following example shows a web result for `Sailing Dinghy`. Bing marked the beginning and end of the query term using the E000 and E001 Unicode characters.
  
![Hit Highlighting](media/bing-web-api/bing-hit-highlighting.png) 

Before displaying the result in your user interface, replace the Unicode characters with ones that are appropriate for your display format.


## Additional text decorations

Bing can return several different text decorations. For example, a `Computation` answer can contain subscript markers for the query term `log(2)` in the `expression` field.

![computation markers](media/bing-web-api/bing-markers-computation.png) 

If the request did not specify decorations, the `expression` field would contain `log10(2)`. 

If `textDecorations` is **true**, Bing may include the following markers in the display strings of answers. If there is no equivalent HTML tag, the table cell is empty.

|Unicode|HTML|Description
|-|-|-
|U+E000|\<b>|Marks the beginning of the query term (hit highlighting).
|U+E001|\</b>|Marks the end of the query term.
|U+E002|\<i>|Marks the beginning of italicized content.
|U+E003|\</i>|Marks the end of italicized content.
|U+E004|\<br/>|Marks a line break.
|U+E005||Marks the beginning of a phone number.
|U+E006||Marks the end of a phone number.
|U+E007||Marks the beginning of an address.
|U+E008||Marks the end of an address.
|U+E009|\&nbsp;|Marks a non-breaking space.
|U+E00C|\<strong>|Marks the beginning of bold content.
|U+E00D|\</strong>|Marks the end of bold content.
|U+E00E||Marks the beginning of content whose background should be lighter than its surrounding background.
|U+E00F||Marks the end of content whose background should be lighter than its surrounding background.
|U+E010||Marks the beginning of content whose background should be darker than its surrounding background.
|U+E011||Marks the end of content whose background should be darker than its surrounding background.
|U+E012|\<del>|Marks the beginning of content that should be struck through.
|U+E013|\</del>|Marks the end of content that should be struck through.
|U+E016|\<sub>|Marks the beginning of subscript content.
|U+E017|\</sub>|Marks the end of subscript content.
|U+E018|\<sup>|Marks the beginning of superscript content.
|U+E019|\</sup>|Marks the end of superscript content.

