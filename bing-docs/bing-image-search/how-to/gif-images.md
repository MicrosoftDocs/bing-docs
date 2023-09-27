---
title: Search for GIF images using the Bing Image Search API
titleSuffix: Bing Search Services
description: The Bing Image Search API enables you to also search across the entire Web for the most relevant .gif images.
services: bing-search-services
author: alekhyasasi\
ms.author: v-alpunnamar
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 09/27/2023
---

# Search for animated GIF images

Bing Image Search API lets you search across the entire Web for the most relevant animated GIF images. To get only GIF images, set the [imageType](../reference/query-parameters.md#imagetype) query parameter to AnimatedGif

The following cURL example shows how to use the API to get animated GIF images.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/images/search?q=interesting&imageType=AnimatedGif&mkt=en-us
```

## Tips and suggestions

- Specify the [maxFileSize](../reference/query-parameters.md#maxfilesize) and [minFileSize](../reference/query-parameters.md#minfilesize) query parameters. Because most GIFs in our index are under 2 MB, set *maxFileSize* to 2000000. This also helps to control the data size if bandwidth is a concern, such as in mobile device scenarios.
- Help improve perceived performance by loading the thumbnail first and then loading the source URL.  
- Use the [safeSearch](../reference/query-parameters.md#safesearch) query parameter to block adult content. Set to `strict` to block adult content.
- Use the *AnimatedGifHttps* image type to return get only animated GIF images that are from an HTTPS address. For security, many applications require connection to external Web links over HTTPS.

<a name="gifExample"></a>

## Getting animated GIFs using Java

```java
package gifSearch;
import java.net.*;
import java.util.*;
import java.io.*;
import javax.net.ssl.HttpsURLConnection;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.1
 *
 * Once you have compiled or downloaded gson-2.8.1.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac GIFsearch.java -classpath .;gson-2.8.1.jar -encoding UTF-8
 * java -cp .;gson-2.8.1.jar GIFsearch
 */


public class GIFsearch {

 // Replace the subscriptionKey string value with your valid subscription key.
    static String subscriptionKey = "YOUR-ACCESS-KEY";

    static String host = "https://api.bing.microsoft.com";
    static String path = "/v7.0/images/search";

    static String searchTerm = "interesting";

    public static SearchResults SearchImages (String searchQuery) throws Exception {
        // construct URL of search request (endpoint + query string)
        URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + "&imageType=AnimatedGif&mkt=en-us");
        HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

        // receive JSON body
        InputStream stream = connection.getInputStream();
        String response = new Scanner(stream).useDelimiter("\\A").next();

        // construct result object for return
        SearchResults results = new SearchResults(new HashMap<String, String>(), response);

        // extract Bing-related HTTP headers
        Map<String, List<String>> headers = connection.getHeaderFields();
        for (String header : headers.keySet()) {
            if (header == null) continue;      // may have null key
            if (header.startsWith("BingAPIs-") || header.startsWith("X-MSEdge-")) {
                results.relevantHeaders.put(header, headers.get(header).get(0));
            }
        }

        stream.close();
        return results;
    }

    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main (String[] args) {
        if (subscriptionKey.length() != 32) {
            System.out.println("Invalid Bing Search API subscription key!");
            System.out.println("Please paste yours into the source code.");
            System.exit(1);
        }

        try {
            System.out.println("Searching the Web for: " + searchTerm);

            SearchResults result = SearchImages(searchTerm);

            System.out.println("\nRelevant HTTP Headers:\n");
            for (String header : result.relevantHeaders.keySet())
                System.out.println(header + ": " + result.relevantHeaders.get(header));

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(result.jsonResponse));
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }

}

//Container class for search results encapsulates relevant headers and JSON data
class SearchResults{
 HashMap<String, String> relevantHeaders;
 String jsonResponse;
 SearchResults(HashMap<String, String> headers, String json) {
     relevantHeaders = headers;
     jsonResponse = json;
 }
}

```

## Next steps

- Learn about the [Image search tutorial](../tutorial/bing-image-search-single-page-app.md)
