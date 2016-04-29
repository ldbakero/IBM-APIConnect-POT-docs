# Lab 4 - Configure and Secure an API

In this lab, you will learn how to configure and secure the `inventory` API created during loopback application generation. Using the graphical design tools in API Designer, you will create an OAuth 2.0 provider API called `oauth` and then update the `inventory` API to use this provider. You will use the API Editor assembly view to specify the API's runtime behavior.

---
# Lab 4 - Objective

In the following lab, you will learn:

+ How to create an OAuth 2.0 Provider, specifically using the Resource Owner Password grant type.
+ How to secure an existing API using the newly created OAuth 2.0 Provider.
+ How to add catalog-specific properties to an API.
+ How to assemble an API implementation using the activity-log, set-variable and invoke policies

```text
FIXME: Because of a limitation in the proxy policy not being able to pass
the Host header, we'll need to use the Invoke policy. One downfall though
of using invoke is that the filter parameters don't work.
```

---
# Lab 4 - Case Study Used in this Tutorial

In this tutorial, you will secure the Inventory API to protect the resources exposed by **ThinkIBM**. Consumers of your API will be required to obtain & provide a valid OAuth token before they can invoke the Inventory API.

---
# Lab 4	- Step by Step Lab Instructions

## 4.1 - Working with the Inventory API in API Designer

1. First, launch API Designer by typing the following commands from your project directory:

	```bash
	cd ~/ThinkIBM/inventory/
	apic edit
	```

	API Designer will open in your browser. You may see an informational message about Draft APIs. (This message appears the very first time you launch the API Designer.) If so, click the `Got it!` button, when you are ready to proceed to creating an API.

	You should see the APIs view, and a single API listed. The `inventory` API was automatically created during loopback app generation. We will edit this API at a later step.
	
	![alt text](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/startapidesigner.png)

## 4.2 - Adding a New OAuth 2.0 Provider API

1. Click the `+ Add` button and select `OAuth 2.0 Provider API` from the menu.

1. Specify the following properties and click the `Next` button to continue.

	> Title: `oauth`
	
	> Name: `oauth`
	
	> Base Path: `/oauth20`
	
	> Description: `API for Obtaining Access Tokens`
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/newoauthprops-1of2.png)

1. Accept the default radio button selection labeled `Don't add to a product` and click the `Add` button.

	The API Editor will launch. If this is your first time using the API Editor, you will see an informational message. When you are ready to proceed, click the `Got it!` button to dismiss the message.  
	
	The API Editor opens to the newly created `oauth` API. The left hand side of the view provides shortcuts to various elements within the API definition: Info, Host, Base Path, etc. By default, the API Editor opens to the `Design` view, which provides a user-friendly way to view and edit your APIs. You may notice additional tabs labeled `Source` and `Assemble`. We will work with these views as well.
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/newoauth-start.png)

1. Navigate to the `Host` section of the API. Remove `$(catalog.host)` from the Host field, as we want to keep this blank.

	> ![][troubleshooting]
	> 
	> The host field will show red, indicating it's a required field. You may ignore this warning.
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/no-host.png)

1. Navigate to the `OAuth 2` section.

	Over the next several steps, we will set up OAuth-specific options, such as client type (public vs confidential), valid access token scopes, supported authorization grant types, etc. The [OAuth 2.0 Specification](http://tools.ietf.org/html/rfc6749) has detailed descriptions of each of the properties we are configuring here.

1. For the Client type field, click the drop down twisty and select `Confidential`.

1. Three scopes were generated for you when the OAuth API Provider was generated: `scope1`, `scope2`, `scope3`.

1.  Modify the values for `scope1`, set the following fields:

	> Name: `inventory`
	
	> Description: `Access to Inventory API`

1. Delete `scope2` and `scope3` by clicking the trashcan icons to the right of the scope definitions.

1. We want to configure this provider to *only* support the Resource Owner Password Credentials grant type. Deselect the `Implicit`, `Application` and `Access Code` Grants, but leave `Password` checked.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/oauthgranttypes.png)

1. Set the remaining OAuth 2 settings as follows:

	> Collect credentials using: `basic`
	
	> Authenticate application users using: `Authentication URL`
	
	> Authentication URL: `https://services.think.ibm:1443/auth`
	
	> TLS Profile: **remove** `tls-profile-4` and leave blank
	
	> Deselect the `Enable revocation URL` option
	
	When complete, your settings should look like this:
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/edit-oauthoauth2.png)

	> ![][important]
	> 
	> The scope defined here must be identical to the scope that we define later when telling the `inventory` API to use this OAuth config. A common mistake is around case sensitivity. To avoid running into an error later, make sure that your scope is set to all **lowercase**.

1. Navigate to the `Paths` section. Notice that the generated paths begin with `/oauth2`. However, since we have configured our base path to be `/oauth20`, we will shorten the authorization and token paths.

1. Change the `/oauth2/authorize` path to `/authorize`  

1. Change the `/oauth2/token` path to `/token`

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/edit-oauthpaths.png)

1. Click the `Save` icon in the top right corner of the editor to save your changes.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/save-icon.png)

## 4.3 - Configuring and Securing the Inventory API

1. Click the `All APIs` link at the top left of the API Editor to return to list of APIs.

1. Click the `inventory` link.

	The inventory API will open in the API Editor, where we can make the necessary configuration changes. Over the next several steps you will set this API up to use the OAuth provider you just created.  
	
1. A nice feature of the API Designer is the ability to automatically validate the API definition. Notice that the definition that was generated for us includes a warning.

	Click on the warning symbol to view the warning message, then click on the `show me` link to navigate to the section of the design causing the warning.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/swagger_warning.png)

1. Click on the `trashcan` icon for the `x-any` Definition to remove it. Confirm the removal by clicking the `OK` button in the prompt.

1. Navigate to the `Base Path` section.

	Change the base path from `/api` to `/inventory`.

1. Navigate to the `Host` section of the API. **Remove** the `$(catalog.host)` value.

	As with the OAuth API Provider we just created, we want this value to remain empty.

1. Navigate to the `Security Definitions` section.

	Click the `+` icon in the **Security Definitions** section and select `OAuth` from the menu.  
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/inv-addoauthsecuritydef.png)
	
	A new security definition is created for you, called `oauth-1 (OAuth)`.

1. Scroll down slightly to edit the newly created security definition.

	Set it to have the following properties:
	
	> Name: `oauth`
	
	> Description: `Resource Owner Password Grant Type`
	
	> Flow: `Password`
	
	> Token URL: `https://api.think.ibm/demo/sb/oauth20/token`

1. Click the `+` icon in the **Scopes** section to create a new scope. Set the following properties:

	> Scope Name: `inventory`
	
	> Description: `Access to all inventory resources`
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/inv-oauth.png)

1. Navigate to the `Security` section and check the `oauth (OAuth)` checkbox.  

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/inv-security-addoauth.png)
	
	Now that the API is secured using our OAuth provider, we can define how the API should behave when called. In the next two sections, we will configure the `inventory` API to call our inventory application which was published at the end of Lab 3.

## 4.4 - Adding API Properties

Before we add and configure the `inventory` API processing policies, we need to set up a couple of API properties. These are used to hold parameters needed by the API, whose values vary based on which Catalog the API is running in.

For example, we will configure the `inventory` API to invoke a backend service. We want to be able to call different instances of that service based on which Catalog the API is deployed to.

1. Navigate to the `Properties` section and click the `+ Add` icon to create a new property. Give it the following attributes:

	> Name: `app-server`
	
	> Description: `Catalog-specific app server URL`
	
	> Default Value: `http://localhost/`

1. Add additional catalog-specific values by clicking the `Add value` link. Specify the following catalog name and value:

	> Catalog: `Sandbox`
	
	> Value: `http://appsvr.think.ibm`
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/inventory_appserver_property.png)

	> ![][important]
	> 
	> The Catalog and Value fields are strict. They are case-sensitive and must be typed exactly as shown.  You will note that the catalog drop down list will have a default entry there for `sb`. It is important that you type in the `Sandbox` name in the Catalog as indicated above, otherwise you will encounter issues.

1. Create another property by clicking on the `+` icon, named `app-id`:

	> Name: `app-id`

	> Description: `Inventory application id`

	> Default Value: `null`

1. Click on the Notepad icon in the top-righthand corner of the OS task bar.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/notepad.png)

1. Copy the `Host header` from the notepad.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/copy_app-id.png)

1. Add a new value to the `app-id` property:

	> Catalog: `Sandbox`
	
	> Value: paste the value of the `app-id` here
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/inventory_appid_property.png)

	> ![][troubleshooting]
	> 
	> The UI may inadvertently add another value after you paste in the app-id. If this occurs, click the trashcan icon to remove the unwanted entry.

1. Save your changes.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/save-icon.png)

## 4.5 - Defining API Processing Behavior
An API Assembly provides collection of policies which are enforced and executed on the API Gateway. Policies include actions like modifying the logging behavior and altering the message content or headers. Additionally, if the out of the box policies do not meet your specific needs, you may opt to create your own policy and have it available for API designers through the API Connect UI.

1. Switch to the `Assemble` tab. A simple assembly has been created for you.  

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/inv-assembly-start.png)

1. Modify your assembly to use DataPower Gateway policies.

	Expand the `Filter` menu by clicking the menu icon (to the right of the `Filter` label).
	
	Select the `DataPower Gateway policies` radio button.
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/filter-datapowerpolicies.png)

1. Add an `activity-log` policy to the assembly and configure it to log API payload.  

	Drag the `activity-log` policy from the list of available policies to the left of the `invoke` policy already created in your assembly.

1. Select the newly added `activity-log` step. A properties menu will open on the right of your screen.

	Under `Content` select `payload` from the drop-down list.  
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/inv-assembly-logpayload.png)

1. Click on the `X` to close the activity-log editor menu.

1. Add a `set-variable` policy to the assembly, to the right of the activity-log step.

1. Click on the `set-variable` policy to access the editor and set the `Host` header for our request. The `Host` header is used by the liberty runtime to properly route the request to our Inventory application.

1. Click the `+ Action` button.

	Edit the action properties to set the header value:
	
	> Action: `Set`
	
	> Set: `message.headers.Host`
	
	> Type: `string`
	
	> Value: `$(app-id)`
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/set_app-id_host_header.png)

1. Click on the `X` to close the set-variable editor menu.

1. Click on the `invoke` policy to bring up the editor.

1. Replace the default `Invoke URL` path.

	Update the URL to be: `$(app-server)$(request.path)`
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/invoke_url.png)
	
	> ![][info]
	> 
	> The `$(request.path)` variable will automatically remove the organization and catalog components of the full API Connect request path. Additionally, the leading slash `/` is already included in the variable value.

1. Scroll down to the bottom of the editor screen. Set the `Response object variable` to `message`.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/invoke_rsp_var.png)

1. Click on the `X` to close the invoke policy editor menu.

1. Save your changes.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab4/save-icon.png)

# Lab 4 - Validation

Using both the CLI and the API Designer, a lot of files have been created and updated. Before moving on to the next lab, a simple script has been provided for you which will validate your source files against those of a completed lab. These next steps will help ensure that easily made typing errors do not cause problems down the line.

1. From the `Terminal Emulator`, type:

	```bash
	validate_lab 4
	```
	
1. The script will execute a series `diff` commands against specific files in your project folder (`~/ThinkIBM/inventory`)

1. If the output of the `validate_lab` script includes discrepencies, you may merge the corrected changes into your source files by typing:

	```bash
	merge_lab 4
	```

# Lab 4 - Conclusion

**Congratulations!** You have successfully configured and secured the inventory API. You will consume this API in a later step.

Proceed to [Lab 5 - Advanced API Assembly](../Lab%205%20-%20Advanced%20API%20Assembly)

[important]: https://github.com/ibm-apic-pot/lab-guide/raw/master/img/common/important.png "Important!"
[info]: https://github.com/ibm-apic-pot/lab-guide/raw/master/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apic-pot/lab-guide/raw/master/img/common/troubleshooting.png "Troubleshooting"
