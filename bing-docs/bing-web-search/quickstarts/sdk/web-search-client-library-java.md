---
title: Bing Web Search Java client library quickstart 
titleSuffix: Bing Search Services
description: Learn how to get web search results using the Java client library for Bing Web Search API.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use a Bing Web Search Java client library


The Bing Web Search client library makes it easy to integrate Bing Web Search into your Java application. In this quickstart, you'll learn how to send a request, receive a JSON response, and filter and parse the results.

Want to see the code right now? Samples for the [Bing Search client libraries for Java](https://github.com/microsoft/bing-search-sdk-for-java/tree/main/samples/sdk/WebSearchSample) are available on GitHub.

## Prerequisites

Here are a few things that you'll need before running this quickstart:

* [JDK 7 or 8](https://aka.ms/azure-jdks)
* [Apache Maven](https://maven.apache.org/download.cgi) or your favorite build automation tool
* A subscription key

<!--
[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]
-->

## Create a project and set up your POM file

Create a new Java project using Maven or your favorite build automation tool. Assuming that you're using Maven, add the following lines to your [Project Object Model (POM)](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html) file. Replace all instances of `mainClass` with your application.

```xml
<build>
    <plugins>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>1.4.0</version>
        <configuration>
          <!--Your comment
            Replace the mainClass with the path to your java application.
            It should begin with com and doesn't require the .java extension.
            For example: com.bingwebsearch.app.BingWebSearchSample. This maps to
            The following directory structure:
            src/main/java/com/bingwebsearch/app/BingWebSearchSample.java.
          -->
          <mainClass>com.path.to.your.app.APP_NAME</mainClass>
        </configuration>
      </plugin>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.0</version>
        <configuration>
          <source>1.7</source>
          <target>1.7</target>
        </configuration>
      </plugin>
      <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>attached</goal>
            </goals>
            <configuration>
              <descriptorRefs>
                <descriptorRef>jar-with-dependencies</descriptorRef>
              </descriptorRefs>
              <archive>
                <manifest>
                  <!--Your comment
                    Replace the mainClass with the path to your java application.
                    For example: com.bingwebsearch.app.BingWebSearchSample.java.
                    This maps to the following directory structure:
                    src/main/java/com/bingwebsearch/app/BingWebSearchSample.java.
                  -->
                  <mainClass>com.path.to.your.app.APP_NAME.java</mainClass>
                </manifest>
              </archive>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure</artifactId>
      <version>1.9.0</version>
    </dependency>
    <dependency>
      <groupId>commons-net</groupId>
      <artifactId>commons-net</artifactId>
      <version>3.3</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.bing</groupId>
      <artifactId>bing-websearch</artifactId>
      <version>1.0.0</version>
    </dependency>
  </dependencies>
```

## Declare dependencies

Open your project in your favorite IDE or editor and import these dependencies:

```java
import com.microsoft.bing.websearch.implementation.WebSearchClientImpl;
import com.microsoft.bing.websearch.models.ImageObject;
import com.microsoft.bing.websearch.models.NewsArticle;
import com.microsoft.bing.websearch.models.SearchResponse;
import com.microsoft.bing.websearch.models.VideoObject;
import com.microsoft.bing.websearch.models.WebPage;
import com.microsoft.rest.credentials.ServiceClientCredentials;
import okhttp3.*;
import okhttp3.OkHttpClient.Builder;
import java.io.IOException;
```

If you created the project with Maven, the package should already be declared. Otherwise, declare the package now. For example:

```java
package com.bingwebsearch.app
```

## Declare the BingWebSearchSample class

Declare the `BingWebSearchSample` class. It will include most of our code including the `main` method.  

```java
public class BingWebSearchSample {

// The code in the following sections goes here.

}
```

## Construct a request

The `runSample` method, which lives in the `BingWebSearchSample` class, constructs the request. Copy this code into your application:

```java
public static boolean runSample(BingWebSearchAPI client) {
    /*
     * This function performs the search.
     *
     * @param client instance of the Bing Web Search API client
     * @return true if sample runs successfully
     */
    try {
        /*
         * Performs a search based on the .withQuery and prints the name and
         * url for the first web pages, image, news, and video result
         * included in the response.
         */
        System.out.println("Searched Web for \"Xbox\"");
        // Construct the request.
        SearchResponse webData = client.webs().search("Xbox");

// Code continues in the next section...
```

## Handle the response

Next, let's add some code to parse the response and print the results. The `name` and `url` for the first web page, image, news article, and video are printed when included in the response object.

```java
/*
* WebPages
* If the search response has web pages, the first result's name
* and url are printed.
*/
if (webData != null && webData.webPages() != null && webData.webPages().value() != null &&
        webData.webPages().value().size() > 0) {
    // find the first web page
    WebPage firstWebPagesResult = webData.webPages().value().get(0);

    if (firstWebPagesResult != null) {
        System.out.println(String.format("Webpage Results#%d", webData.webPages().value().size()));
        System.out.println(String.format("First web page name: %s ", firstWebPagesResult.name()));
        System.out.println(String.format("First web page URL: %s ", firstWebPagesResult.url()));
    } else {
        System.out.println("Couldn't find the first web result!");
    }
} else {
    System.out.println("Didn't find any web pages...");
}
/*
 * Images
 * If the search response has images, the first result's name
 * and url are printed.
 */
if (webData != null && webData.images() != null && webData.images().value() != null &&
        webData.images().value().size() > 0) {
    // find the first image result
    ImageObject firstImageResult = webData.images().value().get(0);

    if (firstImageResult != null) {
        System.out.println(String.format("Image Results#%d", webData.images().value().size()));
        System.out.println(String.format("First Image result name: %s ", firstImageResult.name()));
        System.out.println(String.format("First Image result URL: %s ", firstImageResult.contentUrl()));
    } else {
        System.out.println("Couldn't find the first image result!");
    }
} else {
    System.out.println("Didn't find any images...");
}
/*
 * News
 * If the search response has news articles, the first result's name
 * and url are printed.
 */
if (webData != null && webData.news() != null && webData.news().value() != null &&
        webData.news().value().size() > 0) {
    // find the first news result
    NewsArticle firstNewsResult = webData.news().value().get(0);
    if (firstNewsResult != null) {
        System.out.println(String.format("News Results#%d", webData.news().value().size()));
        System.out.println(String.format("First news result name: %s ", firstNewsResult.name()));
        System.out.println(String.format("First news result URL: %s ", firstNewsResult.url()));
    } else {
        System.out.println("Couldn't find the first news result!");
    }
} else {
    System.out.println("Didn't find any news articles...");
}

/*
 * Videos
 * If the search response has videos, the first result's name
 * and url are printed.
 */
if (webData != null && webData.videos() != null && webData.videos().value() != null &&
        webData.videos().value().size() > 0) {
    // find the first video result
    VideoObject firstVideoResult = webData.videos().value().get(0);

    if (firstVideoResult != null) {
        System.out.println(String.format("Video Results#%s", webData.videos().value().size()));
        System.out.println(String.format("First Video result name: %s ", firstVideoResult.name()));
        System.out.println(String.format("First Video result URL: %s ", firstVideoResult.contentUrl()));
    } else {
        System.out.println("Couldn't find the first video result!");
    }
} else {
    System.out.println("Didn't find any videos...");
}
```

## Declare the main method

In this application, the main method includes code that instantiates the client, validates the `subscriptionKey`, and calls `runSample`. Make sure that you enter a valid subscription key for your Azure account before continuing.

```java
public static void main(String[] args) {
    try {
        // Enter a valid subscription key for your account.
        final String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
        final String endpoint = "YOUR_ENDPOINT";
        // Instantiate the client.
        ServiceClientCredentials credentials = new ServiceClientCredentials() {
                @Override
                public void applyCredentialsFilter(Builder builder) {
                    builder.addNetworkInterceptor(
                        new Interceptor() {
                            @Override
                            public Response intercept(Chain chain) throws IOException {
                                Request request = null;
                                Request original = chain.request();
                                Request.Builder requestBuilder = original.newBuilder();
                                requestBuilder.addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
                                request = requestBuilder.build();
                                return chain.proceed(request);
                            }
                        }
                    );
                }
            };
            WebSearchClientImpl client = new WebSearchClientImpl(endpoint,credentials);

        // Make a call to the Bing Web Search API.
        runSample(client);
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }
}
```

## Run the program

The final step is to run your program!

```console
mvn compile exec:java
```

## Clean up resources

When you're done with this project, make sure to remove your subscription key from the program's code.

## Next steps

> [!div class="nextstepaction"]
> [Web Search Java SDK samples](https://github.com/microsoft/bing-search-sdk-for-java/tree/main/samples/sdk/WebSearchSample)

