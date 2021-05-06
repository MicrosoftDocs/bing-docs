---
title: Bing Visual Search Java client library quickstart 
titleSuffix: Bing Search Services
description: Learn how to get image insights using the Java client library for Bing Visual Search API.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Visual Search Java client library

Use this quickstart to begin getting image insights from the Bing Visual Search service, using the Java client library. While Bing Visual Search has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this quickstart can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-java/tree/main/sdk/VisualSearch).

Use the Bing Visual Search client library for Java to:

* Upload an image to send a visual search request.
* Get the image insight token and visual search tags.

[Reference documentation](https://docs.microsoft.com/en-us/bing/search-apis/bing-visual-search/overview) | [Library source code](https://github.com/microsoft/bing-search-sdk-for-java/tree/main/sdk/VisualSearch) | [Artifact (Maven)](https://mvnrepository.com/artifact/com.microsoft.bing/bing-visualsearch) | [Samples](https://github.com/microsoft/bing-search-sdk-for-java/tree/main/samples/sdk/VisualSearchSample)

## Prerequisites

* Azure subscription - [Create one for free](https://aka.ms/bingapisignup)
* The current version of the [Java Development Kit(JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* The [Gradle build tool](https://gradle.org/install/), or another dependency manager

<!--
[!INCLUDE [bing-visual-search-signup-requirements](../../../../includes/bing-visual-search-signup-requirements.md)]
-->

After you get a key from your resource, create an environment variable for the key, named `BING_SEARCH_V7_SUBSCRIPTION_KEY`.

### Create a new Gradle project

In a console window (such as cmd, PowerShell, or Bash), create a new directory for your app, and navigate to it. 

```console
mkdir myapp && cd myapp
```

Run the `gradle init` command from your working directory. This command will create essential build files for Gradle, including *build.gradle.kts* which is used at runtime to create and configure your application.

```console
gradle init --type basic
```

When prompted to choose a **DSL**, select **Kotlin**.

Locate *build.gradle.kts* and open it with your preferred IDE or text editor. Then copy in this build configuration:

```kotlin
plugins {
    java
    application
}
application {
    mainClassName = "main.java.BingVisualSearchSample"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("org.slf4j:slf4j-simple:1.7.25")
    compile("com.microsoft.bing:bing-visualsearch:1.0.0")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Create a folder for your sample app. From your working directory, run the following command:

```console
mkdir -p src/main/java
```

Create a folder for the image you want to upload to the API. Place the image inside the **resources** folder.

```console
mkdir -p src/main/resources
``` 

Navigate to the new folder and create a file called *BingVisualSearchSample.java*. Open it in your preferred editor or IDE and add the following `import` statements:



```java
package com.microsoft.bing.samples;

import com.microsoft.bing.autosuggest.models.SearchAction;
import com.microsoft.bing.autosuggest.models.Suggestions;
import com.microsoft.bing.autosuggest.models.SuggestionsSuggestionGroup;
import com.microsoft.bing.autosuggest.implementation.AutoSuggestClientImpl;
import com.microsoft.rest.credentials.ServiceClientCredentials;
import okhttp3.*;
import okhttp3.OkHttpClient.Builder;
import java.io.IOException;
import java.util.List;`

Then create a new class.

```java
public class BingVisualSearchSample {
}
```

In the application's `main` method, create variables for your resource's Azure endpoint and key. If you created the environment variable after you launched the application, you will need to close and reopen the editor, IDE, or shell running it to access the variable. Then create a `byte[]` for the image you'll be uploading. Create a `try` block for the methods you'll define  later, and load the image and convert it to bytes using `toByteArray()`.


```java
    public static void main(String[] args) {
        try {
            //=============================================================
            // Authenticate
            // Add your Bing Autosuggest subscription key to your environment variables.
            String subscriptionKey = System.getenv("BING_AUTOSUGGEST_SUBSCRIPTION_KEY");
            String endpoint = System.getenv("BING_AUTOSUGGEST_ENDPOINT") +  "/v7.0";
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
            AutoSuggestClientImpl client = new AutoSuggestClientImpl(endpoint,credentials);
            runSample(client);
        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
}

```

### Install the client library

This quickstart uses the Gradle dependency manager. You can find the client library and information for other dependency managers on the [Maven Central Repository](https://mvnrepository.com/artifact/com.microsoft.bing/bing-visualsearch).

In your project's *build.gradle.kts* file, be sure to include the client library as an `implementation` statement. 

```kotlin
dependencies {
    compile("org.slf4j:slf4j-simple:1.7.25")
    compile("com.microsoft.bing:bing-visualsearch:1.0.0")
    compile("com.google.code.gson:gson:2.8.5")
}
```

## Code examples

These code snippets show you how to do the following tasks with the Bing Visual Search client library and Java:

* [Authenticate the client](#authenticate-the-client)
* [Send a visual search request](#send-a-visual-search-request)
* [Print the image insight token and visual search tags](#print-the-image-insight-token-and-visual-search-tags)

## Authenticate the client

> [!NOTE]
> This quickstart assumes you've created an environment variable for your Bing Visual Search key, named `BING_SEARCH_V7_SUBSCRIPTION_KEY`.


In your main method, be sure to use your subscription key to instantiate a [BingVisualSearchAPI](https://docs.microsoft.com/en-us/bing/search-apis/bing-visual-search/overview) object.

```csharp
BingVisualSearchAPI client = BingVisualSearchManager.authenticate(subscriptionKey);
```

## Send a visual search request


```java
    ImageKnowledge visualSearchResults = client.images().visualSearch(null, null, null, null, null, null, null, null, null, null, imageBytes);
    PrintVisualSearchResults(visualSearchResults);

    }
```

## Print the image insight token and visual search tags



```java
    static void PrintVisualSearchResults(ImageKnowledge visualSearchResults) {
        if (visualSearchResults == null) {
            System.out.println("No visual search result data.");
        } else {
            // Print token

            if (visualSearchResults.image() != null && visualSearchResults.image().imageInsightsToken() != null) {
                System.out.println("Found uploaded image insights token: " + visualSearchResults.image().imageInsightsToken());
            } else {
                System.out.println("Couldn't find image insights token!");
            }

            // List tags

            if (visualSearchResults.tags() != null && visualSearchResults.tags().size() > 0) {
                System.out.format("Found visual search tag count: %d\n", visualSearchResults.tags().size());
                ImageTag firstTagResult = visualSearchResults.tags().get(0);

                // List of actions in first tag

                if (firstTagResult.actions() != null && firstTagResult.actions().size() > 0) {
                    System.out.format("Found first tag action count: %d\n", firstTagResult.actions().size());
                    System.out.println("Found first tag action type: " + firstTagResult.actions().get(0).actionType());
                }
            } else {
                System.out.println("Couldn't find image tags!");
            }
        }
    }
```

## Run the application

You can build the app with:

```console
gradle build
```

Run the application with the `run` goal:

```console
gradle run
```


## Next steps

> [!div class="nextstepaction"]
> [Build a single-page web app](../../tutorial/visual-search-single-page-app.md)
