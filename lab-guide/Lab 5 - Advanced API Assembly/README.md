# Lab 5 - Advanced API Assembly

In this lab, you will learn how to create advanced API assemblies. Using the graphical design tools and source editor, you will create a new API called `financing` which will expose an existing SOAP service as a RESTful API. Additionally, you will create another API called `logistics` which connects to existing public services and uses the assembly tools to map responses into a desired format.

---
# Lab 5 - Objective

In the following lab, you will learn:

+ How to create a new API, including object definitions and paths
+ How to configure an API to access an existing SOAP service
+ How the source editor can be used to import an existing API definition
+ How to map data retrieved from multiple API calls into an aggregate response
+ How to use gatewayscript directly within an API assembly

---
# Lab 5	- Case Study Used in this Tutorial

In this tutorial, you will expand the product offerings for **ThinkIBM**. In addition to the Inventory API, **ThinkIBM** wishes to provide API's that offer financing and shipping logistics to consumer applications. Your goal is to utilize existing enterprise and public assets to create these API offerings.

---
# Lab 5	- Step by Step Lab Instructions

## 5.1 - Create the Financing API (REST to SOAP)

1. If the API Designer screen has not already been launched, open a terminal and start the designer by issuing the following commands:

	```bash
	cd ~/ThinkIBM/inventory
	apic edit
	```

1. Otherwise, click the `All APIs` link to return to the main Designer screen.

### 5.1.1 - Create the API Definition

1. Click on the `+ Add` button and select `API`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_add_new.png)

1. Fill in the form values for the API, then click the `Next` button to continue.

	> Title: `financing`
	
	> Name: `financing`
	
	> Version: `1.0.0`
	
	> Add to an existing product: `inventory 1.0.0`

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_api_info.png)

1. Keep the selected default to `Don't add to a product` and click the `Add` button.

1. API Connect will generate a new swagger definition file for the `financing` API and automatically load the API editor screen. Notice that the API does not contain any paths or data definitions. We will be adding these in the following steps.

1. Click on `Host` from the API editor menu. Remove `$(catalog.host)` from the Host field. We will keep this blank.

	> ![][troubleshooting]
	> 
	> The host field will show a red line indicating that the field is required. You may ignore this message.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_no_host.png)
	
	```
	NOTES: LETS SEE IF THIS IS NECESSARY
	```

1. Click on `Base Path` menu option and set the base path to `/financing`.

	![]()

1. Click on the `Schemes` menu option, or select it from the API editor menu. Enable the `https` scheme.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_schemes.png)

1. Click on the the `Consumes` menu option. For both "Consumes" and "Produces", choose `application/json`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_consumes_produces.png)

1. Next, we need to create the model definition for our new API. These definitions are used in a few places. Their primary role is to serve as documentation in the developer portal on expected input and output parameters; however, they can also be used for data mapping actions. Click on `Definitions` from the API Designer menu.

1. Click the `+` icon in the **Definitions** section to create a new definition. Then, click on `new-definitions-1` to edit the new definition.

1. Edit the `Name` of the definition, set it to `paymentAmount`.

1. The new definition already adds in a sample property called `new-property-1`. Edit the property values:

	> Property Name: `paymentAmount`
	
	> Description: `Monthly payment amount`
	
	> Type: `float`

	> Example: `199.99`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_definition_complete.png)

1. Now that we have a definition, we'll create a path. Click on `Paths` from the API Designer menu.

1. Click on the `+` button to create a new path. The template will generate the path and a GET resource under the path. This is sufficient for our needs, but we could also add other resources and REST verbs to our path if needed.

1. Edit the default path location to be `/calculate`.

	+ Recall that our Base Path for this API is `/financing`. This new path will be appended to the base, creating a final path of `/financing/calculate`.

1. Click on the `GET` operation to expand the configuration options for the resource.

1. Next, we have the option of adding request parameters to the operation. This defines the input to the API request. Since this is a GET request, we'll add the required request parameters to the query component of the URI.

	Click on the `Add Parameter` link to create a new query parameter. Then, select `Add new parameter` from the sub-menu.

1. We're actually going to need three total parameters for this operation, so go ahead and click on the `Add Parameter` link two more times to add the parameter templates.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_parameter_template.png)

1. Edit the parameters to set the values:

	> Name: `amount`
	  
	> Located In: `Query`
	  
	> Description: `amount to finance`
	  
	> Required: *selected*
	  
	> Type: `float`
	  
	> ---
	  
	> Name: `duration`
	    
	> Located In: `Query`
	  
	> Description: `length of term in months`
	  
	> Required: *selected*
	  
	> Type: `integer`
	  
	> ---
	    
	> Name: `rate`
	    
	> Located In: `Query`
	  
	> Description: `interest rate`
	  
	> Required: *selected*
	  
	> Type: `float`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_parameter_complete.png)

1. Next we'll set the schema for the response. Since we already defined the `paymentAmount` definition, we will select it from the drop down list. You will find the `paymentAmount` definition at the top of the list.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_resp_schema.png)

1. Save the API definition.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/save-icon.png)

1. You have completed the creation of the new API definition. The path and model data will be presented to our consumers on the developer portal once it's published.

### 5.1.2 - Build the Financing API Assembly

1. Click on the `Assemble` tab to access the assembly editor.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_assemble_tab.png)

1. Select the `DataPower Gateway policies` filter.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_gateway_policies.png)

1. In the policy list, click and drag the `activity-log` policy to the front of the processing pipeline.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_assembly_pipeline_1.png)

1. Click on the `activity-log` action resting on the pipeline to open the configuration options for the policy.

1. Change the selected item for the Content field from `activity` to `payload`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_activity_log.png)

1. Click the `X` to close the policy editor window to return to the assembly processing pipeline.

1. Now we are going to add the remaining policies required for mapping our REST API into SOAP.

1. Between the `activity-log` and `invoke` policies, add a `map` policy.

1. After the `invoke` policy, add a `gatewayscript` policy and then a `xml-to-json` policy.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_assembly_pipeline_2_temp.png)

1. In order to consume a SOAP-based service from our REST-based API, we need to map the query parameter inputs that we defined as part of the `GET /calculate` operation to a SOAP payload. To do so, click on the `map` policy on our pipeline to open the map editor.

1. Click on the `+` icon to make the editor window fill the screen.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_map_fullscreen.png)

1. On the **Input** column, click on the `pencil` icon to bring up the input editor.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/input-pencil-icon.png)

1. Recall that our GET operation has three required query parameters: `amount`, `duration` and `rate`. Click on the `+ input` button three times to add the entries to the input table.

1. Fill in the values for each of the input parameters:

	> Context variable: `request.parameters.amount`
	
	> Name: `amount`
	
	> Content type: `none`
	
	> Definition: `float`
	
	> ---
	
	> Context variable: `request.parameters.duration`
	  
	> Name: `duration`
	  
	> Content type: `none`
	  
	> Definition: `integer`
	
	> ---
	
	> Context variable: `request.parameters.rate`
	  
	> Name: `rate`
	    
	> Content type: `none`
	    
	> Definition: `float`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_map_inputs.png)

1. Click on the `Done` button to return to the map editor.

1. Next, in the **Output** column, click on the `pencil` icon to add an output definition.

1. Click on `+ output`.

1. Set the `Content type` to `application/xml`.

1. For the `Definition` field, click on the drop down menu and scroll to the bottom to select `Inline schema`.

1. In the interest of time, and to avoid typing errors, a sample schema file has been provided for you.

	Select the `Atom` application from the system task bar if it was left open, or open it using the favorites menu.

1. Make sure you're in the `Atom` instance that is opened to browse the `lab-files` folder. If you need to open the `lab-files` folder, use the file menu and select `File -> Open Folder...` and choose `student -> lab-files`.
	
1. Use the file tree menu to open the `lab5/schema_financingSoap.yaml` file:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/open-schema-financing-soap.png)

1. Copy the contents of the `schema_financingSoap.yaml` file to the system clipboard.

	```yaml
	$schema: 'http://json-schema.org/draft-04/schema#'
	id: 'http://services.think.ibm'
	type: object
	properties:
	  Envelope:
	    type: object
	    xml:
	      prefix: soapenv
	      namespace: 'http://schemas.xmlsoap.org/soap/envelope/'
	    properties:
	      Body:
	        type: object
	        properties:
	          financingRequest:
	            type: object
	            xml:
	              prefix: ser
	              namespace: 'http://services.think.ibm'
	            properties:
	              amount:
	                id: 'http://services.think.ibm/Envelope/Body/financingRequest/amount'
	                type: number
	                name: amount
	              duration:
	                id: 'http://services.think.ibm/envelope/body/financingRequest/duration'
	                type: integer
	                name: duration
	              rate:
	                id: 'http://services.think.ibm/envelope/body/financingRequest/rate'
	                type: number
	                name: rate
	            additionalProperties: false
	            required:
	              - amount
	              - duration
	              - rate
	            name: financingRequest
	        additionalProperties: false
	        required:
	          - financingRequest
	        name: Body
	    additionalProperties: false
	    required:
	      - Body
	    name: Envelope
	additionalProperties: false
	required:
	  - Envelope
	title: output
	```

1. Switch back to the `Firefox` browser and paste (`control+v`) the SOAP schema definition into the schema editor window, then click the `Done` button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_soap_schema.png)

1. Click the `Done` button in the output editor window to return to the map editor.

1. For each of the `Input` query parameters, map them to their respective SOAP `Output` elements.

	To map from an input field to an output field, click the circle next to the *source* element once, then click the circle next to the *target* element. A line will be drawn between the two, indicating a mapping from the source to the target.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_map_to_soap.png)

1. Click the `X` button in the map editor to return to the policy pipeline.

1. Click the `invoke` policy to open its editor.

1. Set the `Invoke URL` to: `https://services.think.ibm:1443/financing/calculate`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_invoke_url.png)

1. Scroll down and set the `HTTP Method` to `POST`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/fin_invoke_method.png)

1. Click the `X` button to return to the policy pipeline.

1. Click on the `gatewayscript` policy to open the editor. Add the following line:

	```javascript
	session.name('_apimgmt').setVar('content-type', 'application/xml');
	```
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/gws-workaround.png)

1. You are now finished with the assembly for the `financing` API. The assembly takes the following actions:

	+ Logs requests.
	+ Maps the REST query parameters into a SOAP body.
	+ Sets the SOAPAction header.
	+ Invokes the SOAP service.
	+ Transforms the SOAP service's response into JSON.

1. Save the API definition.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/save-icon.png)

## 5.2 - Add Logistics API (Advanced Assembly)

In this lab section, we will be adding a new API called `logistics` which will provide helper services around calculating shipping rates and locating nearby stores.
 
Rather than require you to build the entire API from scratch again, you will see how you can use the source editor to paste in an API definition that has already been started for you.  

### 5.2.1 - Import Predefined API Definition

1. Click on the `All APIs` link to return to the main API Designer screen.

1. Click on the `+ Add` button to import a new `OpenAPI (Swagger 2.0)`.

1. Click on the `Select File` button to open the file browser. Navigate to the `~/home/student/lab-files/lab5` folder and choose the `api_logistics.yaml` file.

	![][]

1. Click the `import` button to import the `logistics` API definition template.

	![][]

1. Click on the `logistics 1.0.0` API from the list to browse the API definition.

	![][]

### 5.2.2 - Create the Logistics API Assembly

1. Switch to the `Assemble` tab and click the `Create assembly` button.

1. Add an `activity-log` policy to the assembly pipeline.

1. Click the `activity-log` policy to open editor, configure it to log the `payload` for the **Content** field.

1. Add an `operation-switch` policy to the right of the activity-log step.

1. Click on the `operation-switch` policy to launch the editor. A single case `case 0` is created by default.

1. Click `search operations...` to bring up the drop-down list of available operations.

1. Select the `get /shipping` operation.

1. Click the `+ Case` button to add a second case for the `get /stores` operation.  

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics-operationswitch-cases.png)

1. Click the `X` to close the operation switch configuration editor.

	You should see two new processing pipelines created on your `operation-switch` step - one for each case:  

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics-twopipelines.png)

#### 5.2.2.1 - Configure the `get /shipping` Case:

This operation will end up invoking two separate back-end services to acquire shipping rates for the respective companies, then utilize a map action to combine the two separate responses back into a single, consolidated message for our consumers.

1. Add an invoke policy to the `get /shipping` case with the following properties:

	> Title: `invoke_xyz`
	  
	> Invoke URL: `$(shipping_svc_url)?company=xyz&from_zip=90210&to_zip={zip}`
	  
	> Response object variable (scroll to the bottom): `xyz_response`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics_invokexyz1.png)
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics_invokexyz2.png)
	
	> ![][info]
	>
	> The `{zip}` parameter provided here is a reference to the `zip` parameter defined as input to the operation. The `{zip}` portion of the URL will get replaced by the actual zip code provided by then API consumers.

1. Hover over the `invoke_xyz` policy and click on the `clone` button to add another invoke action:

	![][]

1. Edit the new invoke policy with the following properties:

	> Title: `invoke_cek`
	  
	> Invoke URL: `$(shipping_svc_url)?company=cek&from_zip=90210&to_zip={zip}`
	
	> Response object variable: `cek_response`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics_invokecek1.png)
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics_invokecek2.png)

1. Add a `map` policy after the last invoke, then click it to open the editor.

1. Click the `pencil` icon next to `Input` and specify the following map properties:
  
	> Title: `map_responses`
	  
	> Description: `Map responses from invoke_xyz and invoke_cek to output`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics-map-properties.png)

1. Click the `+ input` button to add an input. Specify the following input configuration:
  
	> Context variable: `xyz_response.body`
	  
	> Name: `xyz`
	  
	> Content type: `application/json`
	  
	> Definition: `Inline schema`

1. After you select `Inline schema`, you will be prompted to "Provide a schema".

	Use the `Atom` editor to open the `/home/student/lab-files/lab5/schema_shippingSvc.yaml` file.
	
	Copy the contents to your clipboard and paste them into the schema editor window.
	  
1. Click `Done` to close the Schema window  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics-map-xyz-schema.png)

1. Click the `+input` button again to add another input. Specify the following input configuration:
  
	> Context variable: `cek_response.body`
	  
	> Name: `cek`
	  
	> Content type: `application/json`
	  
	> Definition: `Inline schema`

1. Paste the same schema definition that was used for the previous input (for our lab purposes, the responses from the shipping service are in the same format, thus using the same schema)

1. You now have two inputs assigned to the `map` policy:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics-map-responses-inputs.png)

1. Click the `Done` button to return to the editor.

1. Click the `pencil` icon next to `Output`, then click the `+ output` button to add an output field with the following properties:

	> Context variable: `message.body`
	  
	> Name: `output`
	  
	> Content type: `application/json`
	  
	> Definition: `#/definitions/shipping`  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics-map-responses-output.png)

1. Click the `Done` button to return to the editor.

1. Complete the mapping. To map from an input field to an output field, click the circle next to the *source* element once, then click the circle next to the *target* element. A line will be drawn between the two, indicating a mapping from the source to the target.  

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics-map-complete.png)

1. Click the `X` to close the map editor.

	Your assembly policy for the `shipping.calc` operation is now complete.
	  
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics-shipping-calc-policy.png)

1. Save your changes.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/save-icon.png)

#### 5.2.2.2 - Configure the `get /stores` Case:

This operation will call out to the Google Geocode API to obtain location information about the provided zip code, then utilize a simple gatewayscript to modify the response and provide a formatted Google Maps link.

1. Add an invoke policy to the `get /stores` case with the following properties:

	> Title: `invoke_google_geolocate`
	  
	> Invoke URL: `https://maps.googleapis.com/maps/api/geocode/json?&address={zip}`
	  
	> Response object variable (scroll to the bottom): `google_geocode_response`  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics_invokegeolocate1.png)
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics_invokegeolocate2.png)

1. Add a `gatewayscript` policy with the following properties:  
  
	> Title: `gws-format-maps-link`
	  
	> Paste the contents of `/home/student/lab-files/lab5/gws_formatMapsLink.js` into the code section of the policy.
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics-gws.png)
	
	> ![][info]
	> 
	> Take a quick look at line 5. Notice how our gateway script file is reading the body portion of the `google_geocode_response` variable which was assigned to the output of the `invoke` action.

1. Click the `X` to close the gatewayscript editor.

1. Your assembly for the `logistics` API will now include two separate operation policies:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/logistics_assembly-complete.png)

1. Save your changes.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab5/save-icon.png)

1. Close the Firefox browser by clicking the `x` on the tab or browser window.

1. Return to your `Terminal Emulator` session.

1. Even though we closed the browser, the API Designer application itself is still running.

	Hold the `control` key and press the `c` key to end the API Designer session:
	
	```bash
	control+c
	```
	
	This will return you to the command line prompt.

# Lab 5 - Validation

Using both the CLI and the API Designer, a lot of files have been created and updated. Before moving on to the next lab, a simple script has been provided for you which will validate your source files against those of a completed lab. These next steps will help ensure that easily made typing errors do not cause problems down the line.

1. From the `Terminal Emulator`, type:

	```bash
	validate-lab 5
	```
	
1. The script will execute a series `diff` commands against specific files in your project folder (`~/ThinkIBM/inventory`)

1. If the output of the `validate_lab` script includes discrepencies, you may merge the corrected changes into your source files by typing:

	```bash
	merge-lab 5
	```

# Lab 5 - Conclusion

**Congratulations!** You have successfully configured two new API's with advanced assemblies. In the next lab, you will bundle the API's into a Product and publish it to the consumer portal.

Proceed to [Lab 6 - Working with API Products](../Lab%206%20-%20Working%20with%20API%20Products)

[important]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/common/important.png "Important!"
[info]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/common/troubleshooting.png "Troubleshooting"