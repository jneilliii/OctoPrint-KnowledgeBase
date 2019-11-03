- [OctoPrint UI-Sections](#octoprint-ui-sections)
- [Plugin Names](#plugin-names)
- [How does the UI-Communication work](#how-does-the-ui-communication-work)
- [ViewModel Callbacks](#viewmodel-callbacks)
- [Knockout in Octoprint](#knockout-in-octoprint)
- [Available ViewModels](#available-viewmodels)
- [Hijack Print/Resume-Button](#hijack-printresume-button)
- [Create Modal-Dialog](#create-modal-dialog)
- [Spinning Button](#spinning-button)
- [ForEach / ObservableArray](#foreach)
- [Table](#table)
- [UI Libraries](#ui-libraries)

# Octoprint UI-Sections
The UI of Octoprint is seperated in the following areas. Each area could be adapted with a Jina2-Template.

Just use the following namingconventions (path: ```/templates/'yourpluginId'_'area'.jinja2```)

Areas
* <plugin_identifier>_tab.jinja2			-> Tab
* <plugin_identifier>_navbar.jinja2		    -> Header
* <plugin_identifier>_sidebar.jinja2		-> Left sidebar
* <plugin_identifier>_settings.jinja2		-> Settings

# Plugin Names
The Name of a plugin is used in different contexts: Plugin-Repository, Plugin-Manager, Tab-Name (if used)
(I am not talking about the Plugin-Identifier!!!)

|Location|Description|
|-------------|-------------|
| setup.py <br>```plugin_name = "PrintJobHistory"```||
| init.py <br>```__plugin_name__ = "Job History""```|Used in Settings menu on the left side| 
| init.py <br>```def get_template_configs(self):```<br>```...```<br>```dict(type="tab", name="History")```|Tab-Name|

# How does the UI-Communication work
For Server-Side rendering http://jinja.octoprint.org/ is used.

E.g. if you want to include HTML-Snippets you can use this:

```
{% include "snippets/settings/server/serverOnlineCheckHost.jinja2" %}
```
or for I18N-Support you can use this:
```
<p>{% trans %}Hello {{ user }}!{% endtrans %}</p>
```
Octoprint use https://knockoutjs.com/ for UI-Widget to JavaScript-Model Binding (2way binding)

![OverviewExample](images/knockout-example.png)
Source: https://knockoutjs.com/

# ViewModel Callbacks

JavaScript call backs like:
```javascript
        // receive data from server
        self.onDataUpdaterPluginMessage = function (plugin, data) {

            if (plugin != PLUGIN_ID) {
                return;
            }
            debugger
        }

        self.onBeforeBinding = function() {
           ...
        }

onAfterTabChange(current, previous)
onSettingsBeforeSave()
...
```

https://docs.octoprint.org/en/master/plugins/viewmodels.html#callbacks

# Knockout in Octoprint
1. Create JavaScript file and add it to the getAsset()-Section in ```__init__.py```

    Minimal JavaScript-File: _pluginId_.js

    Sample: _PauseAt.js_

    ```javascript
    $(function() {
        function PauseAtViewModel(parameters) {
            var self = this;

            // assign the injected parameters, e.g.:
            self.loginStateViewModel = parameters[0];
            self.settingsViewModel = parameters[1];

            // TODO: Implement your plugin's view model here.
        }

        /* view model class, parameters for constructor, container to bind to
        * Please see http://docs.octoprint.org/en/master/plugins/viewmodels.html#registering-custom-viewmodels for more details
        * and a full list of the available options.
        */
        OCTOPRINT_VIEWMODELS.push({
            construct: PauseAtViewModel,
            // ViewModels your plugin depends on, e.g. loginStateViewModel, settingsViewModel, ...
            dependencies: [  "loginStateViewModel", "settingsViewModel"  ],
            // Elements to bind to, e.g. #settings_plugin_PauseAt, #tab_plugin_PauseAt, ...
            elements: [
                document.getElementById("pauseat_plugin_settings"),
                document.getElementById("pauseat_plugin_sidebar")
            ]
        });
    });
    ```

2. Simple Textfield-Binding
   in your template:
   ```html
    ...
    <input type="text" name="myTextField" data-bind="textInput: myTextFieldValue">
    ...
   ```
   In your PluginId.js
   ```javascript
    ...
    self.myTextFieldValue = ko.observable();
    ...
   ```
    List of all knockout-bindings: https://knockoutjs.com/documentation/text-binding.html


# Available ViewModels
In the above example we saw some injected viewmodels, e.g. loginStateViewModel, here you can find an overview of all viewmodels:
http://docs.octoprint.org/en/master/plugins/viewmodels.html

Also on this page there are all "Event-Hooks" listed. E.g.:
   ```javascript
    ...
    // Data send from backend to frontend
    self.onDataUpdaterPluginMessage = function(plugin, data) {
    if (plugin != PLUGIN_ID) {
        return;
    }
    ...
   ```


# Hijack Print/Resume-Button
```javascript
    const startPrint = self.printerStateViewModel.print;
    self.printerStateViewModel.print = function confirmSpoolSelectionBeforeStartPrint() {
        alert("pre vaildate stuff");
        // printAllowed = showDialog();
        startPrint();
    };

    const resumePrint = self.printerStateViewModel.resume;
    self.printerStateViewModel.resume = function confirmSpoolSelectionBeforeResumePrint() {
        // do you resume stuff
        resumePrint();
    };
    
    ....
    dependencies: [  
        ...
        "printerStateViewModel"  
    ],
```

# Create Modal-Dialog
You need two parts:
1. HTML-Template for the Dialog-Content
```html
<!-- Modal-Dialog -->
<div id="sidebar_simpleDialog" class="modal hide fade">
    <div class="modal-header">
        <a href="#" class="close" data-dismiss="modal" aria-hidden="true">&times;</a>
        <h3 class="modal-title">This is the Dialog-Title</h3>
    </div>
    <div class="modal-body">
        <h4>Dialog-Content</h4>
    </div>
    <div class="modal-footer">
        <button class="btn btn-cancel" data-dismiss="modal" aria-hidden="true">Cancel</button>
        <button class="btn btn-danger btn-confirm">Confirm</button>
    </div>
</div>
```
2. JavaScript function to show/hide dialog
```javascript
showDialog("#sidebar_simpleDialog", function(dialog){
    // printAllowed = showDialog();
    startPrint();
    dialog.modal('hide');
});

...

function showDialog(dialogId, confirmFunction){
        // show dialog
        // sidebar_deleteFilesDialog
        var myDialog = $(dialogId);
        var confirmButton = $("button.btn-confirm", myDialog);
        var cancelButton = $("button.btn-cancel", myDialog);
        //var dialogTitle = $("h3.modal-title", editDialog);

        confirmButton.unbind("click");
        confirmButton.bind("click", function() {
            alert ("Do something");
            confirmFunction(myDialog);
        });
        myDialog.modal({
            //minHeight: function() { return Math.max($.fn.modal.defaults.maxHeight() - 80, 250); }
        }).css({
            width: 'auto',
            'margin-left': function() { return -($(this).width() /2); }
        });
}
```


# Spinning Button

    <i class="fa fa-spinner fa-spin" data-bind="enabled:!requestInProgress(), css: {disabled: requestInProgress()}, visible: requestInProgress()"></i> 

```javascript
    self.requestInProgress(true);
    $.ajax({
        url: API_BASEURL + "plugin/"+PLUGIN_ID,
        type: "POST",
        dataType: "json",
        data: JSON.stringify({
            command: "deleteConfirmed",
        }),
        contentType: "application/json; charset=UTF-8"
    }).done(function(data){

        //alert("back from server");
        editDialog.modal("hide");
    }).always(function(){
        self.requestInProgress(false);
    }) ;
```

# ForEach
1. Create Item-Model with update-function for a row
2. Create observable array and add some items to the array
3. Attach jquery-fontpicker functionality to all rows
4. Implement sync between jquery-fontpicker and observable items 

```
        // Single Item-Model
        IconItem = function(data) {
            this.iconName = ko.observable()
            // Fill Item with (initial) data
            this.update(data);
        }
        // Update the Item-Function
        IconItem.prototype.update = function (data) {
            var updateData = data || {}

            this.iconName = ko.observable(updateData.iconName)
        }

        // collects all IconItems
        self.allIcons = ko.observableArray();

        // fill icon-collection with dummy values (or a loop or a complete data-array)
        self.allIcons.push(new IconItem({
            iconName: "fab fa-500px"
        }));
        self.allIcons.push(new IconItem({
            iconName: "fas fa-ad"
        }));

        //        
        self.onAfterBinding = function() {
            // all inits / loop-rendering were done

            // init jquery-iconpicker
            allIconPickers = $('.supericonpicker').iconpicker();
            // attach selection listener for all iconPickers
            allIconPickers.each(function(index, item){
                $(item).on('iconpickerSelected', function(event){
                  newIcon = event.iconpickerValue;
                  // get IconItem-Model and fill the selection
                  iconItemModel = self.allIcons()[index];
                  iconItemModel.iconName(newIcon);
                });
            });
        }

Multi-IconPicker
<div data-bind="foreach: allIcons">
  <div class="input-prepend input-block-level">
    <span class="add-on" style="width: 25px;"><i class="fa-lg" data-bind="css: iconName"></i></span>
    <input type="text" class="supericonpicker input-large text-right" data-bind="textInput: iconName"/>
  </div>
</div>
```

The example does not include the saveAll-feature, because it is just converting the array to json and send the data to the server, like this:
```
ko.toJSON(self.allIcons()); 
--> "[{"iconName":"fab fa-500px"},{"iconName":"fas fa-ad"}]"
```

# Table
How to create a table with paging, sorting and filtering (all client-sie based)

* Create a layout
```html
    <table id="printjobhistory_main_table" class="table table-striped table-hover table-condensed table-hover" style="clear: both;">
        <thead>
            <tr>
                <th style="width: 43%">Filename</th>
...
            </tr>
        </thead>
        <tbody data-bind="foreach: printJobHistorylistHelper.paginatedItems">
            <tr>
                <td>
                    <span data-bind="text: fileName, attr: { title: fileName }"></span>
                    <div class="additionalInfo">
                        <div data-bind="visible: note">Note: <span data-bind="text: note"></span></div>
                        <div data-bind="visible: spool">Spool: <span data-bind="text: spool"></span></div>
                        <div data-bind="visible: user">User: <span data-bind="text: user"></span></div>
                    </div>
                </td>
...
            </tr>
        </tbody>
    </table>

    <div class="pagination pagination-mini pagination-centered">
        <ul>
            <li data-bind="css: {disabled: printJobHistorylistHelper.currentPage() === 0}">
                <a href="#" data-bind="click: printJobHistorylistHelper.prevPage">«</a>
            </li>
        </ul>
        <ul data-bind="foreach: printJobHistorylistHelper.pages">
            <li data-bind="css: { active: $data.number === $root.printJobHistorylistHelper.currentPage(), disabled: $data.number === -1 }">
                <a href="#" data-bind="text: $data.text, click: $root.printJobHistorylistHelper.changePage.bind($data, $data.number)"></a>
            </li>
        </ul>
        <ul>
            <li data-bind="css: {disabled: printJobHistorylistHelper.currentPage() === printJobHistorylistHelper.lastPage()}">
                <a href="#" data-bind="click: printJobHistorylistHelper.nextPage">»</a>
            </li>
        </ul>
    </div>
```

* Create table-model
```javascript
        var printHistoryJobItems = [];
        var item = {
            "fileName" : "hallo.gcode",
...
        }
        ;
        printHistoryJobItems.push(item);
```

* Create ListItemHelper for paging, sorting and filtering
```javascript
todo



```
* Create several functions for interaction

# Usefull 3rd party libs
* https://github.com/itsjavi/fontawesome-iconpicker

# UI Libraries used by OctoPrint V1.3.10
* [Bootstrap 2.3.2](https://getbootstrap.com/2.3.2/scaffolding.html#gridSystem)    
* [Fontawsome 3.2](https://fontawesome.com/v3.2.1/examples/)
* JQuery 2.2.4
* [Lodash 3.10.1](https://lodash.com/docs/3.10.1)
* SockJS to 1.1.1