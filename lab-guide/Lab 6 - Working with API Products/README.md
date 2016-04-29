# Lab 6 - Working with API Products

API's published by API Connect are bundled into an object called a **Product**. The Product combines one or more API's with one or more Plans.

A **Plan** is effectively a contract between the API Provider and API Consumer which specifies the allowed rate of API calls over a given period of time.

---
# Lab 6 - Objective

In the following lab, you will learn:

+ How to create a Product
+ How to attach APIs to a Product
+ How to create a Plan
+ How to publish a Product

---
# Lab 6	- Case Study Used in this Tutorial

Your work as an Application Developer / API Designer is now complete. It's time to switch roles and become an API Product Manager. The role of the API Product Manager is to take the developed assets and bundle them together using a go-to-market strategy.

In the case of **ThinkIBM**, you will publish all of our API's together as a single product offering to API Consumers. Additionally, you will create two plans which have different levels of access to your APIs.

---
# Lab 6	- Step by Step Lab Instructions

## 6.1 - Creating an API Product

1. Return to the API list, and switch to the `Products` tab

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/products.png)

1. Click the link for the `inventory` product.
	
	> ![][info]
	> 
	> This product was automatically created during creation of the Inventory LoopBack application.

1. Edit the Product Info & Contact details:

	> Title: `think`
	
	> Name: `think`
	
	> Description: `The **think** product will provide really awesome APIs to your application.`
	
	> Contact Name: `Thomas Watson`
	
	> Contact Email: `thomas@think.ibm`
	
	> Contact URL: `http://www.ibm.com`  
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/think-infocontact.png)

1. Specify a License and Terms of Service:

	> License Name: `The MIT License (MIT)`
		
	> License URL: `https://opensource.org/licenses/MIT`
		
	> Terms of Service: paste the contents of the `lab6/license.txt` file
	  
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/think-licensetos.png)
	
1. Modify the Visibility so that the `think` product is only visible to `Authenticated users`:
  
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/think-visibility.png)
	
1. Navigate to the APIs section. Click the `+` button to add all of our new APIs to this product.

1. Check the checkboxes next to `financing`, `logistics` and `oauth`. 
	 
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/think-apis.png)

1. Click the `Apply` button.

1. Navigate to the Plans section. Modify the default plan by selecting it and specifying the following properties:

	> Title: `Silver`
	
	> Name: `silver`
	
	> Description: `Limited access to these APIs`
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/think-silverplan.png)

1. Click the `+` button to create a new plan. Give it the following properties:

	> Title: `Gold`
	
	> Name: `gold`
	
	> Description: `Unlimited access to these APIs for approved users`
	
	> Rate limit: `Unlimited`
	
	> Approval: check `Require subscription approval`  
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/think-goldplan.png)

1. Save your changes.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/save-icon.png)

## 6.2 - Publishing the API Product

1. Click the `Publish` icon.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/think-publish.png)

1. Select `Add and Manage Targets` from the menu.

1. Select `Add a different target`.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/publish-adddifferent.png)

1. Provide connection information to sign into the IBM API Connect management server, then click the `Sign in` button:

	> API Connect host address: `mgr.think.ibm`
	
	> Username: `student@think.ibm`
	
	> Password: `Passw0rd!`  
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/publish-target-signin.png)

1. On the **Select an organization and catalog** screen, choose the `Sandbox` catalog and click the `Next` button.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/publish-sandbox-catalog.png)

1. On the **Select an App** screen, choose the `Inventory` application and click the `Save` button.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/publish-inventory-app.png)
	
	> ![][info]
	> 
	> Our offline toolkit environment is now configured to speak to the central management server.
	> 
	> Since we already published the Inventory app in Lab 3, now all we need to do is publish the associated Product containing the APIs.

1. Click `Publish` button once more and select our target:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/publish-target.png)

1. Check the box to `Stage or Publish products`:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/publish-product.png)

	> ![][info]
	> 
	> Here we have the opportunity to select what gets published. If we were working on multiple API products as part of this project, we could chose specific ones to publish.
	> 
	> Also, the option exists to only Stage the product. A Stage-only action implies that we'll push the configuration to the Management server, but not actually make it available for consumption yet. The reason for doing this may be because your user permissions only allow staging, or that a different group is in charge of publishing Products.

1. Click the `Publish` button to make the API available in the Developer Portal and enforced on our API Gateway.

1. Wait a moment while the Product is published, a `Success` message will appear letting you know the step is complete:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab6/publish-success.png)

1. Close the Firefox web browser.

1. In the `Terminal Emulator`, use the `control+c` keyboard command to quit the API Designer process.

# Lab 6 - Validation

Using both the CLI and the API Designer, a lot of files have been created and updated. Before moving on to the next lab, a simple script has been provided for you which will validate your source files against those of a completed lab. These next steps will help ensure that easily made typing errors do not cause problems down the line.

1. From the `Terminal Emulator`, type:

	```bash
	validate_lab 6
	```
	
1. The script will execute a series `diff` commands against specific files in your project folder (`~/ThinkIBM/inventory`)

1. If the output of the `validate_lab` script includes discrepencies, you may merge the corrected changes into your source files by typing:

	```bash
	merge_lab 6
	```
	
1. If a merge was necessary, you will also need to re-publish the API Product in order to update the API Connect server with the latest changes.

1. Run the `apic edit` command to launch the API Designer, then repeat the steps from a moment ago to re-publish the product.

# Lab 6 - Conclusion

**Congratulations!** You have completed Lab 6.

In this lab, you learned:

+ About API Products
+ How to publish an API Product

Proceed to [Lab 7 - Consumer Experience](../Lab%207%20-%20Consumer%20Experience)

[important]: https://github.com/ibm-apic-pot/lab-guide/raw/master/img/common/important.png "Important!"
[info]: https://github.com/ibm-apic-pot/lab-guide/raw/master/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apic-pot/lab-guide/raw/master/img/common/troubleshooting.png "Troubleshooting"