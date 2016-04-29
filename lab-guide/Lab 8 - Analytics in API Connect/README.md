# Lab 8	- Analytics in API Connect

In this lab, you will gain a high level understanding of how analytics are used to visualize the information captured by the gateway node. You can filter, sort and aggregate your API event data; then present the results within correlated charts, tables and maps to help you manage service levels, set quotas, establish controls, set up security policies, manage communities and analyze trends.

API Analytics is built on the Kibana V4.3 open source analytics and visualization platform, which is designed to work with the Elastic Search real-time distributed search and analytics engine.

---
# Lab 8 - Objective
 
In this lab, you will learn:

+ How to view the analytics for a catalog inside of API Manager
+ How to create a custom dashboard
+ How to add visualizations to a dashboard 
+ How to customize and arrange visualizations in the dashboard

---
# Lab 8	- Case Study Used in this Tutorial

In this tutorial, you will simulate a good amount of traffic passing through the API Connect platform by executing a script that will invoke a series of `curl` commands that will hit a dummy API. Upon that script finishing, you will go into the API Manager and get some hands on experience with customizing a dashboard and viewing the analytics for traffic run on the system.

---
# Lab 8	- Step by Step Lab Instructions

# 8.1	- Stage the API Calls to Populate the Analytics Data in API Connect

1. If not already open, launch the `Firefox Web Browser` from the favorites menu.

1. Click on the `API Manager` bookmark.

1. Log in to the `API Manager` server with the following credentials:

	> Username: `student@think.ibm`
	
	> Password: `Passw0rd!`

1. Use the menu icon to open the navigation panel:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/api-mgr-menu.png)

1. Select the `Drafts` option from the menu:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/api-mgr-menu-drafts.png)

1. From the top menu select the `APIs` link:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/api-mgr-draft-apis.png)
 
1. Click the `+ Add` button and then `Import Swagger 2.0`: 

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/api-mgr-add-via-swagger.png)

1. Click the `Select File` button to open the file explorer.

1. Navigate to the `/home/student/lab_files/lab8` directory and chose the `load-student_1.0.0.yaml` file.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/open-sample-api-yaml.png)

1. Click the `Import` button to import the API definition.

1. Click on the `Products` link in the top menu.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/api-mgr-draft-products.png)

1. Click the `+ Add` button. In the drop menu, chose to to create a `New Product`:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/api-mgr-new-product.png)

1. Complete the new product form using the following details:

	> Title: `load-student`
	
	> Name: `load-student`
	
	> Version: `1.0.0`
 
1. Navigate to the `APIs` section of the Product screen and click the `+` button.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/product-add-api.png)

1. Select the `load-student` API and then click the `Apply` button.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/product-select-api-to-add.png)

1. Save the product:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/product-save.png)

1. Stage the Product by clicking the `cloud` icon, then select the `Sandbox` catalog:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/product-stage.png)
	
1. When complete you will see the output message that the staging was successful.

1. Use the menu icon at the top-left of the screen to return back to the `Dashboard`.

1. Click on the `Sandbox` catalog tile:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/api-mgr-dashboard-sandbox-tile.png)

1. Click the `options` icon to the right of the `load-student` product:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/product-options-icon.png)

1. Select `Publish` from the drop menu.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/product-options-publish.png)

1. Click the `Publish` button once the **Edit Visibility and Subscriber** window appears.

	The API is now subscribable on the portal.
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/image12.png)

# 8.2	- Subscribe to the API from the Developer Portal

1. If the Developer Portal tab is still open, select it for focus. Otherwise, open a new browser tab and chose the `Portal` bookmark.

1. If you're not already logged in to the Developer Portal, click the `Login` link and enter the following credentials:

	> Username: `developer@consumer.ibm`
	
	> Password: `Passw0rd!`

1. Once logged in, select the `API Products` link from the navigation menu.

	You should see the `load-student` product avaialble:
	
	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/image13.png)
	
1. Click the `load-student` Product and then click on the `Plans` link in the left side panel.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/image14.png)

1. Click the `Subscribe` button under the **Default** plan.

1. When the subscribe box appears, select your app via the radio button and then click the `Subscribe` button.

# 8.3	- Run the Analytics Load Script

1. Launch the `Notes` application by clicking on the notepad icon in the task bar.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/notepad.png)

1. Highlight your application's client_id and copy it to the clipboard.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/copy-client-id.png)

1. Use the application favorites menu to open a fresh `Terminal Emulator` window.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/launch-teminal-emulator.png)

1. To change into the `~/lab_files/lab8` directory, type:

	```bash
	cd ~/lab_files/lab8
	```

1. Execute the analytics load script by typing:

	```bash
	./analytics.sh <client_id>
	```
	
	Where `<client_id>` is the Client ID for your consuming application. Paste your client_id into the terminal by using the `Edit > Paste` menu option, or **right-mouse-click** and select `Paste` from the menu.
	
	Your client_id will be different than the screenshot below.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/run-analytics-script.png)

1. The script will invoke the dummy API numerous times to give us some data to look at inside the analytics engine.

# 8.4	- Create a Dashboard in API Connect

1. Return to the `Firefox Web Browser` by clicking the app in the task bar.

1. From the `Sandbox` catalog configuration screen, click on the `Analytics` tab:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/analytics-tab.png)

1. The default dashboard gives some general information like the 5 most active Products and 5 most active APIs.  This information is interesting, but we can see much more information by customizing the dashboard. Add a new visualization by clicking on the `+ Add Visualization` icon:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/analytics-add-visualization.png)

1. This will bring a list of some of the standard visualizations. You can then type in a string to filter through visualizations or use the arrows to page through the list.

1. Add the `API Calls` visualization to the dashboard by simply clicking on it. The new visualization will be added to the bottom of our dashboard.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/image21.png)

1. Scroll down to find the new visualization. You can adjust the size by clicking and dragging the border from the lower right. Additionally, you can adjust its position by clicking and dragging the box to where you want it.

	Move the `API Calls` box in between the 5 Most Active Products and 5 Most Active API windows.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/image22.png)
	
1. Feel free to play around with the other visualizations by adding them to the Dashboard. You can also save the dashboard by clicking on the `Save Dashboard` button:

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/analytics-save-dashboard.png)

1. There are also several out of the box Dashboards that you can play with by clicking on the `Load Saved Dashboard` icon. Go ahead and open the `api default` Dashboard.

	![](https://github.com/ibm-apic-pot/lab-guide/raw/master/img/lab8/analytics-load-dashboard.png)

1. Here you will see some interesting visualizations that show graphs and charts with information about the API Traffic that was processed.

---
# Lab 8 - Completion

**Congratulations!** You have completed all of the labs!

[important]: https://github.com/ibm-apic-pot/lab-guide/raw/master/img/common/important.png "Important!"
[info]: https://github.com/ibm-apic-pot/lab-guide/raw/master/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apic-pot/lab-guide/raw/master/img/common/troubleshooting.png "Troubleshooting"