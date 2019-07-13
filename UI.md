- [Octoprint UI-Sections](#octoprint-ui-sections)
- [How does the UI-Communication work](#how-does-the-ui-communication-work)
- [Knockout in Octoprint](#knockout-in-octoprint)
- [Available ViewModels](#available-viewmodels)
- [Hijack Print/Resume-Button](#hijack-printresume-button)
- [Create Modal-Dialog](#create-modal-dialog)
- [Spinning Button](#spinning-button)
- [UI Libraries](#ui-libraries)


# Octoprint UI-Sections
The UI of Octoprint is seperated in the following areas. Each area could be adapted withj a Jina2-Template.

Just us the following namingconventions (path: ```/templates/'yourpluginId'_'area'.jinja2```)

Areas
* <plugin_identifier>_tab.jinja2			-> Tab
* <plugin_identifier>_navbar.jinja2		    -> Header
* <plugin_identifier>_sidebar.jinja2		-> Left sidebar
* <plugin_identifier>_settings.jinja2		-> Settings

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

# Knockout in Octoprint
1. Create JavaScript file and add it to the getAsset()-Section in ```__init__.py```

    Minimal JavaScript-File: PluginId.js

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
        editDialog.modal({
            //minHeight: function() { return Math.max($.fn.modal.defaults.maxHeight() - 80, 250); }
        }).css({
            width: 'auto',
            'margin-left': function() { return -($(this).width() /2); }
        });
}
```


# Spinning Button

    <i class="fa fa-spinner fa-spin" data-bind="enabled:!requestInProgress(), css: {disabled: requestInProgress()}, visible: requestInProgress()"></i> 


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

# UI Libraries
* [Bootstrap 2.3.2](https://getbootstrap.com/2.3.2/scaffolding.html#gridSystem)    
* JQuery 2.2.4
* [Lodash 3.10.1](https://lodash.com/docs/3.10.1)
* SockJS to 1.1.1