# Lab 3 - Customize and Deploy the Inventory Application

In this lab, you will add capabilities into the LoopBack application which was created in Lab 2. You will add custom javascript code which will alter the default behavior of the application. Once your edits are complete, you will package the application and publish it to a WebSphere Liberty runtime collective where it will be managed and enforced by the API Connect solution.

---
# Lab 3 - Objective

In the following lab, you will learn:

+ About LoopBack remote hooks
+ How to create a remote hook
+ How to publish a LoopBack application to a Liberty runtime collective

---
# Lab 3 - Case Study Used in this Tutorial

At this point, you have: created a basic application template, added an `item` data model backed by a MySQL datasource, added a `review` data model backed by a MongoDB data source, added a relationship between the `item` and `review` models.

In this tutorial you will extend the `inventory` application by adding a remote hook. Remote hooks allow you to provide pre- and post-processing to an API call, such as adding additional header information to a remote service or calculating a value, which is what you will do in this lab.

Then, you will publish your LoopBack Inventory application to the Liberty Collective. Making it generally available for consumption.

For more information on [Liberty Collectives](https://www.ibm.com/support/knowledgecenter/SSD28V_8.5.5/com.ibm.websphere.wlp.nd.doc/ae/cwlp_collective_arch.html) see here:

<https://www.ibm.com/support/knowledgecenter/SSD28V_8.5.5/com.ibm.websphere.wlp.nd.doc/ae/cwlp_collective_arch.html>

---
# Lab 3 - Step by Step Lab Instructions

## Lab 3.1 - Edit the Application Configuration

Before publishing the API for our application, the configuration file that was generated for you needs to be edited. By default, the generated application uses a base path of `/api`. In the next few steps, you will modify the base path to listen on `/inventory`.

1. Click on the application favorites menu and open the `Atom` text editor.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/launch-atom.png)

1. From the `Atom` menu, click on `File > Open Folder`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/atom-open-folder.png)

1. Click on the `student` location from the Places menu, then navigate to the `ThinkIBM > inventory` folder and click the `OK` button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/atom-open-folder-inventory.png)

	> ![][info]
	> 
	> LoopBack applications use a series of configuration files which drive the application runtime. For more information about these files, review the table in Lab 1.

1. From the folder tree menu, expand the `server` folder and click on the `config.json` file to view the source.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/atom-open-config.png)

1. Edit line 2 of the `config.json` file. Change `/api` to `/inventory`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/atom-edit-config.png)

1. Use the `Atom` file menu to save the changes.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/atom-save-changes.png)

## Lab 3.2 - Create a Remote Hook

Remote hooks are custom javascript code that execute before or after calling an operation on a LoopBack application.

For more information on Remote Hooks please see:

<https://docs.strongloop.com/display/public/LB/Remote+hooks>

1. In the `Atom` editor, expand the directory structure for the `common/models` location and select the `item.js` file.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/atom-item-file1.png)

1. You are going to update this file to include a new remote hook function which will run *after* a new review is submitted for an item. The function will take an average of all reviews for that item, then update the item rating in the MySQL data source.

1. From the `Atom` menu, click on `File > Open Folder`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/atom-open-folder.png)

1. Click on the `student` location from the Places menu, then navigate to the `lab-files` folder and click the OK button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/atom-open-lab-files.png)
	
1. Expand the `lab-files/lab3` folder and select the example `item.js` file.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/item-file.png)
	
1. Use the menu option for `Edit > Select All` to highlight all of the text.

1. Use the menu option for `Edit > Copy` to copy the file contents your clipboard.

1. Return to the other `Atom` application. **Remove** everything in the `item.js` file. Then paste (`control+v` or `Edit > Paste`) the contents of your clipboard to update the file.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/atom-item-file2.png)

	```javascript
	module.exports = function (Item) {
	  Item.afterRemote('prototype.__create__reviews', function (ctx, remoteMethodOutput, next) {
	    var itemId = remoteMethodOutput.itemId;
	    
	    console.log("calculating new rating for item: " + itemId);
	
	    var searchQuery = {include: {relation: 'reviews'}};
	
	    Item.findById(itemId, searchQuery, function findItemReviewRatings(err, findResult) {
	      var reviewArray = findResult.reviews();
	      var reviewCount = reviewArray.length;
	      var ratingSum = 0;
	
	      for (var i = 0; i < reviewCount; i++) {
	        ratingSum += reviewArray[i].rating;
	      }
	
	      var updatedRating = Math.round((ratingSum / reviewCount) * 100) / 100;
	
	      console.log("new calculated rating: " + updatedRating);
	
	      findResult.updateAttribute("rating", updatedRating, function (err) {
	        if (!err) {
	          console.log("item rating successfully updated");
	        } else {
	          console.log("error updating rating for item: " + err);
	        }
	      });
	      next();
	    });
	  });
	};
	```

	> ![][info]
	> 
	> You can scroll to the right within the `Atom` text editor to view the code comments describing what each line is doing.

1. Use the `File > Save` menu option to save the changes.

## Lab 3.3 - Publish App to a Collective

In this section, you will publish the `inventory` application to a Liberty runtime collective for general consumption.

### 3.3.1 - Register the App with API Connect

1. Use the favorites menu to launch the `Firefox Web Browser`:

	  ![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/launch-firefox.png)

1. Click on the `API Manager` bookmark:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/api-mgr-bookmark.png)

1. Enter the following credentials and then click the the `Sign in` button:

	Username: `student@think.ibm`
	
	Password: `Passw0rd!`
	
	> ![][info]
	> 
	> Username and Password may already be saved for you by the browser, you can re-use these saved credentials.
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/api-mgr-login.png)
	
1. Now that the API Manager Dashboard is open, click the `+ Add` button and select `App` from the list:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/api-mgr-add-app.png)

1. Fill out the `Add App` form with the following details:

	> Display Name: `Inventory`
	
	> Name: `inventory`
	
	> Collective: `AppSvr`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/api-mgr-app-info.png)

1. Click the `Add` button to link the application between our API Connect server and the Liberty Collective server. This step creates a registration that allows app management from API Connect once the application is published.

1. Click on the user profile icon and select `Log Out`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/api-mgr-signout.png)

1. Close the Firefox browser by clicking the `x` on the tab or browser window.

### 3.3.2 - Configure the Developer Toolkit to Communicate with API Connect

1. Return to your `Terminal Emulator` session, or open a new one if you had closed it previously.

1. Ensure you are in the `~/ThinkIBM/inventory` project folder by typing the following command:

	```bash
	cd ~/ThinkIBM/inventory
	```

1. Launch the API Designer:

	```bash
	apic edit
	```

1. Click the `Publish` icon.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/publishButton.png)

1. Select `Add and Manage Targets` from the menu.

1. Select `Add a different target`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/add-target.png)

1. Provide connection information to sign into the IBM API Connect management server, then click the `Sign in` button:

	> API Connect host address: `mgr.think.ibm`
	
	> Username: `student@think.ibm`
	
	> Password: `Passw0rd!`  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/publish-target-signin.png)

1. On the "Select an organization and catalog" screen, choose the `Sandbox` catalog and click the `Next` button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/publish-sandbox-catalog.png)

1. Select the `inventory` application, then click `Save`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/select-inventory-app.png)

### 3.3.3 - Publish the Application

1. Click `Publish` button once more and select our target, indicated by the grey highlighting:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/publish-target.png)

1. Click the check box to select `Publish application`, then click the `Publish` button.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/publish-application.png)
	
	> ![][info]
	> 
	> The Developer Toolkit will package up our Node.js StrongLoop application and deploy it to the runtime collective.
	> 
	> We used the web UI in the previous few steps to publish the application. However, we could have also used the API Designer CLI to accomplish the same task.

1. Wait for the application publish process to complete:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/publish-app-success.png)

1. Close the Firefox browser by clicking the `x` on the tab or browser window.

1. Return to the terminal editor. Stop the API Designer process:

	```bash
	control+c
	```

1. Notice that logs were generated during the application publishing process:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/app-published.png)

1. You will need the `host` header that is returned in the next lab.

	**Highlight** the `host header:` value and then **right-mouse-click** to show the menu and select `Copy`.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/app-copy-host.png)
	
1. Open the `Notes` application by clicking on the notepad icon in the task bar:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/launch-notepad.png)
	
1. Paste (use the `control+v` keyboard command) the host header into the `Notes` window. Add a label so that you know what the value is:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/lab3/notes-save-host.png)

# Lab 3 - Validation

Using both the CLI and the API Designer, a lot of files have been created and updated. Before moving on to the next lab, a simple script has been provided for you which will validate your source files against those of a completed lab. These next steps will help ensure that easily made typing errors do not cause problems down the line.

1. From the `Terminal Emulator`, type:

	```bash
	validate-lab 3
	```
	
1. The script will execute a series `diff` commands against specific files in your project folder (`~/ThinkIBM/inventory`)

1. If the output of the `validate_lab` script includes discrepencies, you may merge the corrected changes into your source files by typing:

	```bash
	merge-lab 3
	```

# Lab 3 - Conclusion

In this lab you learned:

+ How to modify the LoopBack application configuration
+ How to create a remote hook in a LoopBack application
+ Publish a LoopBack application to the APIC Collective

Proceed to [Lab 4 - Configure and Secure an API](../Lab%204%20-%20Configure%20and%20Secure%20an%20API)

[important]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/common/important.png "Important!"
[info]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/5010/lab-guide/img/common/troubleshooting.png "Troubleshooting"

