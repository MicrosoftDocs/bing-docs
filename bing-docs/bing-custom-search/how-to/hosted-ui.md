---
title: Configure a hosted UI for Bing Custom Search
titleSuffix: Bing Search Services
description: Use this article to configure and integrate a hosted UI for Bing Custom Search.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 07/15/2023
---

# Configure your hosted UI experience

Bing Custom Search provides a hosted UI that easily integrates into your webpage using JavaScript. You can configure the UI's layout, color, and search options in the Custom Search portal.

## Configure your hosted UI

To configure a hosted UI for your web applications, follow these steps. As you make changes, the pane on the right provides a preview of what the UI will look like. (The displayed search results are not actual results for your instance.)

1. Sign in to the [Custom Search portal](https://customsearch.ai).  
  
2. Select your Custom Search instance.

3. Click the **Hosted UI** tab.  
  
4. Select a layout.

    - Search bar and results (default): Displays a search box with search results below it.  

    - Results only: Displays search results only, without a search box. When using this layout, you must provide the search string (`&q=<search string>`). Add the query parameter (*q*) to the request URL in the JavaScript snippet, or the HTML endpoint link.  

    - Pop-over: Provides a search box and displays the search results in a sliding overlay.

5. Select a color theme. You can customize the colors to fit your application by clicking **Customize theme**. To change a color, either enter the color's RGB HEX value (for example, `#366eb8`) or click on the color preview.

   You can preview your changes on the right side of the portal. Clicking **Reset to default** reverts your changes to the default colors for the selected theme.

   > [!NOTE]
   > Consider accessibility when choosing colors.

6. Under **Additional Configurations**, provide values as appropriate for your app. These settings are optional. To see the effect of applying or removing them, see the preview pane on the right.  

7. Enter the search subscription key or choose one from the dropdown list. The dropdown list is populated with keys from your Azure account's subscriptions.

8. If you enabled autosuggest, enter the autosuggest subscription key or choose one from the dropdown list. The dropdown list is populated with keys from your Azure account's subscriptions.

## Publish or revert a search instance

Custom search has two environments: staging/testing (see the **Configuration** tab) and production (see the **Production** tab). When you create a new instance or make changes to an existing instance, those changes occur in the testing environment.

After configuring and validating your changes, click **Publish** to make your hosted UI configuration live. Changes are not reflected against your production endpoints until you publish.

Before publishing, if you decide that you don't want to keep the changes you've made, click **Revert**. When you revert your changes, the **Published** version remains unchanged and the **Configuration** version is reverted to match the **Published** version.

## Consume your custom UI

To consume your hosted UI after publishing it, select the **Production** tab and then select the **Endpoints** tab. In the list of endpoints, click **Hosted UI**.

Click **Additional parameters** if you want to include the *safeSearch* query parameter or specify the language to use for user interface strings.

Finally, use one of the following options to access your custom hosted UI:

- Copy the JavaScript snippet and paste it into your webpage. The following is an example only (copy the actual snippet from the portal).
  
  ```html
  <html>
      <body>
          <script type="text/javascript" 
              id="bcs_js_snippet"
              src="https://ui.customsearch.ai/api/ux/rendering-js?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=">
          </script>
      </body>    
  </html>
  ```

- Or, copy and use the URL to access your custom UI in a web browser. The following is an example only (copy the actual URL from the portal).  
  
  `https://ui.customsearch.ai/hosted-page?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=`  
  
  > [!IMPORTANT]
  > The page cannot display your privacy statement or other notices and terms. Suitability for your use may vary.  

## Configuration options

The following sections describe the additional configurations listed under **Additional Configurations** (see step 6 above). These settings are optional. To see the effect of applying or removing them, see the preview pane on the right.

### Web search configurations

- Web results enabled: Determines if web search is enabled (see the Web tab at the top of the page).
- Enable autosuggest: Determines if custom autosuggest is enabled.
- Web results per page: Determines the number of web search results to display (the maximum is 50 results per page).
- Image caption: Determines if images are displayed with search results.

The following configurations are shown if you select **Show advanced configurations**:

- Highlight words: Determines whether words or phrases from the user's search string are highlighted (bolded) in the search results.
- Link target: Determines if the webpage opens in a new browser tab (Blank) or the same browser tab (self) when the user clicks a search result link.

### Image search configurations

- Image results enabled: Determines if image search is enabled (see the Images tab at the top of the page).
- Image results per page: Determines the number of image search results to display at a time (the maximum is 150 results per page).

The following configuration is shown if you click **Show advanced configurations**.  
  
- Enable filters: Adds filters that the user can use to filter the images that Bing returns. For example, the user can filter the results for only animated GIFs.

### Video search configurations

- Video results enabled: Determines if video search is enabled (see the Videos tab at the top of the page).
- Video results per page: Determines the number of video search results to display at a time (the maximum is 150 results per page).

The following configuration is shown if you click **Show advanced configurations**.  
  
- Enable filters: Adds filters that the user can use to filter the videos that Bing returns. For example, the user can filter the results for videos with a specific resolution or videos discovered in the last 24 hours.

### Miscellaneous configurations

- Page title: Text displayed in the title area of the search results page (not for pop-over layout).
- Toolbar theme: Determines the background color of the title area of the search results page.

The following configurations are shown if you click **Show advanced configurations**.  

- Search box text placeholder: The text displayed in the search box prior to user input.
- Title link url: The target for the title link.
- Logo URL: The image displayed next to the title.
- Favicon: The icon displayed in the browser's title bar.

The following configurations apply only if you consume the Hosted UI through the HTML endpoint (they don't apply if you use the JavaScript snippet).

- Page title
- Toolbar theme
- Title link URL
- Logo URL
- Faviicon URL  
