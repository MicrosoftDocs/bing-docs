---
title: "Quickstart: Get image insights using the REST API and Java - Bing Visual Search"
titleSuffix: Bing Search Services
description: Learn how to get image insights using Java and Bing Visual Search API.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Get image insights using the Bing Visual Search REST API and Java

Use this quickstart to make your first call to the Bing Visual Search API. This Java application uploads an image to the API and displays the information it returns. Although this application is written in Java, the API is a RESTful Web service compatible with most programming languages.

## Prerequisites

* The [Java Development Kit (JDK) 7 or 8](https://aka.ms/azure-jdks).
* The [Gson Java library](https://github.com/google/gson).
* [Apache HttpComponents](https://hc.apache.org/downloads.cgi).

<!--
[!INCLUDE [bing-visual-search-signup-requirements](../../../../includes/bing-visual-search-signup-requirements.md)]
-->

## Create and initialize a project

1. Create a new Java project in your favorite IDE or editor, and import the following libraries:

    ```java
    import java.util.*;
    import java.io.*;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    
    // HttpClient libraries
    
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.ContentType;
    import org.apache.http.entity.mime.MultipartEntityBuilder;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClientBuilder;
    ```

2. Create variables for your API endpoint, subscription key, and the path to your image. 

    ```java
    static String endpoint = "https://api.bing.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "your-key-here";
    static String imagePath = "path-to-your-image";
    ```

    
3. When you upload a local image, the form data must include the `Content-Disposition` header. Set its `name` parameter to "image", and set the `filename` parameter to the file name of the image. The contents of the form include the binary data of the image. The maximum image size you can upload is 1 MB.
    
    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

## Create the JSON parser

Create a method to make the JSON response from the API more readable by using `JsonParser`.

```java
public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
```

## Construct the search request and query

1. In the main method of your application, create an HTTP client using `HttpClientBuilder.create().build();`.

    ```java
    CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    ```

2. Create an `HttpEntity` object to upload your image to the API.

    ```java
    HttpEntity entity = MultipartEntityBuilder
        .create()
        .addBinaryBody("image", new File(imagePath))
        .build();
    ```

3. Create an `httpPost` object with your endpoint, and set the header to use your subscription key.

    ```java
    HttpPost httpPost = new HttpPost(endpoint);
    httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
    httpPost.setEntity(entity);
    ```

## Receive and process the JSON response

1. Use the `HttpClient.execute()` method to send a request to the API, and store the response in an `InputStream` object.
    
    ```java
    HttpResponse response = httpClient.execute(httpPost);
    InputStream stream = response.getEntity().getContent();
    ```

2. Store the JSON string, and print the response.

    ```java
    String json = new Scanner(stream).useDelimiter("\\A").next();
    System.out.println("\nJSON Response:\n");
    System.out.println(prettify(json));
    ```

## Next steps

> [!div class="nextstepaction"]
> [Build a Visual Search single-page web app](../../tutorial/visual-search-single-page-app.md).
