# Lab 7 - Consumer Experience

In this lab, you will learn the consumer experience for APIs that have been exposed to your developer organization.

Using the developer portal, you will first login in as the admin user to load a Think IBM custom theme. You will then login as a developer to register your application and then subscribe to an API and test that API.

---
# Lab 7 - Objective

In the this lab, you will learn:

+ How to add a custom theme to change the overall look and feel of the developer portal.
+ How to register an application with the developer portal.
+ How to subscribe to a plan in order to consume an API.
+ How to test an API from the developer portal.
+ How to consume an API from a sample test application.

---
# Lab 7 - Case Study Used in this Tutorial

In this tutorial, **ThinkIBM** wishes to brand their developer portal to their own look and feel. They also want to provide a self-service experience to application developers who are potential consumers of their APIs. 

---
# Lab 7	- Step by Step Lab Instructions

## 7.1 - Customize the Developer Portal

In this section, you will leverage a custom prebuilt theme. A theme file is packaged as a zip file that contains a set of predefined files. The theme zip file is what is loaded into the developer portal. The best method for creating a custom theme is to obtain an existing custom theme, rename it and change the files as explained in the table below to meet your customization needs.

|Filename|Description|
|--------|-----------|  
|`overrides.css`|This style sheet overrides the fonts, colors and other defaults from the inherited base IBM API Connect theme. The name of this css file is specified in the theme.info file.|
|`screenshot.png`|The file contains a screenshot of the home page for the custom theme and is used in the appearance settings so you can easily find the theme you want to enable and set as the default theme. When you are finishing completing the edits to the overrides.css and have set up the welcome banner to your satisfaction, you should take a screen shot of the developer portal home page and capture it with Snagit or some other screen capture tool and place the file in the top level directory of the theme file. The name of this screenshot file is specified in the theme.info file.|
|`templete.php`|The template.php file contains your sub-theme's functions to manipulate Drupal's default markup. It is one of the most useful files when creating or modifying Drupal themes. With this file you can modify any theme hooks variables or add your own variables, using preprocess or process functions. Override any theme function: that is, replace a module's default theme function with one you write. Call hook alter functions, which allow you to alter various parts of Drupal's internals, including the render elements in forms. See <http://api.drupal.org> for more information about these functions.| 
|`thinkibm_connect_theme.info`|The .info file is a static text file for defining and configuring a theme. Each line in the.info file is a key-value pair with the key on the left and the value on the right, with an "equals sign" between them (e.g. name = my_theme). The info file naming conventions needs to have the theme name specified as part of the filename. The project key in the .info file contains the name of the theme.|
|`theme-settings.php`|Drupal themes get a number of configurable settings options for free. For example, most provide toggle switches for the search box, site slogans, user pictures, and so on. Similarly, most provide file uploading widgets to add a custom logo or favicon. These settings are easy: Drupal will add them to the theme's configuration page by default, so it takes no extra work. We want to create our own custom setting, however; one that adds a hidden field that contains the current release information to the Theme configuration form. To do that, we'll need to add a new file to the theme: `theme-settings.php`. The function name specified within this file needs to be prefixed with the theme name: `thinkibm_connect_theme`.|
|`thinkibm_welcome_banner.png`|This file provides the image that shows up in the welcome banner. Although the welcome banner does not need to be part of the theme zip file contents, it is useful to place the welcome banner with other theme files.|
|`favicon.ico`|In web development, you can provide a small logo for your site that appears near the address bar and in the bookmarks folder in a visitor's browser. This logo is called the favicon. Drupal provides a default one, which is the recognizable water drop logo. Using the Drupal logo as the favicon is fine but if you really want to make your site stand out, you should provide your own. Favicon files are in the .ico format and are extremely small in dimensions. The default Drupal favicon is 32 pixels high by 32 pixels wide, many browsers use a 16 x 16 pixel version that can be included in the same file. This is because the favicon is only an icon that shows up in the address bar and favorites (bookmarks) list and typically there is not a lot of room there. Any favicon that you create should be just as small.|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
|`logo.png`|The default logo that appears at the top left-hand side of the developer portal page.|

### 7.1.1 - Install and Configure a Custom Theme

Now that we have explained the contents of a custom theme file, it is time to load the custom theme.

1. From the Firefox browser, click the `Portal` bookmark on the toolbar to lauch the developer portal.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/DeveloperPortalFirefoxBookmark.png)

1. Login into the developer portal as an administrator using a username of `admin` and a password of `Passw0rd!`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/PortalAdminLogin.png)

1. From the Administrator menu, select `Appearance` and then `Install new theme`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/InstallNewPortalTheme.png)

1. Click the `Browse` button and navigate to the `/home/student/lab_files/lab7/` directory and select the `think_ibm_connect_theme.zip file`, then click the `Open` button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/open-theme-zip.png)

1. Click the `Install` button to install the **ThinkIBM** custom theme.   

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/InstallPortalThemeZipFile.png)
    
1. The installation of the custom theme completes. Click the `Enable newly added themes` link.
   
1. Scroll down the list of themes to find the `thinkibm_connect 7.43` theme. Click the `Enable and set default link` link.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/EnableandSetDefaultThinkIBMTheme.png)
       
1. We need to go into settings for the theme and save the configuration. Click the `Settings` link under the theme.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/ThinkIBMSettings.png)
   
1. Scroll down to the bottom of the settings page to find the **Toggle Display** section. Expand the **Toggle click the `Save Configuration` button at the bottom of the screen:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/ThinkIBMSaveConfiguration.png)
	
1. Click the `Home` icon to return to the home page.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/CloseThemeSettings.png)

### 7.1.2 - Customize the Welcome Banner
Now we want to change the default Welcome banner to use our custom Welcome banner image. 

1. Navigate to the blocks content page by selecting `Content > Blocks` from the admin menu.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/ThinkIBMBlocks.png)

1. Underneath operations select `Edit` to the right of the Welcome banner block.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/EditWelcomeBannerBlock.png)
	   
1. Scroll down the banner edit page, click the `Browse` button underneath **Image** to launch the file explorer.

1. Navigate to the `/home/student/lab_files/lab7/think_ibm_connect_theme` directory and select the `thinkibm_welcome_banner.png` file. Then, click the `Open` button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/open-welcome-banner-img.png)

1. Click the `Upload` button to upload the new welcome banner image.

1. Scroll to the bottom of the page and click the `Save` button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/SaveWelcomeBanner.png)

1. Close the content block settings.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/CloseContentBlockSettings.png)

### 7.1.3 - Change the Region Settings for the Content Blocks

The region settings for some of the content blocks have been reset, so we will set these back.

1. Navigate to the region settings for block content page by selecting `Structure > Blocks` from the admin menu.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/ThinkIBMBlockRegionSettings.png)

1. Locate the **Sidebar first** block. You will see entries for `Navigation`, `Support` and `User login`.

1. For **both** the `Navigation` and `User Login` entries, select `- None -` from the drop down menu.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/SetBlockRegionstoNone.png)

1. The only entry left in the section should be `Support`.
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/sidebar-first-complete.png)

1. Scroll down to the bottom of the screen anc click the `Save Blocks` button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/SaveBlockRegions.png)

1. Click close on the block region settings.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/CloseContentBlockSettings.png)

1. You are finished with customizing the developer portal. There is a lot more that can be customized than what we have time for in this lab. Log out of the developer portal.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/PortalAdminLogout.png)

## 7.2 - Register an Application as a Developer 

In this section, you will log into the portal as a user in the application developer role, then register an application that will be used to consume APIs.

1. Login into the developer portal as an application developer using a username of `developer@consumer.ibm` and a password of `Passw0rd!`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/PortalDeveloperLogin.png)

1. Click the `Apps` link.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/PortalAppsList.png)

1. Click the `Register new Application` link.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/RegisterNewApplication.png)

1. Enter a title and description for the application as shown below and click the `Submit` button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/SubmitNewApplication.png)

1. We need to capture the Client Secret and Client ID in a text editor for later use by our web application. Select the `Show Client Secret` checkbox next to Client Secret at the top of the page and the `Show` checkbox next to Client ID.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/ShowClientSecretandID.png)

1. Launch the `Notes` application as shown below. 

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/SelectNotesApp.png)
	
1. Navigate back to the browser and copy/paste both the Client ID and Client Secret into the Notes application window as shown below. Add a notation above each item so you know which value is the Client ID and which is the Client Secret. Do not exit the Notes application as we will need these values later in this lab.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/SaveClientIDandSecret.png)

## 7.3 - Subscribe to a Plan for the ThinkIBM APIs 
In this section, we will subscribe to a plan for the ThinkIBM APIs using the Think IBM Web Consumer application.

1. Click the `API Products` link.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/PortalAPIProductsList.png)

1. Click the `think (v1.0.0)` API product link.                                                          

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/SelectThinkIBMProduct.png)

1. Click the `Plans` link on the left-hand navigation menu and then click the `Subscribe` button under the **Silver** plan as shown below.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/SubscribetoThinkSilverPlan.png)
	
	> ![Alt text][info]
	> 
	> The Gold plan requires approval by the API provider for any subscription requests and allows unlimited requests for a given time period. The Silver plan is limited to 100 requests per hour and does not require approval by the API provider for subscription requests.    

1. Select the `Think IBM Web Consumer` toggle and click the `Subscribe` button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/SubscribeThinkIBMWebApplicationtoPlan.png)

1. The `Think IBM Web Consumer` Web application is now subscribed to the Silver plan for the `think` API product.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/ThinkIBMWebApplicationSubscribed.png)

1. Let's validate that we subscribed to the Silver plan for the `think v1.0.0` API product. Click the `Apps` link.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/PortalAppsList.png)

1. Click the `Think IBM Web Consumer` application link.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/SelectThinkIBMWebConsumerApp.png)

1. The `Think IBM Web Consumer` application is subscribed to the Silver plan for the `think` product as shown below.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/ValidateSubscribedtoThinkSilverPlan.png)

## 7.4 - Test `think` Product APIs from the Developer Portal

In this section, we will use the developer portal to test one of the think product APIs. This is useful for application developers to **_try out_** the APIs before their application is fully developed or to simply see the expected response based on inputs they provide the API. Later in this lab, you will run an actual application that is already developed and uses these same APIs. We will test the `logistics` API from the developer portal.

1. Click the `API Products` link.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/PortalAPIProductsList.png)

1. Click the `think (v1.0.0)` API product link.

  ![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/SelectThinkIBMProduct.png)

1. Click the `logistics 1.0.0` link on the left-hand navigation menu and then expand the `GET /shipping` path by clicking on the twisty next to the path.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/PortalExpandShipping.png)

1. Scroll down to the **Try this operation** section for the `GET /shipping` path. Enter any zip code and click the `Call Operation` button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/PortalTryShippingOperation.png)

1. Scoll down below the `Call operation` button. You should see a `200 OK` and a response body as shown below.
  
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/PortalTryShippingOperationResultsSuccess.png)

## 7.5 - Test the APIs from the Think IBM Web Consumer Application

Now that you have browsed the API Portal and registered / tested the API's that **ThinkIBM** is providing, it's time to test them out from a real application. We have provided a sample consumer application which will be used to interract with the **ThinkIBM** API's.

1. The ThinkIBM Consumer Application is automatically set up to launch when you open the Chrome browser:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/launch-chrome.png)

1. In order to set up the consumer app to use our registered client credentials, a setup screen will be displayed asking for some application configuration parameters.

1. Using the Client ID and Client Secret values you saved earlier in the Notes application, copy/paste them into the `Client ID` and `Client Secret` fields as shown below:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/config-consumer-app.png)

1. Leave the `API Connect Host`, `API Connect Org` and `API Connect Catalog` fields as-is. Click the `Submit` button.

1. The home page is a simple landing page which does not invoke any of the API's. Recall that in Lab 4 you secured the `inventory` API by requiring OAuth. If you attempt to view the Item Inventory before logging in, you will be challenged for a username and password. Either click on the `Browse Item Inventory` button, or the `Log In` link to open the login prompt.

1. Enter any Username and Password then click the `Log In` button. Our sample authentication service is a dummy repository that will accept any credentials provided.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/consumer-login.png)

	> ![][info]
	> 
	> The consumer app will contact our OAuth Token URL and handle the token exchange using standard OAuth proceedures. If you're interested in seeing the token, you can switch over to the terminal from where you launched the consumer app and view the logs. Look for a line similar to:

	```text
	Using OAuth Token: AAEkZGEwZDgxNDMtMzIxMS00ZDgyLWE3MGYtMTJmY2UyYjk1YmUyxzqSMdEr7wM8XU_edO3dmxGDmC1ZGaqUC9Ibh-rOVNlflzd6blfDq4CGaNsD1qn-KESw6Q6RN_TPA0IfMdIn1nHY4ZpGRYa5N0f7mgY2Jg4Tfhm0IlhCUq5HRvoo7c4SIX7SAS3rL998_BvMVBST_g
	```

1. Once you are logged in, the consumer application will redirect you to the item inventory page. The data displayed on the page is being powered by our `inventory` API! The consumer application is calling to our API, which is then being sent to our LoopBack application which handles the connection to the MySQL data source where the inventory data is persisted.

1. Click around the pages. Use the review form to leave new reviews for items. Recall how the item reviews are separate data models stored in MongoDB, yet are related to the items through our LoopBack application.

	Notice how the product rating is updated automatically as new reviews are posted. This happens because of the custom code you added to the LoopBack application in Lab 3.
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/sample-reviews.png)

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/updated-rating.png)

1. Continue testing the features of the consumer application. Try entering your home zip code to obtain shipping data. This feature is powered by the `logistics` APIs that were built using advanced assembly techniques in Lab 5. Your quotes will vary based on the zip code provided.

	Notice how you are also provided a link to the nearest store. Clicking here will launch the Google Maps link which was also built in our Lab 5 assembly.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/shipping-results.png)

1. Finally, try clicking on the `Calculate Monthly Payment` link. This feature is powered by the `financing` API that was created using our REST-to-SOAP assembly in Lab 5.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab7/financing-results.png)

1. Repeat the steps on a few of products to drive data into the analytics database.

# Lab 7 - Completion

**Congratulations!** You have completed Lab 7!

Proceed to [Lab 8 - Analytics in API Connect](../Lab%208%20-%20Analytics%20in%20API%20Connect)

[important]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/common/important.png "Important!"
[info]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/common/troubleshooting.png "Troubleshooting"